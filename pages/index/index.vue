<template>
	<view class="index-container">
		<Swiper />
		<!-- 通知公告 -->
		<view class="index-notice" @click="indexNotice">
			<view class="header-title">
				<image src="../../static/image/home/notice.png" class="icon" />
				<text>通知公告</text>
			</view>
			<view class="header-desc">
				<text class="info">AMD最新培训活动上线啦! 赶快来报名哟~</text>
				<text class="date">06-04</text>
			</view>
		</view>
		<!-- 最新资讯 -->
		<view class="index-news">
			<view class="common-header">
				<image src="../../static/image/home/activity.png" class="icon" />
				<text>最新资讯</text>
			</view>
			<view class="scroll-news">
				<view class="news-items" v-for="(item,index) in newsItems" :key="index">
					<image :src="item.url" mode="" />
					<view class="items-desc">
						<view class="title">
							<text>{{item.mainTitle}}</text>{{item.subTitle}}
						</view>
						<text class="date">{{item.date}}</text>
					</view>
				</view>
			</view>
		</view>
		<!-- banner  -->
		<view class="index-banner">
			<view class="item" v-for="(item,index) in bannerItems" :key="index">
				<image :src="item.url" mode="aspectFit" @click="bannerJump(item.jumpUrl)" />
				<!-- <text>{{item.title}}</text> -->
			</view>
		</view>
		<!-- 最新培训 -->
		<view class="index-train">
			<view class="common-header">
				<image src="../../static/image/home/activity.png" class="icon" />
				<text>最新培训</text>
			</view>
			<view class="train-content">
				<image :src="trainItem.topPic" mode="aspectFit" />
				<view class="main">
					<view class="header">
						<text>{{trainItem.title}}</text>
						<view>
							<image src="../../static/image/home/time.png" mode="" />
							<text class="time-status">{{trainItem.status}}</text>
						</view>
					</view>
					<text class="date">培训时间：<text>{{trainItem.date}}</text></text>
					<view class="train-info">
						<view>培训名额：<text>{{trainItem.number}}</text></view>
						<view>培训方式：<text>{{trainItem.type}}</text></view>
						<view class="operate-btn">
							<view class="test" @click="test"><text>考试</text></view>
							<view class="divide"></view>
							<view class="train" @click="train"><text>培训</text></view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				newsItems: [{
					url: "../../static/image/home/news.png",
					mainTitle: "【促销活动】",
					subTitle: "AMD618促销狂欢季全新上线 ",
					date: "06.01 12:00-06.18 24:00"
				}, {
					url: "../../static/image/home/news.png",
					mainTitle: "【促销活动】",
					subTitle: "AMD618促销狂欢季全新上线 ",
					date: "06.01 12:00-06.18 24:00"
				}],
				bannerItems: [{
					url: '../../static/image/home/player.png',
					title: '玩家必看',
					jumpUrl: "/pages/playersMustSee/playersMustSee"
				}, {
					url: '../../static/image/home/uier.png',
					title: '设计师in',
					jumpUrl: "/pages/designerIn/designerIn"
				}, {
					url: '../../static/image/home/InterneBar.png',
					title: '网吧专家',
					jumpUrl: "/pages/internetBar/internetBar"
				}],
				trainItem: {
					topPic: '../../static/image/home/train.png',
					title: 'AMD最新三代处理器全渠道销售培训',
					status: '刚刚',
					date: '06.04 8:00-06.06 15:00',
					number: '50人',
					type: '视频'
				},

			}
		},
		methods: {
			indexNotice() {
				console.log('indexNotice');
			},
			test() {
				console.log('考试');
			},
			train() {
				console.log('培训');
			},
			bannerJump(url) {
				console.log("TCL: bannerJump -> url", url)
				uni.navigateTo({
					url,
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.index-container {
		/* padding: 20px; */
		font-size: 14px;
		line-height: 24px;

		.icon {
			width: 28upx;
			height: 23upx;
			margin-right: 9upx;
		}

		.index-notice {
			height: 126upx;
			margin: 0 30upx;
			padding: 16upx 20upx 30upx;
			background-color: #ffffff;
			border-radius: 20upx;
			position: relative;
			top: -10upx;

			.header-title {
				line-height: 1;


				text {
					font-size: 28upx;
					font-weight: Bold;
					font-stretch: normal;
					line-height: 36upx;
					letter-spacing: 0px;
					color: #333333;
				}
			}

			.header-desc {
				margin-top: 30upx;
				font-family: PingFang-SC-Medium;
				font-size: 24upx;
				font-weight: normal;
				font-stretch: normal;
				line-height: 1;
				letter-spacing: 0upx;
				color: #666666;
				display: flex;
				justify-content: space-between;

				text {
					display: block;
				}
			}
		}

		// 公共头部
		.common-header {
			padding: 30rpx 0;
			line-height: 1;

			text {
				font-size: 30rpx;
				font-weight: bold;
				font-stretch: normal;
				letter-spacing: 0px;
				color: #333333;
			}
		}

		// 活动资讯
		.index-news {
			padding: 0 30rpx 20rpx 30rpx;
			background: #fff;

			.scroll-news {
				display: flex;
				justify-content: space-between; //40rpx

				.news-items {
					width: 325rpx;
					background-color: #ffffff;
					border-radius: 10rpx;
					box-shadow: 2rpx 2rpx 10rpx 2rpx rgb(212, 212, 212);

					image {
						height: 171rpx;
						border-radius: 10rpx 10rpx 0 0;
					}

					.items-desc {
						background-color: #ffffff;
						border-radius: 0rpx 0rpx 10rpx 10rpx;
						padding: 0 10rpx;
						line-height: 1.4;

						.title {
							color: #333;
							text-align: left;

							text {
								color: #ff4c4c;
								font-weight: bold;
							}
						}

						.date {
							display: block;
							font-size: 18rpx;
							font-weight: 500;
							font-stretch: normal;
							line-height: 26rpx;
							letter-spacing: 2px;
							color: #f36523;
							margin: 20rpx 0 20rpx 0;
						}
					}
				}
			}
		}

		//banner 
		.index-banner {
			padding: 20rpx 30rpx;
			display: flex;
			justify-content: space-between;
			height: 129rpx;
			box-sizing: content-box;
			background: #fff;
			margin-top: 10rpx;

			.item {
				position: relative;
				width: 216rpx;
				height: 129rpx;
				border-radius: 10px;

				image {
					width: 216rpx;
					height: 129rpx;
				}

				// text {
				// 	position: absolute;
				// 	bottom: 20rpx;
				// 	font-size: 32rpx;
				// 	font-weight: bold;
				// 	color: rgba(255, 255, 255, 1);
				// }
			}
		}

		.index-train {
			background: #fff;
			margin-top: 10rpx;
			padding: 0 30rpx 30rpx 30rpx;

			.train-content {
				// height: 414rpx;
				border-radius: 10rpx;

				image {
					width: 100%;
					height: 414rpx;
					display: block;
				}

				.main {
					.header {
						display: flex;
						justify-content: space-between;
						margin-top: 26rpx;
						margin-bottom: 30rpx;
						line-height: 1;

						image {
							width: 20rpx;
							height: 20rpx;
						}

						text {
							font-size: 28rpx;
							font-weight: bold;
							color: #333;
							line-height: 1;
						}

						>view {
							display: flex;
							justify-content: space-between;
							align-items: center;

							.time-status {
								font-size: 22rpx;
								font-family: PingFang;
								font-weight: 500;
								color: #bbb;
								margin-left: 10rpx;
							}
						}

					}

					.date {
						display: inline-block;
						color: #666;
						line-height: 1;

						text {
							color: #F36523
						}
					}

					.train-info {
						color: #666;
						display: flex;
						justify-content: space-between;

						text {
							color: #F36523
						}

						.operate-btn {
							width: 222rpx;
							height: 44rpx;
							display: flex;
							justify-self: space-between;
							position: relative;

							.test {
								flex: 1;
								background: #f36523;
								border-radius: 22rpx 0 0 22rpx;
								text-align: center;

								text {
									color: #fff;
								}
							}

							.divide {
								width: 5rpx;
								height: 48rpx;
								transform: rotate(30deg);
								position: absolute;
								background: #fff;
								left: 50%;
								top: -2rpx;
							}

							.train {
								flex: 1;
								background: #f36523;
								border-radius: 0 22rpx 22rpx 0;
								text-align: center;

								text {
									color: #fff;
								}
							}
						}
					}
				}
			}
		}
	}
</style>