<template>
	<view class="kefu" @click="kefu">
		<button open-type="contact" class="btn"></button>
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
		position: relative;
		display: flex;
		flex-direction: column;
		position: fixed;
		right: 30rpx;
		bottom: 160rpx;

		.btn {
			display: block;
			position: absolute;
			top:0;
			left:0;
			width:55rpx;
			height:96rpx;
			z-index: 3;
			border: none;
			opacity: 0;
			outline: none;
		}

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
