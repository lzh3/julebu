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
			<view class="header">
				<image src="../../static/image/home/activity.png" class="icon" />
				<text>最新资讯</text>
			</view>
			<view class="scroll-news">
				<view class="news-items">

				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				scrollTop: 0,
				old: {
					scrollTop: 0
				}

			}
		},
		methods: {
			indexNotice() {
				console.log('indexNotice');
			},
			// 新闻滚动
			scrollNews(e) {
				console.log('scrollNews');
				console.log(e)
				this.old.scrollTop = e.detail.scrollTop
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

		// 活动资讯
		.index-news {}
	}
</style>