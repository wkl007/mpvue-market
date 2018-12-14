<template>
  <div class="index">
    <div class="index-hd">
      <p class="index-desc">以下是小程序手势解锁组件，组件样式仅供参考，开发者可根据自身需求自定义组件样式。</p>
    </div>
    <gesture-lock-component
      ref="gestureLock"
      @change="handleChange"
    />
  </div>
</template>

<script>
  import GestureLockComponent from '../../components/gestureLock'

  export default {
    components: {
      GestureLockComponent
    },
    data () {
      return {
        canvasWidth: 300,
        canvasHeight: 300,
        canvasId: 'canvasLock',
        drawColor: '#3985ff'
      }
    },
    methods: {
      handleChange () {
        console.log(222)
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
