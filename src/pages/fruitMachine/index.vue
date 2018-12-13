<template>
  <div class="fruit-machine">
    <div class="index-hd">
      <p class="index-desc">以下是小程序水果机组件，组件样式仅供参考，开发者可根据自身需求自定义组件样式。</p>
    </div>
    <fruit-machine-component
      @change="handleChange"
      ref="fruitMachine"
    />
  </div>
</template>

<script>
  import FruitMachineComponent from '../../components/fruitMachine'

  export default {
    components: {
      FruitMachineComponent
    },
    data () {
      return {}
    },
    methods: {
      // 结果
      handleChange (val) {
        wx.showModal({
          title: '提示',
          content: `您选中了${val}项`,
          showCancel: false,
          success: res => {
            if (res.confirm) {
              console.log('用户点击确定')
            } else if (res.cancel) {
              console.log('用户点击取消')
            }
            this.$refs.fruitMachine.reset()
          }
        })
      }
    }
  }
</script>

<style lang="less" scoped>
  .index-hd {
    padding: ~'80rpx';
    text-align: center;
    padding: ~'20rpx' ~'26rpx' ~'40rpx';
  }

  .index-desc {
    margin-top: ~'20rpx';
    color: #888;
    font-size: ~'28rpx';
  }

  button {
    margin-top: ~'40rpx';
    width: ~'400rpx';
  }

</style>
