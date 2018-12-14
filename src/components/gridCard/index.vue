<template>
  <div id="grid-card">
    <div class="gridcard">
      <div v-for="(item,index) in card"
           :key="index"
           class="gridcard_item"
           :class="{back:item.isBack,move:item.isMove}"
           @click="onClick(item,index)"
      >
        <div class="gridcard_front">
          <img src="../../assets/images/card_front.png" alt="">
        </div>
        <div class="gridcard_back">
          <img src="../../assets/images/marquee_lottery2.png" alt="">
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  // const Promise = require('./promise.min')
  // console.log(Promise)

  /**
   * runAsync 延迟返回 promise 方法
   * @param  {Number} time 延迟时间
   * @return {type}   返回Promise对象
   */
  function runAsync (time) {
    return new Promise((resolve, reject) => {
      const timer = setTimeout(() => {
        resolve()
        clearTimeout(timer)
      }, time)
    })
  }

  export default {
    props: {
      // 九宫格数据 inlineStyle 组件所需参数 isBack 是否是反面 isMove 是否运动 award 对应奖项
      gridData: {
        type: Array,
        default: []
      }
    },
    data () {
      return {
        isFlip: false,
        card: []
      }
    },
    mounted () {
      this.card = this.gridData
      this.init()
    },
    onUnlaod () {
      this.isFlip = false
    },
    methods: {
      // 初始化
      init () {
        let data = this.card
        for (let i = 0; i < 9; i++) {
          data[i] = { isBack: false, isMove: false, award: data[i].award }
        }
        this.card = data
      },
      onClick (item, index) {
        const { isFlip } = this
        let data = this.card
        if (!isFlip) return
        data[index].isBack = !data[index].isBack
        this.card = data
        console.log(index, item.award)
      },
      // 开始
      start () {
        let data = this.card
        runAsync(100).then(() => {
          for (let i = 0; i < 3; i++) {
            data[i].isBack = true
          }
          this.card = data
          return runAsync(200)
        }).then(() => {
          for (let i = 3; i < 6; i++) {
            data[i].isBack = true
          }
          console.log(200)
          this.card = data
          return runAsync(200)
        }).then(() => {
          for (let i = 6; i <= 8; i++) {
            data[i].isBack = true
          }
          this.card = data
          return runAsync(800)
        }).then(() => {
          for (let i = 0; i < 9; i++) {
            data[i].isBack = false
          }
          this.card = data
          return runAsync(400)
        }).then(() => {
          for (let i = 0; i < 9; i++) {
            data[i].isMove = true
          }
          this.card = data
          return runAsync(500)
        }).then(() => {
          for (let i = 0; i < 9; i++) {
            data[i].isMove = false
          }
          this.isFlip = true
          this.card = data
        })
      },
      // 重置
      reset () {
        let data = this.card
        this.isFlip = false
        for (let i = 0; i < 9; i++) {
          data[i] = { isBack: false, isMove: false, award: data[i].award }
        }
        this.card = data
        runAsync(800).then(() => {
          this.start()
        })
      }
    }
  }
</script>

<style lang="less" rel="stylesheet/less">
  /**
   * perspective： 400px属性，焦距影响其子元素，会影响整个子元素大空间，perspective(400px) 影响元素本身
   */
  .gridcard {
    margin: 0 ~'20rpx';
    position: relative;
    height: ~'710rpx';
  }

  /*perspective: 400px;*/
  .gridcard .gridcard_item {
    position: absolute;
    width: ~'230rpx';
    height: ~'230rpx';
    -webkit-transform-style: preserve-3d;
    transform-style: preserve-3d;
    -webkit-transition: all 0.8s ease-in-out;
    transition: all 0.8s ease-in-out;
  }

  /*    -webkit-transform: translate3d(240rpx, 102rpx, 0);
      transform: translate3d(240rpx, 102rpx, 0);*/
  .gridcard .gridcard_item image {
    display: block;
    width: 100%;
    height: 100%;
  }

  .gridcard .gridcard_item .gridcard_front, .gridcard .gridcard_item .gridcard_back {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    font-size: 20px;
    color: #fff;
    text-align: center;
    line-height: ~'230rpx';
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
    z-index: 1;
  }

  .gridcard .gridcard_item .gridcard_back {
    -webkit-transform: rotateY(180deg) translateZ(0);
    transform: rotateY(180deg) translateZ(0);
    background: #1E9FD9;
  }

  .gridcard .gridcard_item.back {
    -webkit-transform: perspective(1000px) rotateY(-180deg) translateZ(0);
    transform: perspective(1000px) rotateY(-180deg) translateZ(0);
  }

  .gridcard .gridcard_item.move {
    z-index: 3;
    -webkit-transition: all 0.4s linear;
    transition: all 0.4s linear;
  }

  .gridcard .gridcard_item:nth-child(1) {
    top: 0;
    left: 0;
  }

  .gridcard .gridcard_item:nth-child(1).move {
    -webkit-transform: translate3d(~'240rpx', ~'240rpx', 0);
    transform: translate3d(~'240rpx', ~'240rpx', 0);
  }

  .gridcard .gridcard_item:nth-child(2) {
    top: 0;
    left: ~'240rpx';
  }

  .gridcard .gridcard_item:nth-child(2).move {
    -webkit-transform: translate3d(0, ~'240rpx', 0);
    transform: translate3d(0, ~'240rpx', 0);
  }

  .gridcard .gridcard_item:nth-child(3) {
    top: 0;
    left: ~'480rpx';
  }

  .gridcard .gridcard_item:nth-child(3).move {
    -webkit-transform: translate3d(-240 rpx, ~'240rpx', 0);
    transform: translate3d(-240 rpx, ~'240rpx', 0);
  }

  .gridcard .gridcard_item:nth-child(4) {
    top: ~'240rpx';
    left: 0;
  }

  .gridcard .gridcard_item:nth-child(4).move {
    -webkit-transform: translate3d(~'240rpx', 0, 0);
    transform: translate3d(~'240rpx', 0, 0);
  }

  .gridcard .gridcard_item:nth-child(5) {
    top: ~'240rpx';
    left: ~'240rpx';
  }

  .gridcard .gridcard_item:nth-child(6) {
    top: ~'240rpx';
    left: ~'480rpx';
  }

  .gridcard .gridcard_item:nth-child(6).move {
    -webkit-transform: translate3d(~'-240rpx', 0 rpx, 0);
    transform: translate3d(~'-240rpx', 0 rpx, 0);
  }

  .gridcard .gridcard_item:nth-child(7) {
    top: ~'480rpx';
    left: 0;
  }

  .gridcard .gridcard_item:nth-child(7).move {
    -webkit-transform: translate3d(~'240rpx', ~'-240rpx', 0);
    transform: translate3d(~'240rpx', ~'-240rpx', 0);
  }

  .gridcard .gridcard_item:nth-child(8) {
    top: ~'480rpx';
    left: ~'240rpx';
  }

  .gridcard .gridcard_item:nth-child(8).move {
    -webkit-transform: translate3d(0, ~'-240rpx', 0);
    transform: translate3d(0, ~'-240rpx', 0);
  }

  .gridcard .gridcard_item:nth-child(9) {
    top: ~'480rpx';
    left: ~'480rpx';
  }

  .gridcard .gridcard_item:nth-child(9).move {
    -webkit-transform: translate3d(~'-240rpx', ~'-240rpx', 0);
    transform: translate3d(~'-240rpx', ~'-240rpx', 0);
  }


</style>
