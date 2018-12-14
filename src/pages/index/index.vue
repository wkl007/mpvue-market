<template>
  <div class="index">
    <div class="index-hd">
      <img src="../../assets/images/logo.png" alt="" class="index-logo">
      <p class="index-desc">以下将小程序营销组件能力，组件样式仅供参考，开发者可根据自身需求自定义组件样式。</p>
    </div>
    <div class="index-bd">
      <div class="kind-list">
        <div
          v-for="(item,index) in list"
          :key="index"
        >
          <div class="kind-list-item">
            <div :id="item.id" class="kind-list-item-hd">
              <navigator :url="item.url" class="navigator" hover-class="none">
                <div class="kind-list-text">{{item.name}}</div>
                <img class="kind-list-img" :src="item.imgUrl" alt="">
              </navigator>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
  import wheel from '../../assets/images/wheel.png'
  import scratch from '../../assets/images/scratch.png'
  import slotMachine from '../../assets/images/slotMachine.png'
  import fruitMachine from '../../assets/images/fruitMachine.png'
  import shake from '../../assets/images/shake.png'
  import gestureLock from '../../assets/images/gestureLock.png'

  export default {
    data () {
      return {
        list: [
          {
            id: 'wheel',
            sub: 'wheel',
            url: '/pages/wheel/main',
            imgUrl: wheel,
            name: '大转盘'
          },
          {
            id: 'scratch',
            sub: 'scratch',
            url: '/pages/scratch/main',
            imgUrl: scratch,
            name: '刮刮乐'
          },
          {
            id: 'slotMachine',
            sub: 'slotMachine',
            url: '/pages/slotMachine/main',
            imgUrl: slotMachine,
            name: '老虎机'
          },
          {
            id: 'fruitMachine',
            sub: 'fruitMachine',
            url: '/pages/fruitMachine/main',
            imgUrl: fruitMachine,
            name: '水果机'
          },
          {
            id: 'shake',
            sub: 'shake',
            url: '/pages/shake/main',
            imgUrl: shake,
            name: '摇一摇'
          },
          {
            id: 'gestureLock',
            sub: 'gestureLock',
            url: '/pages/gestureLock/main',
            imgUrl: gestureLock,
            name: '手势解锁'
          }
        ]
      }
    },
    components: {},
    methods: {}
  }
</script>

<style lang="less" scoped>
  .index-hd {
    padding: ~'80rpx';
    text-align: center;
  }

  .index-bd {
    padding: 0 ~'30rpx' ~'40rpx';
  }

  .index-ft {
    padding-bottom: ~'20rpx';
    text-align: center;
  }

  .index-logo {
    width: ~'86rpx';
    height: ~'86rpx';
  }

  .index-desc {
    margin-top: ~'20rpx';
    color: #888888;
    font-size:  ~'28rpx';
  }

  .navigator {
    position: relative;
    display: block;
    align-items: center;
    width: 100%;
  }

  .kind-list-item {
    margin: ~'20rpx' 0;
    background-color: #FFFFFF;
    border-radius: ~'4rpx';
    overflow: hidden;
  }

  .kind-list-item:first-child {
    margin-top: 0;
  }

  .kind-list-text {
    flex: 1;
    font-size: ~'30rpx';
  }

  .kind-list-img {
    width: ~'60rpx';
    height: ~'60rpx';
    position: absolute;
    right: 0;
    top: 0;
  }

  .kind-list-item-hd {
    padding: ~'30rpx';
    display: flex;
    align-items: center;

    transition: opacity .3s;
  }

  .kind-list-item-hd-show {
    opacity: .2;
  }

  .kind-list-item-bd {
    height: 0;
    overflow: hidden;
  }

  .kind-list-item-bd-show {
    height: auto;
  }
</style>
