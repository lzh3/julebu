<template>
  <!-- 弹窗 -->
  <uni-popup ref="popup" type="center">
    <view class="modal-wrap error" v-show="status === 'error'">
      <image src="../../static/image/error.png" class="error" />
      <text class="title">{{title}}</text>
      <text class="desc">{{desc}}</text>
      <button type="primary" @click="fn" class="btn">{{btnText}}</button>
      <image src="../../static/image/close.png" class="close" @click="close" />
    </view>
    <view class="modal-wrap success" v-show="status === 'success'">
      <image src="../../static/image/success-line.png" class="success" />
      <text class="title">{{title}}</text>
      <text class="desc">{{desc}}</text>
      <image src="../../static/image/close.png" class="close" @click="close" />
    </view>
  </uni-popup>
  </view>
</template>

<script>
  export default {
    props: {
      status: {
        default: () => 'error'
      },
      btnText: {
        default: () => '立即登录'
      },
      desc: {
        default: () => '你还未登录,请登录进行报名'
      },
      title: {
        default: () => '参与失败'
      },
      cb: {
        default: () => () => {}
      },
    },
    data() {
      return {

      };
    },
    methods: {
      close() {
        this.$refs.popup.close()
      },
      open() {
        this.$refs.popup.open()
      },
      fn() {
        this.cb()
      }
    },

  };
</script>

<style scoped lang='scss'>
  //弹窗
  .modal-wrap {
    width: 438rpx;
    // height: 300rpx;
    min-height: 200rpx;
    background: rgba(255, 255, 255, 1);
    border-radius: 30rpx;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-bottom: 30rpx;
    position: relative;

    .error {
      width: 80rpx;
      min-width: 80rpx;
      height: 80rpx;
      min-height: 80rpx;
      margin: 20rpx 0;
    }

    .success {
      width: 70rpx;
      min-width: 70rpx;
      height: 70rpx;
      min-height: 70rpx;
      margin: 20rpx 0;
    }

    .close {
      width: 40rpx;
      height: 40rpx;
      position: absolute;
      z-index: 99999;
      bottom: -50rpx;
    }

    .title {
      font-size: 30rpx;
      font-weight: bold;
      color: rgba(51, 51, 51, 1);
    }

    .desc {
      line-height: 1;
      font-size: 24rpx;
      font-weight: 500;
      margin: 20rpx 30rpx;
      color: rgba(153, 153, 153, 1);
    }

    .btn {
      box-sizing: content-box;
      width: 145rpx;
      min-height: 48rpx;
      line-height: 48rpx;
      background: rgba(243, 101, 35, 1);
      border-radius: 10rpx;
      font-size: 28rpx;
      text-align: center;
    }
  }
</style>