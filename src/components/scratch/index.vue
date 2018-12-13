<template>
  <div id="scratch">
    <div class="scratch_wp"
         style="position:relative;margin: 0 auto;"
         :style="[{
           width:canvasWidth+'px',
           height:canvasHeight+'px'
         }]"
    >
      <canvas
        style="margin: 0 auto; background: transparent;"
        :style="[{
          width:canvasWidth+'px',
          height:canvasHeight+'px'
        }]"
        canvas-id="scratch"
        @click="click"
        @touchstart="touchStart"
        @touchmove="touchMove"
        @touchend="touchEnd"
        :disable-scroll="isScroll"></canvas>
      <div class="scratch_txt"
           style="position:absolute;top:0;left:0;width: 100%;height:100%;text-align: center;pointer-events:none;z-index: -1;"
           :style="[{
              lineHeight:canvasHeight+'px',
              color:awardTxtColor,
              fontSize:awardTxtFontSize+'px'
            }]"
           v-show="isLoaded"
      >
        {{awardTxt}}
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  const ctx = wx.createCanvasContext('scratch')

  export default {
    props: {
      // 画布宽度
      canvasWidth: {
        type: Number,
        default: 197
      },
      // 画布高度
      canvasHeight: {
        type: Number,
        default: 72
      },
      // 遮罩层图片
      imageResource: {
        type: String,
        default: ''
      },
      // 笔触半径
      radius: {
        type: Number,
        default: 10
      },
      // 底部抽奖文字奖项
      awardTxt: {
        type: String,
        default: 'iphoneXX'
      },
      // 底部抽奖文字颜色
      awardTxtColor: {
        type: String,
        default: '#1AAD16'
      },
      // 底部抽奖文字大小
      awardTxtFontSize: {
        type: Number,
        default: 24
      },
      // 没有图片遮罩层颜色
      maskColor: {
        type: String,
        default: '#ccc'
      }
    },
    canvasOptions: {
      canvasId: 'scratch'
    },
    data () {
      return {
        lastX: 0,
        lastY: 0,
        minX: 0,
        minY: 0,
        maxX: 0,
        maxY: 0,
        isStart: false,
        isScroll: false,
        isLoaded: false
      }
    },
    mounted () {
      console.log(ctx)
      this.init()
      this.isScroll = true
    },
    onUnload () {
      this.lastX = 0
      this.lastY = 0
      this.minX = ''
      this.minY = ''
      this.maxX = ''
      this.maxY = ''
      this.isStart = true
      this.isScroll = false
      this.isLoaded = false
    },
    methods: {
      click () {
        console.log('click')
      },
      // 初始化
      init () {
        const { canvasWidth, canvasHeight, imageResource, maskColor } = this
        ctx.clearRect(0, 0, canvasWidth, canvasHeight)
        if (imageResource) {
          wx.downloadFile({
            url: imageResource,
            success: res => {
              ctx.drawImage(res.tempFilePath, 0, 0, canvasWidth, canvasHeight)
              ctx.draw()
            }
          })
        } else {
          ctx.setFillStyle(maskColor)
          ctx.fillRect(0, 0, canvasWidth, canvasHeight)
          ctx.draw()
        }
      },
      // 开始
      start () {
        this.isLoaded = true
        this.isStart = true
        this.isScroll = false
      },
      // 重置
      restart () {
        this.init()
        this.lastX = 0
        this.lastY = 0
        this.minX = ''
        this.minY = ''
        this.maxX = ''
        this.maxY = ''
        this.isStart = true
        this.isScroll = false
      },
      reset () {
        this.init()
      },
      // 绘图
      drawRect (x, y) {
        const { radius, minX, minY, maxX, maxY } = this
        const x1 = x - radius > 0 ? x - radius : 0
        const y1 = y - radius > 0 ? y - radius : 0
        if (minX !== '') {
          this.minX = minX > x1 ? x1 : minX
          this.minY = minY > y1 ? y1 : minY
          this.maxX = maxX > x1 ? maxX : x1
          this.maxY = maxY > y1 ? maxY : y1
        } else {
          this.minX = x1
          this.minY = y1
          this.maxX = x1
          this.maxY = y1
        }
        this.lastX = x1
        this.lastY = y1

        return [x1, y1, 2 * radius]
      },
      touchStart (e) {
        console.log(111)
        if (!this.isStart) return
        const pos = this.drawRect(e.touches[0].x, e.touches[0].y)
        ctx.clearRect(pos[0], pos[1], pos[2], pos[2])
        ctx.draw(true)
      },

      touchMove (e) {
        if (!this.isStart) return
        const pos = this.drawRect(e.touches[0].x, e.touches[0].y)
        ctx.clearRect(pos[0], pos[1], pos[2], pos[2])
        ctx.draw(true)
      },

      touchEnd (e) {
        if (!this.isStart) return
        // 自动清楚采用点范围值方式判断
        const { canvasWidth, canvasHeight, minX, minY, maxX, maxY } = this
        if (maxX - minX > 0.7 * canvasWidth && maxY - minY > 0.7 * canvasHeight) {
          ctx.draw()
          console.log('完成')
          this.$emit('change', this.awardTxt)
          // this.endCallBack && this.endCallBack()
          this.isStart = false
          this.isScroll = true
        }
      }
    }
  }
</script>

<style lang="less" rel="stylesheet/less" scoped>


</style>
