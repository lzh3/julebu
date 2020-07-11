<template>
	<view class="internet-bar">
		<view class="items" v-for="(item,index) in list" :key="index">
			<image :src="item.pic" mode="scaleToFill" />
			<view class="items-content">
				<view class="title">{{item.title}}</view>
				<view class="operate-btn" @click="detail(item)">
					<text>查看详情</text>
					<image src="../../static/image/internetBar/double-arrow-right.png" mode="" />
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [{
					title: '网吧干货分享平台 - AMD 3600 + 16G内存 + 1080的游戏测试',
					pic: "../../static/image/internetBar/pic.png",
				}, {
					title: '网吧干货分享平台 - AMD 3600 + 16G内存 + 1080的游戏测试',
					pic: "../../static/image/internetBar/pic.png",
				}, {
					title: '网吧干货分享平台 - AMD 3600 + 16G内存 + 1080的游戏测试',
					pic: "../../static/image/internetBar/pic.png",
				}]
			}
		},
		methods: {
			detail(item) {
				console.log("TCL: detail -> item", item)
			}
		}
	}
</script>

<style scoped lang="scss">
	.internet-bar {
		.items {
			height: 233rpx;
			background: rgba(255, 255, 255, 1);
			margin-bottom: 10rpx;
			padding: 30rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;

			>image {
				// width: 100%;
				width: 130rpx;
				min-width: 130rpx;
				height: 130rpx;
				// background: rgba(243, 101, 35, 1);
			}

			.items-content {
				margin-left: 20rpx;

				.title {
					font-size: 26rpx;
					font-weight: bold;
					color: rgba(51, 51, 51, 1);
					line-height: 1.5;
				}

				.operate-btn {
					display: flex;
					align-items: center;
					justify-content: flex-end;
					margin-top: 22rpx;

					text {
						font-size: 22rpx;
						font-weight: bold;
						color: rgba(243, 101, 35, 1);
					}

					image {
						width: 18rpx;
						height: 22rpx;
						background: transparent;
						margin-left: 10rpx;
					}
				}
			}
		}

	}
</style>