<template>
  <view class="swiper-wrap">
    <swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
      :duration="duration">
      <swiper-item v-for="(item,index) in swiperItems" :key="index" class="item">
        <image mode="aspectFill" :src="item.url"></image>
      </swiper-item>
    </swiper>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        url: '',
        swiperItems: [{
          url: '../../static/image/home/banner1.jpg'
        }, {
          url: '../../static/image/home/banner2.png'
        }, {
          url: '../../static/image/home/banner2.png'
        }],
        indicatorDots: true,
        autoplay: true,
        interval: 2000,
        duration: 500
      }
    },
    methods: {
      init() {
        this.$http.get(this.url)
      }
    },
    mounted() {
      // this.init()
    },
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