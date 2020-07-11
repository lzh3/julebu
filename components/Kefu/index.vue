<template>
  <view class="kefu" @click="kefu">
    <image src="../../static/image/kefu.png"></image>
    <text>客服</text>
  </view>
</template>

<script>
  export default {
    props: {

    },
    data() {
      return {

      };
    },
    methods: {
      kefu() {

      },
    },
  };
</script>

<style scoped lang='scss'>
  .kefu {
    display: flex;
    flex-direction: column;
    position: fixed;
    right: 30rpx;
    bottom: 160rpx;

    image {
      width: 55rpx;
      height: 56rpx;
    }

    text {
      font-size: 24rpx;
      font-weight: 500;
      color: rgba(243, 101, 35, 1);
      margin-top: 10rpx;
    }
  }
</style>