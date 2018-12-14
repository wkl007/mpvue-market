<template>
  <div class="scratch">
    <div class="index-hd">
      <p class="index-desc">
        以下是小程序大转盘组件，组件样式仅供参考，开发者可根据自身需求自定义组件样式。
      </p>
    </div>
    <scratch-component
      @change="handleChange"
      ref="scratch"/>
    <button @click="onStart">{{text}}</button>
  </div>
</template>

<script>
  import ScratchComponent from '../../components/scratch'

  export default {
    components: {
      ScratchComponent
    },
    data () {
      return {
        isStart: true,
        text: '开始刮奖'
      }
    },
    onUnload () {
      this.isStart = true
      this.text = '开始刮奖'
    },
    methods: {
      onStart () {
        if (this.isStart) {
          this.$refs.scratch.start()
          this.text = '重新开始'
          this.isStart = false
        } else {
          this.$refs.scratch.restart()
        }
      },
      handleChange (val) {
        wx.showModal({
          title: '提示',
          content: `您中了${val}`,
          showCancel: false,
          success: res => {
            if (res.confirm) {
              console.log('用户点击确定')
            } else if (res.cancel) {
              console.log('用户点击取消')
            }
            this.$refs.scratch.restart()
            this.text = '开始'
            this.isStart = true
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
