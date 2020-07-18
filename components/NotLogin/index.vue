<template>
  <view class="not-login-wrap">
    <view class="content">
      <image src="../../static/image/headimg.png" />
      <text class="txt">你还未登录,不能参加{{pageName}}.</text>
      <button @click="jump">立即登录</button>
    </view>
  </view>
</template>

<script>
  export default {
    props: {
      pageName: {
        default: () => '培训'
      }
    },
    data() {
      return {

      };
    },

    methods: {
      jump() {
        uni.navigateTo({
          url: '/pages/login/login',
        });
      }
    },

  };
</script>

<style scoped lang='scss'>
  .not-login-wrap {
    background: #fff;
    width: 100vw;
    height: calc(100vh - 88rpx - 100rpx);
    display: flex;
    justify-content: center;
    align-items: center;

    image {
      width: 200rpx;
      height: 200rpx;
    }

    .content {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;

      .txt {
        font-size: 26rpx;
        font-weight: 500;
        color: rgba(102, 102, 102, 1);
        margin: 32rpx 0 49rpx 0;
      }

      button {
        width: 185rpx;
        box-sizing: content-box;
        line-height: 60rpx;
        background: rgba(243, 101, 35, 1);
        border-radius: 10rpx;
		font-size: 16px;
        color: #fff;
      }
    }
  }
</style>