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
				<view v-if="index == isCurrent" class="header-desc animated fadeInUp" v-for="(item,index) in notices" :key="item.title">
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
					<image :src="item.image" mode="" />
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
				<image :src="item.url" mode="" @click="bannerJump(item.jumpUrl)" />
				<!-- <text>{{item.title}}</text> -->
			</view>
		</view>
		<!-- 最新培训 -->
		<view class="index-train">
			<view class="common-header">
				<image src="../../static/image/home/activity.png" class="icon" />
				<text>最新培训</text>
			</view>
			<block v-for="(item,index) in trainItem" :key='index'>
				<view class="train-content">
					<view class="main">
						<navigator :url="'/pages/course-detail/course-detail?id='+item.id">
							<image :src="item.image" mode="" class="new-train-pic" />
							<view class="header">
								<text>{{item.title}}</text>
								<view>
									<image src="../../static/image/home/time.png" mode="" />
									<text class="time-status">{{item.time}}</text>
								</view>
							</view>
							<text class="date">培训时间：<text>{{item.start_time}} - {{item.end_time}}</text></text>
						</navigator>
						<view class="train-info">
							<!-- <view>培训名额：<text class="ora">{{item.quota_count}}人</text></view> -->
							<view>培训方式：<text class="ora">{{item.mode === 1 ? '视频培训' : '课件培训'}}</text></view>
							<view class="operate-btn">
								<view class="test" @click="test(item)" v-if="item.exam"><text>考试</text></view>
								<view class="divide"></view>
								<view class="train" @click="train(item)"><text>培训</text></view>
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
		onLoad() {
			let token = uni.getStorageSync('token')
			this.getIdType(token)
			this.getData()
		},
		// 下拉刷新
		onPullDownRefresh() {
			console.log('refresh');
			this.getData()
		},
		methods: {
			indexNotice() {},
			test(item) {
				console.log(item)
				uni.navigateTo({
					url: `/pages/test-detail/test-detail?id=${item.id}`
				});
			},
			train(item) {
				uni.navigateTo({
					url: `/pages/course-detail/course-detail?id=${item.id}`
				});
			},
			bannerJump(url) {
				uni.navigateTo({
					url
				});
			},
			//点击资讯跳转
			newsItemClick(item) {
				const {event_id,id}=item
				/* uni.navigateTo({
					url: "/pages/activity-detail/activity-detail?id=1"
				}); */
				if(!event_id){ // 新闻详情页 传id 
					uni.navigateTo({
						url:'/pages/common/postdetail/postdetail?id='+id
					});
				}else{  // 活动详情页 传event_id
					uni.navigateTo({
						url: "/pages/activity-detail/activity-detail?id="+event_id
					});
				}
			},
			// 因为用户一进来就是首页，可以判断是什么用户类型
			getIdType(token) {
				let _this = this;
				if (token) {
					uni.request({
						url: 'https://amd.mcooks.cn/api/userinfo',
						// url: '/userinfo',
						method: 'get',
						header: {
							'authtoken': 'token ' + token,
						},
						success(res) {
							let id_type = 0 || (res.data.data && res.data.data.id_type);
							uni.setStorageSync('id_type', id_type)
						}
					})
				}
			},
			getData() {
				uni.request({
					url: "https://amd.mcooks.cn/api/index", //仅为示例，并非真实接口地址。
					success: ({
						data
					}) => {
						console.log(data)
						if (data.code == 200) {
							let {
								banners,
								news,
								notices,
								train,
							} = data.data;
							this.newsItems = news;
							this.indexBanner = banners;
							this.notices = notices;
							this.autoplayNotice();
							this.trainItem = train;
							setTimeout(() => {
								uni.stopPullDownRefresh();
							}, 500)
						}
					}
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
	};
</script>

<style lang="scss" scoped>
	@import "./animate";

	.index-container {
		/* padding: 20px; */
		font-size: 14px;
		line-height: 24px;

		.icon {
			width: 30upx;
			height: 30upx;
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
				font-size: 24rpx;
				color:#666;
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
								color: #f39d23;
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
			display: flex;
			padding:20rpx 30rpx;
			justify-content: space-around;
			box-sizing: content-box;
			background: #fff;
			margin-top: 10rpx;

			.item {
				position: relative;
				width: 220rpx;
				height: 136rpx;
				border-radius: 10px;

				image {
					width: 100%;
					height:100%;
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
					.new-train-pic {
						width: 100%;
					}

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

						.ora {
							color: #f39d23;
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
