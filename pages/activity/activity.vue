<template>
	<view>
		<view v-if="list.length==0">
			<no-data />
		</view>
		<view v-else class="cu-card article no-card" :style='{display:showCard}' v-for="(item,key) in list" :key='key'>
			<view class="cu-item shadow">
				<navigator :url="'../activity-detail/activity-detail?id='+item.id">
					<view class="content">
						<view class="pic">
							<image class="img" :src="item.image" mode=""></image>
							<image class="on" v-if='item.status==="in_start"' src="../../static/image/activity/in_start.png" mode="">
							</image>
							<image class="on" v-else-if="item.status==='not_start'" src="../../static/image/activity/not_start.png" mode=""></image>
							<image class="on" v-else-if="item.status==='end'" src="../../static/image/activity/end.png" mode="">
							</image>

						</view>
						<view class="desc">
							<view class="main-title">
								<text class="name">主题:</text> <text class="txt">{{item.title}}</text>
							</view>
							<view class="text-content">
								<text class='name'>时间:</text>
								<text class="main-color" :class="{name:item.status=='end'}">
									{{item.start_time}}-{{item.end_time}}
								</text>
							</view>
							<view class="text-content ">
								<!-- <text class="name">活动名额:</text>
								<text class="ora"  :class="{name:item.status=='end'}"> {{item.quota_count}}人</text> -->
								<text class="f-right">已有{{item.partic_count}}人参与</text>
							</view>
						</view>
					</view>
				</navigator>
			</view>
		</view>

		<view class="activity-wrap container" :style='{display:showLogin}'>
			<NotLogin pageName="活动"></NotLogin>
		</view>
	</view>
</template>

<script>
	import NotLogin from "@/components/NotLogin";
	import NoData from '../common/nodata.vue'
	export default {
		components: {
			NotLogin,
			NoData
		},
		data() {
			return {
				token: '',
				showCard: '',
				showLogin: '',
				list: []
			}
		},
		onShow() {
			let token = uni.getStorageSync('token')
			if (token == '') {
				this.showLogin = "block"
				this.showCard = 'none'
			} else {
				this.token = token;
				this.getActivityList(1, token);
				this.showLogin = "none";
				this.showCard = 'block'
			}
		},
		// 下拉刷新
		onPullDownRefresh() {
			this.getActivityList(1, this.token)
		},
		methods: {
			// 获取活动列表
			getActivityList(page, token) {
				let _this = this;
				uni.request({
					url: 'https://amd.mcooks.cn/api/events/list',
					method: 'POST',
					header: {
						'authtoken': 'token ' + token,
					},
					data: {
						page,
						limit: 10
					},
					success(res) {
						console.log(res.data)
						_this.list = res.data.data;
						setTimeout(() => {
							uni.stopPullDownRefresh();
						}, 500)
					},
					fail(e) {}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.main-title {
		width: 460rpx;
		word-wrap: wrap;
		line-height:40rpx;
		/* white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis; */
	}

	.activity-wrap {
		height: 100vh;
		background-color: #fff;
	}

	.cu-card {
		margin-bottom: 6rpx;
	}

	.cu-item {
		padding-top: 30rpx;

		.pic {
			position: relative;
			width: 150rpx;
			height: 150rpx;
			border-radius: 6px;
			background-color: lightblue;

			.img {
				width: 100%;
				height: 100%;
				border-radius: 6px;
			}

			.on {
				width: 105rpx;
				height: 75rpx;
				position: absolute;
				left: 0;
				top: 0;
			}
		}

		.content {
			.name {
				color: #666;
			}

			.desc {
				padding: 0;
				margin-left: 10rpx;

				view.text-content {
					height: 40rpx;
					line-height: 40rpx;

					.txt {
						color: #333333;
					}

					.ora {
						color: #f39d23;
					}

					.f-right {
						float: right;
					}
				}
			}
		}
	}
</style>
