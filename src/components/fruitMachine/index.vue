<template>
  <div id="fruit-machine">
    <div class="marquee_wp">
      <div class="marquee_item"
           :class="idx===1?'on':''"
      >
        <img src="../../assets/images/marquee_lottery1.png"/>
      </div>

      <div class="marquee_item"
           :class="idx===2?'on':''"
      >
        <img src="../../assets/images/marquee_lottery2.png"/>
      </div>

      <div class="marquee_item"
           :class="idx===3?'on':''"
      >
        <img src="../../assets/images/marquee_lottery3.png"/>
      </div>

      <div class="marquee_item"
           :class="idx===8?'on':''"
      >
        <img src="../../assets/images/marquee_lottery2.png"/>
      </div>

      <div class="marquee_item marquee_item_btn on"
           @click="start"
      >
        <img src="../../assets/images/marquee_btn.png"/>
      </div>

      <div class="marquee_item"
           :class="idx===4?'on':''"
      >
        <img src="../../assets/images/marquee_lottery3.png"/>
      </div>

      <div class="marquee_item"
           :class="idx===7?'on':''"
      >
        <img src="../../assets/images/marquee_lottery1.png"/>
      </div>

      <div class="marquee_item"
           :class="idx===6?'on':''"
      >
        <img src="../../assets/images/marquee_lottery2.png"/>
      </div>

      <div class="marquee_item"
           :class="idx===5?'on':''"
      >
        <img src="../../assets/images/marquee_lottery1.png"/>
      </div>

    </div>
  </div>
</template>

<script>
  export default {
    props: {
      // 宫格个数
      len: {
        type: Number,
        default: 8
      },
      // 抽奖结果对应值1～9
      ret: {
        type: Number,
        default: 6
      },
      // 速度值
      speed: {
        type: Number,
        default: 100
      }
    },
    data () {
      return {
        isStart: false,
        idx: ''
      }
    },
    onUnload () {
      this.reset()
    },
    methods: {
      start () {
        let { ret, len, speed, isStart } = this
        if (isStart) return
        this.isStart = true
        let range = Math.floor(Math.random() * 2 + 2)
        let count = 0
        let spd2 = speed * 2
        !(function interval (self) {
          setTimeout(() => {
            count++
            if (count > range * len) {
              speed = spd2
            }
            if (count !== (range + 1) * len + ret) {
              interval(self)
            } else {
              self.isStart = false
              self.$emit('change', self.ret)
            }
            self.idx = count % 8 === 0 ? 8 : count % 8
          }, speed)
        })(this)
      },
      // 重置
      reset () {
        this.idx = ''
      }
    }
  }
</script>

<style lang="less" scoped>
  .marquee_wp {
    margin: 0 ~'20rpx';
    display: flex;
    flex-wrap: wrap;
  }

  .marquee_wp .marquee_item {
    width: ~'230rpx';
    height: ~'230rpx';
    margin: 0 ~'10rpx' ~'10rpx' 0;
    opacity: .6;
    background: #1E9FD9;
    box-sizing: border-box;
  }

  .marquee_wp .marquee_item.on {
    opacity: 1;
  }

  .marquee_wp .marquee_item img {
    display: block;
    width: 100%;
    height: 100%;
  }

  .marquee_wp .marquee_item:nth-child(3n) {
    margin-right: 0;
  }

  .marquee_wp .marquee_item_btn {
    background: transparent;
  }

  .marquee_wp .marquee_item_btn text {
    display: block;
    text-align: center;
    color: #fff;
  }

</style>
