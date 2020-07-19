<template>
  <view class="designer-in-detail">
    <view class="image" v-if="type == 1">
      <image :src="url" mode="widthFix"></image>
    </view>
    <video class='video' :src="url" controls v-else-if='type == 2'></video>
    <web-view class='web' :src="url" v-else-if='type == 3'></web-view>
  </view>
</template>

<script>
  export default {
  	data() {
  		return {
  			type: 0,
        url: ''
  		};
  	},
  	methods: {
      
    },
    onLoad(options) {
      this.type = options.type;
      this.url = options.url;
    }
  }
</script>

<style scoped>
  .iamge,.video,.web{
    width: 100%;
    height: 100%;
  }
  .video{
    position: fixed;
  }
  .image{
    overflow-y: auto;
  }
  .image image{
    width: 100%;
  }
</style>
