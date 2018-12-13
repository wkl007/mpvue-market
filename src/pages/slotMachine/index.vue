<template>
  <div class="slot-machine">
    <div class="index-hd">
      <p class="index-desc">以下是小程序老虎机组件，组件样式仅供参考，开发者可根据自身需求自定义组件样式。</p>
    </div>
    <slot-machine-component
      @change="handleChange"
      ref="slotMachine"/>
    <button @click="onStart">开始抽奖</button>
  </div>
</template>

<script>
  import SlotMachineComponent from '../../components/slotMachine'

  export default {
    components: {
      SlotMachineComponent
    },
    data () {
      return {}
    },
    methods: {
      // 开始
      onStart () {
        this.$refs.slotMachine.start()
      },
      // 结果
      handleChange (val) {
        const { num1, num2, num3, num4 } = val
        wx.showModal({
          title: '提示',
          content: `您中了${num1}${num2}${num3}${num4}`,
          showCancel: false,
          success: res => {
            if (res.confirm) {
              console.log('用户点击确定')
            } else if (res.cancel) {
              console.log('用户点击取消')
            }
            this.$refs.slotMachine.reset()
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

  .index-tip {
    color: #888;
    font-size: ~'28rpx';
    text-align: center;
    margin: ~'20rpx' 0;
  }

  button {
    margin-top: ~'40rpx';
    width: ~'400rpx';
  }
</style>
