<template>
  <view class="players-must-see">
    <view class="items" v-for="(item,index) in list" :key="index" @click="jump(item.type)">
      <image :src="item.icon" mode="" />
      <text>{{item.title}}</text>
    </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        list: [{
          title: '锐龙情报站',
          icon: '../../static/image//playersMustSee/borad.png',
          url: '',
          type: 1
        }, {
          title: '锐龙影院',
          icon: '../../static/image//playersMustSee/movie.png',
          url: '',
          type: 2
        }, {
          title: '锐龙画廊',
          icon: '../../static/image//playersMustSee/pic.png',
          url: '',
          type: 3
        }],
      };
    },

    methods: {
      jump(type) {
        uni.navigateTo({
          url: 'lists/lists?type=' + type,
        });
      }
    },

  };
</script>

<style scoped lang='scss'>
  .players-must-see {
    width: 100vw;
    height: 100vh;
    box-sizing: border-box;
    overflow: hidden;
    background: #fff;
    padding: 191rpx 142rpx 0;

    .items {
      width: 473rpx;
      height: 150rpx;
      border: 4rpx solid rgba(243, 101, 35, 1);
      display: flex;
      justify-content: flex-start;
      align-items: center;
      margin-bottom: 50rpx;

      image {
        width: 45rpx;
        height: 45rpx;
        margin-right: 30rpx;
        margin-left: 84rpx;
      }

      text {
        font-size: 46rpx;
        font-weight: 800;
        color: rgba(51, 51, 51, 1);
      }
    }
  }
</style>