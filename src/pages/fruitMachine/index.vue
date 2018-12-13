<template>
  <div class="fruit-machine">
    <div class="index-hd">
      <p class="index-desc">以下是小程序水果机组件，组件样式仅供参考，开发者可根据自身需求自定义组件样式。</p>
    </div>
  </div>
</template>

<script>
  export default {
    data () {
      return {}
    },
    components: {},
    methods: {}
  }
</script>

<style lang="less" scoped>
  .index-hd {
    padding: ~'80rpx';
    text-align: center;
    padding: ~'20rpx' ~'26rpx' ~'40rpx';
    .index-desc {
      margin-top: ~'20rpx';
      color: #888;
      font-size: ~'28rpx';
    }
  }

</style>
