<template>
	<view class="designer-in">
		<view class="items" v-for="(item,index) in list" :key="index">
			<image :src="item.pic" mode="scaleToFill" />
			<view class="items-content">
				<view class="title">{{item.title}}</view>
				<image src="../../static/image/arrow-right.png" mode="" class="operate-btn" @click="detail(item)" />
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [{
					title: 'AMD锐龙-瞬间响应你的火花迸线',
					pic: "../../static/image/temp_designerin_pic.png",
				}, {
					title: 'AMD锐龙-瞬间响应你的火花迸线',
					pic: "../../static/image/temp_designerin_pic.png",
				}, {
					title: 'AMD锐龙-瞬间响应你的火花迸线',
					pic: "../../static/image/temp_designerin_pic.png",
				}]
			};
		},
		methods: {
			detail(item) {
				console.log("TCL: detail -> item", item)
			}
		},
	}
</script>

<style scoped lang="scss">
	.designer-in {
		.items {
			margin-bottom: 30rpx;
			padding: 0 30rpx;
			display: flex;
			justify-content: flex-start;
			height: 170rpx;
			background: rgba(255, 255, 255, 1);

			>image {
				flex: 0 0 170rpx;
				height: 170rpx;
				border-radius: 4rpx;
			}

			.items-content {
				margin-left: 40rpx;
				display: flex;
				justify-content: space-between;
				align-items: center;

				.title {
					font-size: 34rpx;
					font-weight: bold;
					color: rgba(51, 51, 51, 1);
					line-height: 1.5;
				}

				.operate-btn {
					width: 30rpx;
					height: 54rpx;
					background: transparent;
					margin-left: 115rpx;
				}
			}
		}

	}
</style>