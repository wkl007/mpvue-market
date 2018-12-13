<template>
  <div class="wheel">
    <div class="index-hd">
      <p class="index-desc">
        以下是小程序大转盘组件，组件样式仅供参考，开发者可根据自身需求自定义组件样式。
      </p>
    </div>
    <div class="index-mod">
      <span>选择旋转物体：</span>
      <span
        class="index-mod-btn"
        :class="mode===1?'on':''"
        @click="onSwitchMode(1)"
      >
        指针旋转
      </span>
      <span
        class="index-mod-btn"
        :class="mode===2?'on':''"
        @click="onSwitchMode(2)"
      >
        大转盘旋转
      </span>
    </div>
    <wheel-component
      :mode="mode"
      :awardNumber="awardNumber"
      @change="handleChange"
      ref="wheel"
    />
  </div>
</template>

<script>
  import WheelComponent from '../../components/wheel'
  import { randomNum } from '../../utils'

  export default {
    components: {
      WheelComponent
    },
    data () {
      return {
        mode: 1,
        awardNumber: randomNum(1, 8)
      }
    },
    methods: {
      onSwitchMode (val) {
        this.mode = val
        this.$refs.wheel.reset()
      },
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
            this.awardNumber = randomNum(1, 8)
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

  .index-mod {
    margin: ~'30rpx' ~'80rpx' ~'60rpx' ~'60rpx';
    color: #888;
  }

  .index-mod text {
    display: inline-block;
    line-height: 20px;
  }

  .index-mod-btn {
    display: inline-block;
    flex: 1;
    height: 20px;
    line-height: 20px;
    text-align: center;
    border: 1px solid #666;
    border-radius: ~'6rpx';
    font-size: 12px;
    padding: 0 ~'20rpx';
  }

  .index-mod-btn.on {
    color: #3985ff;
    border: 1px solid #3985ff;
  }

  .index-mod-btn:last-child {
    margin-left: ~'20rpx';
  }
</style>
