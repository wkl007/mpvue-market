<template>
  <div class="shake">
    <div class="index-hd">
      <p class="index-desc">以下是小程序摇一摇组件，组件样式仅供参考，开发者可根据自身需求自定义组件样式。</p>
    </div>
    <shake-component
      :shakeThreshold="shakeThreshold"
      :result="result"
      ref="shake"
      @change="handleChange"
    />
  </div>
</template>

<script>
  import ShakeComponent from '../../components/shake'

  export default {
    components: {
      ShakeComponent
    },
    data () {
      return {
        shakeThreshold: 100,
        result: true
      }
    },
    methods: {
      // 结果
      handleChange (val) {
        if (val) {
          wx.showModal({
            title: '提示',
            content: `您中奖啦！！！`,
            showCancel: false,
            success: res => {
              if (res.confirm) {
                console.log('用户点击确定')
              } else if (res.cancel) {
                console.log('用户点击取消')
              }
              this.$refs.shake.isStart = true
              this.result = false
            }
          })
        } else {
          wx.showModal({
            title: '提示',
            content: `您没有中奖`,
            showCancel: false,
            success: res => {
              if (res.confirm) {
                console.log('用户点击确定')
              } else if (res.cancel) {
                console.log('用户点击取消')
              }
              this.$refs.shake.isStart = true
              this.result = true
            }
          })
        }
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
