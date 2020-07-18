<template>
  <view class="swiper-wrap">
    <swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
      :duration="duration">
      <swiper-item v-for="(item,index) in swiperItems" :key="index" class="item">
        <image mode="aspectFill" :src="item.image" @click="jump(item.url)"></image>
      </swiper-item>
    </swiper>
  </view>
</template>

<script>
  export default {
    props: {
      swiperItems: Array
    },
    data() {
      return {
        indicatorDots: true,
        autoplay: true,
        interval: 2000,
        duration: 500
      }
    },
    methods: {
      jump(url) {
        uni.navigateTo({
          url
        });
      }
    },
    mounted() {},
  }
</script>

<style lang="scss" scoped>
  image {
    width: 100%;
    height: 350upx;
  }

  .swiper-wrap,
  .swiper,
  .item {
    height: 350upx;
  }
</style>