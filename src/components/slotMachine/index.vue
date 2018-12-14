<template>
  <div id="slot-machine">
    <div class="machine_wp">
      <div class="machine_item_wp">
        <div class="machine_item_slide"
             :style="[{
             webkitTransform: 'translateY('+transY1+'rpx)',
             transform: 'translateY('+transY1+'rpx)',
             }]"
        >
          <div class="machine_item_ele">0</div>
          <div class="machine_item_ele">1</div>
          <div class="machine_item_ele">2</div>
          <div class="machine_item_ele">3</div>
          <div class="machine_item_ele">4</div>
          <div class="machine_item_ele">5</div>
          <div class="machine_item_ele">6</div>
          <div class="machine_item_ele">7</div>
          <div class="machine_item_ele">8</div>
          <div class="machine_item_ele">9</div>
          <div class="machine_item_ele">0</div>
        </div>
      </div>

      <div class="machine_item_wp">
        <div class="machine_item_slide"
             :style="[{
             webkitTransform: 'translateY('+transY2+'rpx)',
             transform: 'translateY('+transY2+'rpx)',
             }]"
        >
          <div class="machine_item_ele">0</div>
          <div class="machine_item_ele">1</div>
          <div class="machine_item_ele">2</div>
          <div class="machine_item_ele">3</div>
          <div class="machine_item_ele">4</div>
          <div class="machine_item_ele">5</div>
          <div class="machine_item_ele">6</div>
          <div class="machine_item_ele">7</div>
          <div class="machine_item_ele">8</div>
          <div class="machine_item_ele">9</div>
          <div class="machine_item_ele">0</div>
        </div>
      </div>

      <div class="machine_item_wp">
        <div class="machine_item_slide"
             :style="[{
             webkitTransform: 'translateY('+transY3+'rpx)',
             transform: 'translateY('+transY3+'rpx)',
             }]"
        >
          <div class="machine_item_ele">0</div>
          <div class="machine_item_ele">1</div>
          <div class="machine_item_ele">2</div>
          <div class="machine_item_ele">3</div>
          <div class="machine_item_ele">4</div>
          <div class="machine_item_ele">5</div>
          <div class="machine_item_ele">6</div>
          <div class="machine_item_ele">7</div>
          <div class="machine_item_ele">8</div>
          <div class="machine_item_ele">9</div>
          <div class="machine_item_ele">0</div>
        </div>
      </div>

      <div class="machine_item_wp">
        <div class="machine_item_slide"
             :style="[{
             webkitTransform: 'translateY('+transY4+'rpx)',
             transform: 'translateY('+transY4+'rpx)',
             }]"
        >
          <div class="machine_item_ele">0</div>
          <div class="machine_item_ele">1</div>
          <div class="machine_item_ele">2</div>
          <div class="machine_item_ele">3</div>
          <div class="machine_item_ele">4</div>
          <div class="machine_item_ele">5</div>
          <div class="machine_item_ele">6</div>
          <div class="machine_item_ele">7</div>
          <div class="machine_item_ele">8</div>
          <div class="machine_item_ele">9</div>
          <div class="machine_item_ele">0</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import { debounce } from '../../utils'

  export default {
    props: {
      // 单个数字高度
      height: {
        type: Number,
        default: 80
      },
      // 单个项目数字个数
      len: {
        type: Number,
        default: 10
      },
      // 第一列初始位置
      transY1: {
        type: Number,
        default: 0
      },
      // 第一列结束数字
      num1: {
        type: Number,
        default: 5
      },
      // 第二列初始位置
      transY2: {
        type: Number,
        default: 0
      },
      // 第二列结束数字
      num2: {
        type: Number,
        default: 2
      },
      // 第三列初始位置
      transY3: {
        type: Number,
        default: 0
      },
      // 第三列结束数字
      num3: {
        type: Number,
        default: 1
      },
      // 第四列结束数字
      transY4: {
        type: Number,
        default: 0
      },
      // 第四列结束数字
      num4: {
        type: Number,
        default: 1
      },
      // 速度
      speed: {
        type: Number,
        default: 24
      }
    },
    data () {
      return {
        result: {}
      }
    },
    mounted () {
      this.$watch('result', debounce(newVal => {
        this.$emit('change', newVal)
      }, 200))
    },
    onUnload () {
      this.reset()
    },
    methods: {
      // 开始
      start () {
        let { isStart, len, height, transY1, transY2, transY3, transY4, speed, num1, num2, num3, num4 } = this
        if (isStart) return
        this.isStart = true
        const totalHeight = height * len
        const sRange = Math.floor(Math.random() * 2 + 2)
        const halfSpeed = speed / 2
        const endDis1 = num1 === 0 ? 10 * height : num1 * height
        const endDis2 = num2 === 0 ? 10 * height : num2 * height
        const endDis3 = num3 === 0 ? 10 * height : num3 * height
        const endDis4 = num4 === 0 ? 10 * height : num4 * height
        let i1 = 1
        let i2 = 1
        let i3 = 1
        let i4 = 1
        this.timer = setInterval(() => {
          if (i1 <= sRange) {
            transY1 -= speed
            if (Math.abs(transY1) > totalHeight) {
              transY1 += totalHeight
              i1++
            }
          } else if (i1 > sRange && i1 < sRange + 2) {
            transY1 -= halfSpeed
            if (Math.abs(transY1) > totalHeight) {
              transY1 += totalHeight
              i1++
            }
          } else {
            if (transY1 === endDis1) return
            let dropSpeed = (endDis1 + transY1) / halfSpeed
            dropSpeed = dropSpeed > halfSpeed ? halfSpeed : dropSpeed < 1 ? 1 : dropSpeed
            transY1 -= dropSpeed
            transY1 = Math.abs(transY1) > endDis1 ? transY1 = -endDis1 : transY1
          }

          this.timer1 = setTimeout(() => {
            if (i2 <= sRange) {
              transY2 -= speed
              if (Math.abs(transY2) > totalHeight) {
                transY2 += totalHeight
                i2++
              }
            } else if (i2 > sRange && i2 < sRange + 2) {
              transY2 -= halfSpeed
              if (Math.abs(transY2) > totalHeight) {
                transY2 += totalHeight
                i2++
              }
            } else {
              if (transY2 === endDis2) return
              let dropSpeed = (endDis2 + transY2) / halfSpeed
              dropSpeed = dropSpeed > halfSpeed ? halfSpeed : dropSpeed < 1 ? 1 : dropSpeed
              transY2 -= dropSpeed
              transY2 = Math.abs(transY2) > endDis2 ? transY2 = -endDis2 : transY2
            }
          }, 200)

          this.timer2 = setTimeout(() => {
            if (i3 <= sRange) {
              transY3 -= speed
              if (Math.abs(transY3) > totalHeight) {
                transY3 += totalHeight
                i3++
              }
            } else if (i3 > sRange && i3 < sRange + 2) {
              transY3 -= halfSpeed
              if (Math.abs(transY3) > totalHeight) {
                transY3 += totalHeight
                i3++
              }
            } else {
              if (transY3 === endDis3) return
              let dropSpeed = (endDis3 + transY3) / halfSpeed
              dropSpeed = dropSpeed > halfSpeed ? halfSpeed : dropSpeed < 1 ? 1 : dropSpeed
              transY3 -= dropSpeed
              transY3 = Math.abs(transY3) > endDis3 ? transY3 = -endDis3 : transY3
            }
          }, 400)

          this.timer3 = setTimeout(() => {
            if (i4 <= sRange) {
              transY4 -= speed
              if (Math.abs(transY4) > totalHeight) {
                transY4 += totalHeight
                i4++
              }
            } else if (i4 > sRange && i4 < sRange + 2) {
              transY4 -= halfSpeed
              if (Math.abs(transY4) > totalHeight) {
                transY4 += totalHeight
                i4++
              }
            } else {
              let dropSpeed = (endDis4 + transY4) / halfSpeed
              if (num4 < 3) {
                dropSpeed = dropSpeed > halfSpeed ? halfSpeed : dropSpeed < 0.1 ? 0.1 : dropSpeed
              } else if (num4 < 5 && num4 >= 3) {
                dropSpeed = dropSpeed > halfSpeed ? halfSpeed : dropSpeed < 0.3 ? 0.3 : dropSpeed
              } else {
                dropSpeed = dropSpeed > halfSpeed ? halfSpeed : dropSpeed < 0.3 ? 0.3 : dropSpeed
              }

              transY4 -= dropSpeed
              transY4 = Math.abs(transY4) > endDis4 ? transY4 = -endDis4 : transY4
              if (Math.abs(transY4) >= endDis4) {
                clearInterval(this.timer)
                clearTimeout(this.timer1)
                clearTimeout(this.timer2)
                clearTimeout(this.timer3)
                this.isStart = false
                this.result = {
                  num1,
                  num2,
                  num3,
                  num4
                }
              }
            }
          }, 600)
          this.transY1 = transY1
          this.transY2 = transY2
          this.transY3 = transY3
          this.transY4 = transY4
        }, 1000 / 60)
      },
      // 重置
      reset () {
        this.transY1 = 0
        this.transY2 = 0
        this.transY3 = 0
        this.transY4 = 0
      }
    }
  }
</script>

<style lang="less" scoped>
  .machine_wp {
    display: flex;
    width: ~'400rpx';
    margin: ~'60rpx' auto;

    .machine_item_wp {
      width: ~'80rpx';
      height: ~'80rpx';
      position: relative;
      border: ~'1rpx' solid #6190e8;
      border-radius: ~'2rpx';
      overflow: hidden;
      margin-right: ~'10rpx';

      &:last-child {
        margin-right: 0;
      }

      .machine_item_slide {
        position: absolute;
        width: ~'80rpx';
        height: ~'80rpx';
        top: 0;
        left: 0;
      }

      .machine_item_ele {
        width: ~'80rpx';
        height: ~'80rpx';
        line-height: ~'80rpx';
        font-size: ~'24px';
        color: #6190e8;
        text-align: center;
      }
    }
  }

</style>
