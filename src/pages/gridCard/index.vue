<template>
  <div class="grid-card">
    <div class="index-hd">
      <p class="index-desc">以下是小程序九宫格翻纸牌组件，组件样式仅供参考，开发者可根据自身需求自定义组件样式，具体属性参数详见开发文档。</p>
    </div>
    <grid-card-component
      ref="gridCard"
      :gridData="gridData"
      @change="handleChange"
    />
    <button @click="onStart">开始抽奖</button>
  </div>
</template>

<script>
  import GridCardComponent from '../../components/gridCard'

  export default {
    components: {
      GridCardComponent
    },
    data () {
      return {
        count: 0,
        gridData: [
          { inlineStyle: '', isBack: false, isMove: false, award: '一等奖' },
          { inlineStyle: '', isBack: false, isMove: false, award: '二等奖' },
          { inlineStyle: '', isBack: false, isMove: false, award: '三等奖' },
          { inlineStyle: '', isBack: false, isMove: false, award: '四等奖' },
          { inlineStyle: '', isBack: false, isMove: false, award: '五等奖' },
          { inlineStyle: '', isBack: false, isMove: false, award: '六等奖' },
          { inlineStyle: '', isBack: false, isMove: false, award: '七等奖' },
          { inlineStyle: '', isBack: false, isMove: false, award: '八等奖' },
          { inlineStyle: '', isBack: false, isMove: false, award: '九等奖' }
        ]
      }
    },
    onUnload () {
      this.count = 0
    },
    methods: {
      // 开始
      onStart () {
        this.count++
        console.log(this.count)
        if (this.count !== 0) {
          this.$refs.gridCard.reset()
        } else {
          this.$refs.gridCard.start()
        }
      },
      // 结果
      handleChange (val) {
        console.log(val)
      }
    }
  }
</script>

<style lang="less" scoped>
  .index-hd {
    padding: ~'80rpx';
    text-align: center;
    padding: ~'20rpx' ~'30rpx' ~'40rpx';
  }

  .index-desc {
    margin-top: ~'20rpx';
    color: #888888;
    font-size: ~'28rpx';
  }

  button {
    margin-top: ~'40rpx';
    width: ~'400rpx';
  }


  button {
    margin-top: ~'40rpx';
    width: ~'400rpx';
  }
</style>
