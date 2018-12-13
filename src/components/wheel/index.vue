<template>
  <div id="wheel">
    <div class="wheel-wp">
      <div v-if="mode===1">
        <img src="./images/dial_bg.png" alt="">
        <div class="wheel-pointer"
             :style="[{webkitTransform:'rotate('+deg+'deg) translateZ(0)',transform:'rotate('+deg+'deg) translateZ(0)'}]"
        >
          <img @click="start" src="./images/dial_pointer.png" alt="">
        </div>
      </div>
      <div v-else>
        <img
          src="./images/dial_bg.png" alt=""
          :style="[{webkitTransform:'rotate('+deg+'deg) translateZ(0)',transform:'rotate('+deg+'deg) translateZ(0)'}]"
        >
        <div class="wheel-pointer">
          <img @click="start" src="./images/dial_pointer.png" alt="">
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      // 抽奖模式 1是指针旋转，2为转盘旋转
      mode: {
        type: Number,
        default: 1
      },
      // 奖区数量
      areaNumber: {
        type: Number,
        default: 8
      },
      // 转动速度
      speed: {
        type: Number,
        default: 16
      },
      // 中奖区域从1开始
      awardNumber: {
        type: Number,
        default: 1
      }
    },
    data () {
      return {
        deg: 0, // 开始位置
        singleAngle: '', // 每片扇形的角度
        isStart: false // 开始状态
      }
    },
    mounted () {
      this.init()
    },
    methods: {
      // 转盘初始化
      init () {
        this.singleAngle = 360 / this.areaNumber
      },
      // 开始抽奖
      start () {
        if (this.isStart) return
        this.isStart = true
        let endAddAngle = 0
        if (this.mode === 1) {
          endAddAngle = 360 + this.singleAngle / 2 + (this.awardNumber - 1) * this.singleAngle
        } else {
          endAddAngle = 360 - this.singleAngle / 2 - (this.awardNumber - 1) * this.singleAngle
        }
        const rangeAngle = (Math.floor(Math.random() * 4) + 4) * 360 // 随机旋转几圈再停止
        let cAngle
        this.deg = 0
        this.timer = setInterval(() => {
          if (this.deg < rangeAngle) {
            this.deg += this.speed
          } else {
            cAngle = (endAddAngle + rangeAngle - this.deg) / this.speed
            cAngle = cAngle > this.speed ? this.speed : cAngle < 1 ? 1 : cAngle
            this.deg += cAngle

            if (this.deg >= (endAddAngle + rangeAngle)) {
              this.deg = endAddAngle + rangeAngle
              this.isStart = false
              clearInterval(this.timer)
              this.$emit('change', this.awardNumber)
              // this.endCallBack()
            }
          }
        }, 1000 / 60)
      },
      // 重置
      reset () {
        this.singleAngle = ''
        this.deg = 0
        this.init()
      }
    }
  }
</script>

<style lang="less" rel="stylesheet/less" scoped>

  .wheel-wp {
    position: relative;
    width: ~'500rpx';
    height: ~'500rpx';
    margin: 0 auto;
    img {
      display: block;
      width: ~'500rpx';
      height: ~'500rpx';
    }
    .wheel-pointer {
      position: absolute;
      width: ~'150rpx';
      height: ~'150rpx';
      top: 50%;
      left: 50%;
      margin: ~'-75rpx' 0 0 ~'-75rpx';
      transform-origin: 50% 50%;
      img {
        position: absolute;
        width: ~'150rpx';
        height: ~'238rpx';
        left: 0;
        top: ~'-64rpx';
      }
    }
  }
</style>
