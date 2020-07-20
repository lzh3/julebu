<template>
	<view class="index-container">
		<Swiper :swiperItems="indexBanner" />
		<!-- 通知公告 -->
		<view class="index-notice" @click="indexNotice">
			<view class="header-title">
				<image src="../../static/image/home/notice.png" class="icon" />
				<text>通知公告</text>
			</view>
			<view class="header-content">
				<!-- <transition-group
          enter-active-class="animated fadeInUp"
          leave-active-class="animated fadeOutUp"
        > -->
				<view v-show="index == isCurrent" class="header-desc animated fadeInUp" v-for="(item,index) in notices"
					:key="item.title">
					<view class="info">{{item.title}}</view>
					<view class="date">{{item.date}}</view>
				</view>
				<!-- </transition-group> -->
			</view>
		</view>
		<!-- 最新资讯 -->
		<view class="index-news">
			<view class="common-header">
				<image src="../../static/image/home/activity.png" class="icon" />
				<text>最新资讯</text>
			</view>
			<view class="scroll-news">
				<view class="news-items" v-for="(item,index) in newsItems" :key="item.id" @click="newsItemClick(item)">
					<image :src="item.url" mode="" />
					<view class="items-desc">
						<view class="title">
							<text :style="{color:item.tag_color}">【{{item.tag}}】</text>{{item.title}}
						</view>
						<text class="date">{{item.desc}}</text>
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
			<block v-for="item in trainItem">
				<view class="train-content">
					<image :src="item.topPic" mode="aspectFit" />
					<view class="main">
						<view class="header">
							<text>{{item.title}}</text>
							<view>
								<image src="../../static/image/home/time.png" mode="" />
								<text class="time-status">{{item.time}}</text>
							</view>
						</view>
						<text class="date">培训时间：<text>{{item.start_time}} - {{item.end_time}}</text></text>
						<view class="train-info">
							<view>培训名额：<text>{{item.quota_count}}</text></view>
							<view>培训方式：<text>{{item.mode === 1 ? '视频培训' : '课件培训'}}</text></view>
							<view class="operate-btn">
								<view class="test" @click="test" v-if="item.exam"><text>考试</text></view>
								<view class="divide"></view>
								<view class="train" @click="train"><text>培训</text></view>
							</view>
						</view>
					</view>
				</view>
			</block>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				indexBanner: [],
				newsItems: [],
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
				trainItem: [],
				notices: [], //通知
				isCurrent: 0
			};
		},
		methods: {
			indexNotice() {
				console.log("indexNotice");
			},
			test() {
				console.log("考试");
				uni.navigateTo({
					url: `/pages/test-detail/test-detail?id=${this.trainItem.id}`
				});
			},
			train() {
				console.log("培训");
				uni.navigateTo({
					url: `/pages/course-detail/course-detail?id=${this.trainItem.id}&type=${this.trainItem.type}`
				});
			},
			bannerJump(url) {
				console.log("TCL: bannerJump -> url", url);
				uni.navigateTo({
					url
				});
			},
			//点击资讯跳转
			newsItemClick(item) {
				console.log("TCL: newsItemClick -> item", item);
				uni.navigateTo({
					url: "/pages/activity-detail/activity-detail?id=1"
				});
			},
			autoplayNotice() {
				let length = this.notices.length;
				if (this.isCurrent >= length) {
					this.isCurrent = 0;
					this.autoplayNotice();
				} else {
					setTimeout(() => {
						this.isCurrent++;
						this.autoplayNotice();
					}, 3000);
				}
			}
		},
		mounted() {
			console.log("TCL: mounted -> 1", 1)
			uni.request({
				url: "https://amd.mcooks.cn/api/index", //仅为示例，并非真实接口地址。
				success: ({
					data
				}) => {
					if (data.code == 200) {
						let {
							banners,
							news,
							notices,
							train
						} = data.data;
						this.newsItems = news;
						this.indexBanner = banners;
						this.notices = notices;
						this.autoplayNotice();
						this.trainItem = train;
						console.log("TCL: mounted -> newsItems", this.newsItems);
						console.log("TCL: mounted -> bannerItems", this.bannerItems);
						console.log("TCL: mounted -> trainItem", this.trainItem);
					}
				}
			});
		}
	};
</script>

<style lang="scss" scoped>
	@import "./animate";

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
			overflow: hidden;

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
				font-size: 12px;
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

		.header-content {
			display: flex;
			flex-wrap: no-wrap;
			overflow: hidden;
			flex-direction: column;

			.info {
				overflow: hidden;
				white-space: nowrap;
				text-overflow: ellipsis;
			}

			.date {
				width: fit-content;
				white-space: nowrap;
			}

			.header-desc {
				min-width: 100%;
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
							color: #f36523;
						}
					}

					.train-info {
						color: #666;
						display: flex;
						justify-content: space-between;

						text {
							color: #f36523;
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