<template>
  <div id="gesture-lock">
    <div class="lock_wp">
      <div class="lock_title"
           :style="[{color: color}]"
      >
        {{title}}
      </div>
      <canvas
        v-if="canvasId"
        :canvas-id="canvasId"
        class="canvasLock"
        :style="[{
        width:canvasWidth+'px',
        height:canvasHeight+'px'
        }]"


        @touchend="onTouchend"
        @touchstart="onTouchstart"
        @touchmove="onTouchmove"
        @touchcancel="onTouchcancel"

        disable-scroll="true"></canvas>
      <button class="lock_reset" @click="updatePassword">重置密码</button>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  /**
   * getDis 获取两点直线距离
   * @param  {Object} a 坐标
   * @param  {Object} b 坐标
   * @return {Number}   距离值
   */
  function getDis (a, b) {
    return Math.sqrt(Math.pow(a.x - b.x, 2) + Math.pow(a.y - b.y, 2))
  }

  const LIMIT = 60
  let prev = 0
  export default {
    props: {
      // canvas画布宽度 px
      canvasWidth: {
        type: Number,
        default: 300
      },
      // canvas画布高度 px
      canvasHeight: {
        type: Number,
        default: 300
      },
      // canvas画布id
      canvasId: {
        type: String,
        default: 'canvasLock'
      },
      // 绘制颜色
      drawColor: {
        type: String,
        default: '#3985ff'
      },
      // 宫格
      chooseType: {
        type: Number,
        default: 3
      }
    },
    data () {
      return {
        title: '',
        color: ''
      }
    },
    mounted () {
      this.init()
    },
    onShow () {
      this.init()
    },
    onUnload () {
      this.title = ''
      this.color = ''
    },
    methods: {
      // 初始化
      init () {
        this.setData()
        this.pswObj = {}

        this.lastPoint = []
        this.touchFlag = false
        this.ctx = wx.createCanvasContext(this.canvasId)
        this.createCircle()
      },
      // 绘图
      createCircle () { // 计算各个点坐标，画圆，根据canvas的大小来平均分配半径
        const n = this.chooseType
        let count = 0
        this.r = this.canvasWidth / (2 + 4 * n) // 计算cricle半径
        const r = this.r
        this.lastPoint = []
        this.arr = [] // 记录9宫格位置
        this.restPoint = []
        for (let i = 0; i < n; i++) {
          for (let j = 0; j < n; j++) {
            count++
            const obj = {
              x: j * 4 * r + 3 * r,
              y: i * 4 * r + 3 * r,
              index: count
            }
            this.arr.push(obj)
            this.restPoint.push(obj)
          }
        }
        this.ctx.clearRect(0, 0, this.canvasWidth, this.canvasHeight)

        for (let i = 0; i < this.arr.length; i++) {
          this.drawCle(this.arr[i].x, this.arr[i].y)
        }
        this.ctx.draw(true)
      },
      // 初始化解锁密码面板
      drawCle (x, y) {
        this.ctx.setStrokeStyle(this.drawColor)
        this.ctx.setLineWidth(2)
        this.ctx.beginPath()
        this.ctx.arc(x, y, this.r, 0, Math.PI * 2, true)
        this.ctx.closePath()
        this.ctx.stroke()
      },
      onTouchstart (e) {
        const po = this.getPosition(e)
        for (let i = 0; i < this.arr.length; i++) {
          if (Math.abs(po.x - this.arr[i].x) < this.r && Math.abs(po.y - this.arr[i].y) < this.r) {
            this.touchFlag = true
            this.drawPoint(this.arr[i].x, this.arr[i].y)
            this.lastPoint.push(this.arr[i])
            this.restPoint.splice(i, 1)
            break
          }
        }
        this.touchFlag && this.ctx.draw(true)
      },
      onTouchmove (e) {
        const now = new Date()
        const duration = now - prev
        // 由于小程序canvas效率低下，帧频率大于60丢弃
        if (duration < Math.floor(1000 / LIMIT)) return
        prev = now

        if (this.touchFlag) {
          this.update(this.getPosition(e))
        }
      },

      onTouchend (e) {
        if (this.touchFlag) {
          this.touchFlag = false
          this.storePass(this.lastPoint)
          // 300ms 重置，重置时间会影响lastPoint取值， 影响drawline报错
          setTimeout(() => {
            this.reset()
          }, 300)
        }
      },
      onTouchcancel (e) {
        this.touchFlag = false
      },
      // 获取touch点相对于canvas的坐标
      getPosition (e) {
        return {
          x: e.touches[0].x,
          y: e.touches[0].y
        }
      },
      // 核心变换方法在touchmove时候调用
      update (po) {
        this.ctx.clearRect(0, 0, this.canvasWidth, this.canvasHeight)
        for (let i = 0; i < this.arr.length; i++) { // 每帧先把面板画出来
          this.drawCle(this.arr[i].x, this.arr[i].y)
        }

        this.drawPoint(this.lastPoint) // 每帧花轨迹
        this.drawLine(po, this.lastPoint) // 每帧画圆心

        for (let i = 0; i < this.restPoint.length; i++) {
          const pt = this.restPoint[i]

          if (Math.abs(po.x - pt.x) < this.r && Math.abs(po.y - pt.y) < this.r) {
            this.drawPoint(pt.x, pt.y)
            this.pickPoints(this.lastPoint[this.lastPoint.length - 1], pt)
            break
          }
        }
        this.ctx.draw(true)
      },
      // 初始化圆心
      drawPoint () {
        for (let i = 0; i < this.lastPoint.length; i++) {
          this.ctx.setFillStyle(this.drawColor) // 注意用set方法
          this.ctx.beginPath()
          this.ctx.arc(this.lastPoint[i].x, this.lastPoint[i].y, this.r / 2, 0, Math.PI * 2, true)
          this.ctx.closePath()
          this.ctx.fill()
        }
      },
      // 解锁轨迹
      drawLine (po, lastPoint) {
        this.ctx.beginPath()
        this.ctx.lineWidth = 3
        this.ctx.moveTo(this.lastPoint[0].x, this.lastPoint[0].y)

        for (let i = 1; i < this.lastPoint.length; i++) {
          this.ctx.lineTo(this.lastPoint[i].x, this.lastPoint[i].y)
        }
        this.ctx.lineTo(po.x, po.y)
        this.ctx.stroke()
        this.ctx.closePath()
      },
      pickPoints (fromPt, toPt) {
        const lineLength = getDis(fromPt, toPt)
        const dir = toPt.index > fromPt.index ? 1 : -1

        const len = this.restPoint.length
        let i = dir === 1 ? 0 : (len - 1)
        let limit = dir === 1 ? len : -1
        while (i !== limit) {
          const pt = this.restPoint[i]
          if (getDis(pt, fromPt) + getDis(pt, toPt) === lineLength) {
            this.drawPoint(pt.x, pt.y)
            this.lastPoint.push(pt)
            this.restPoint.splice(i, 1)
            if (limit > 0) {
              i--
              limit--
            }
          }
          i += dir
        }
      },
      // touchend结束之后对密码和状态的处理
      storePass (psw) {
        let title, color
        if (this.pswObj.step === 1) {
          if (this.checkPass(this.pswObj.fpassword, psw)) {
            this.pswObj.step = 2
            this.pswObj.spassword = psw
            title = '密码保存成功'
            color = this.drawColor
            this.drawStatusPoint(this.drawColor)
          } else {
            title = '两次不一致，重新输入'
            color = 'red'
            this.drawStatusPoint('red')
            delete this.pswObj.step
          }
        } else if (this.pswObj.step === 2) {
          if (this.checkPass(this.pswObj.spassword, psw)) {
            title = '解锁成功'
            this.drawStatusPoint(this.drawColor)
          } else {
            title = '解锁失败'
            this.drawStatusPoint('red')
          }
        } else {
          this.pswObj.step = 1
          this.pswObj.fpassword = psw
          title = '再次输入'
        }

        this.setData(title, color)
      },

      checkPass (psw1, psw2) { // 检测密码
        let p1 = ''
        let p2 = ''
        psw1.forEach(item => {
          p1 += item.index + item.index
        })
        psw2.forEach(item => {
          p2 += item.index + item.index
        })
        return p1 === p2
      },

      drawStatusPoint (color) { // 初始化状态线条
        for (let i = 0; i < this.lastPoint.length; i++) {
          this.ctx.setStrokeStyle(color)
          this.ctx.beginPath()
          this.ctx.arc(this.lastPoint[i].x, this.lastPoint[i].y, this.r, 0, Math.PI * 2, true)
          this.ctx.closePath()
          this.ctx.stroke()
        }

        this.ctx.draw(true)
      },

      updatePassword () {
        this.pswObj = {}
        this.setData()
        this.reset()
      },
      // 设置
      setData (title = '绘制解锁图案', color = '#888') {
        this.title = title
        this.color = color
        if (title === '密码保存成功') {
          setTimeout(() => {
            this.title = '开始解锁'
            this.color = color
          }, 1000)
        }
      },
      // 重置
      reset () {
        this.createCircle()
      }

    }
  }
</script>

<style lang="less" rel="stylesheet/less" scoped>
  .lock_wp {
    margin: ~'30rpx' auto;
  }

  .canvasLock {
    display: block;
    margin: 0 auto;
  }

  .lock_title {
    text-align: center;
    color: #888;
  }

  .lock_reset {
    width: ~'300rpx';
    font-size: 12px;
    color: #888;
  }
</style>
