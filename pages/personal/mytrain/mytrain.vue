<template>
	<view class="train">
		<wuc-tab :tab-list="tabList" :tabCur.sync="TabCur" @change="tabChange" :textFlex="true">
		</wuc-tab>
		<swiper :current="TabCur" duration="300" @change="swiperChange" class="my-train-swiper">
			<swiper-item v-for="(item,index) in tabList" :key="index" class="my-train-swiper-item">
				<view v-if="item.list.length==0">
					<no-data />
				</view>
				<view v-else class="item" v-for="(v,index) in item.list" :key="index" @click="viewTrain" :data-item="JSON.stringify(v)">
					<view class="content">
						<view class="top-pic">
							<image :src="v.image" mode="" class="main-pic" />
							<!-- <view :class="['jiao-biao',v.trainStatus]">{{trainStatus(v.trainStatus)}}</view> -->
						</view>
						<view class="text-area">
							<text class="title">{{v.title}}</text>
							<view class="train-time">培训时间：<text>{{v.start_time}}-{{v.end_time}}</text></view>
							<view class="train-info">
								<!-- <view>培训名额：<text>{{v.quota_count}}</text></view> -->
								<view class="type">培训方式：<text>{{v.mode == 1 ? "视频" : "课件"}}</text></view>
							</view>
						</view>
					</view>
					<view class="operate">
						<view class="time-wrap scope">
							<text class="time" v-if="v.exam_btn==0">得分：{{v.answers.scores}}</text>
						</view>
						<view :class="['operate-btn',v.trainStatus]">
							<view class="exam" v-if="v.exam_btn==1">
								<view @click.stop="exam" :data-item="JSON.stringify(v)" v-show="v.exam==1">
									考试
								</view>
							</view>
							<view class="traninandscore" v-else>
								<view class="test"><text>查看培训</text></view>
								<!-- v-if='v.done' -->
								<view class="train-btn" @click.stop="lookResult(v)"><text>查看成绩</text></view>
							</view>
						</view>
					</view>
				</view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	import WucTab from '@/components/wuc-tab/wuc-tab.vue';
	import swiper from '@/components/wuc-tab/wuc-tab.vue';
	import Kefu from '@/components/Kefu'
	import NoData from '../../common/nodata.vue'
	export default {
		data() {
			return {
				token: null,
				TabCur: 0,
				page: 1,
				limit: 6,
				type: 1, //培训方式 1、季度培训 2、产品知识 3、ACCSP
				tabList: [{
						name: '季度培训',
						list: [],
						type: 1,
					},
					{
						name: '产品知识',
						list: [],
						type: 2,
					},
					{
						name: 'ACCSP认证',
						list: [],
						type: 3,
					}
				],
				plateformType: '',
				signTrainedUrl: this.plateformType == 'h5' ? '/sign-trained' : 'https://amd.mcooks.cn/api/sign-trained'
			}
		},
		components: {
			WucTab,
			Kefu,
			NoData
		},
		onShow() {
			console.log('onshow')
			let token = uni.getStorageSync('token')
			if (token) {
				this.token = token
				this.getTrainList()
			} else {
				this.showLogin = true
			}
		},
		watch: {
			TabCur(n) {
				// 没数据再请求
				if (this.tabList[n].list.length == 0) {
					this.type = n + 1
					this.getTrainList()
				}
			}
		},
		methods: {
			getTrainList() {
				let that = this
				uni.request({
					url: that.signTrainedUrl,
					method: 'post',
					header: {
						'authtoken': 'token ' + this.token,
					},
					data: {
						page: that.page,
						limit: that.limit,
						type: this.TabCur + 1
					},
					success(res) {
						console.log(res.data)
						that.tabList[that.TabCur].list = res.data.data // 季度培训
					}
				})
			},
			tabChange(index) {
				this.TabCur = index;
			},
			swiperChange(e) {
				let current = e.detail.current
				this.TabCur = current;
			},
			// 查看培训
			viewTrain(event) {
				let id = JSON.parse(event.currentTarget.dataset.item).id
				uni.navigateTo({
					url: `/pages/course-detail/course-detail?id=${id}`
				});
			},
			// 培训
			lookResult(item) {
				uni.navigateTo({
					url: `/pages/personal/mytrain/result?item=${JSON.stringify(item)}`
				});
			},
			trainStatus(status) {
				return {
					online: '已开始',
					notyet: '未开始',
					overtime: '已过期',
				} [status]
			},
			// 考试按钮
			exam(event) {
				let id = JSON.parse(event.currentTarget.dataset.item).id
				uni.navigateTo({
					url: '../../test-detail/test-detail?id=' + id
				})
			}
		},
	}
</script>

<style scoped lang="scss">
	.train {
		height: calc(100vh - 88rpx);
		background: $uni-bg-color-grey;

		/deep/.text-blue {
			color: #F36523
		}

		/deep/.uni-swiper-wrapper {
			height: calc(100vh - 90rpx);
		}

		.item {
			padding: 30rpx 30rpx 0 30rpx;
			height: 263rpx;
			box-sizing: border-box;
			margin-bottom: 20rpx;
			background: #fff;

			.content {
				display: flex;
				justify-content: space-between;
				overflow: hidden;

				.top-pic {
					margin-right: 20rpx;
					position: relative;

					.main-pic {
						width: 250rpx;
						height: 150rpx;
						border-radius: 10rpx;
					}

					.jiao-biao {
						color: #fff;
						font-size: 24rpx;
						font-weight: 500;
						line-height: 2em;
						width: 6em;
						position: absolute;
						left: 0;
						top: 0;
						z-Index: 2;
						transform-origin: right bottom;
						transform: translate(-29.29%, -100%) rotate(-45deg);
						text-indent: 0;
						text-align: center;
					}

					.online {
						background-color: #3AA608
					}

					.notyet {
						background-color: #F36523
					}

					.overtime {
						background-color: rgb(116, 116, 116)
					}
				}

				.text-area {
					flex: 1;

					.title {
						font-size: 26rpx;
						font-weight: bold;
						color: rgba(51, 51, 51, 1);
						white-space: nowrap;
					}

					.train-time {
						font-size: 26rpx;
						font-weight: 500;
						color: rgba(102, 102, 102, 1);

						text {
							font-size: 26rpx;
							font-weight: 500;
							color: rgba(243, 101, 35, 1);
						}
					}

					.train-info {
						color: rgba(102, 102, 102, 1);
						display: flex;
						justify-content: space-between;

						view {
							width: 50%;
						}

						text {
							color: #F39D23
						}
					}

				}
			}

			.operate {
				display: flex;
				justify-content: space-between;

				.time-wrap {
					image {
						width: 20rpx;
						height: 20rpx;
					}

					.score {
						line-height: 34rpx;
						background: #f36523;
						text-align: center;
						color: #fff;
					}

					.time {
						font-size: 22rpx;
						font-family: PingFang;
						font-weight: 500;
						color: #f36523;
						margin-left: 10rpx;
					}

					.sign-number {
						font-size: 22rpx;
						font-weight: 500;
						color: rgba(255, 76, 76, 1);
						margin-left: 20rpx;
					}

					.sign-number-failed {
						color: #666;
					}
				}

				.operate-btn {
					height: 44rpx;
					display: flex;
					justify-self: space-between;
					position: relative;

					view {
						width: 130rpx;
						margin: 0 10rpx;
						border-radius: 6rpx;
					}

					.traninandscore {
						width: 300rpx;

						view {
							display: inline-block;
						}
					}

					.exam {
						width: 120rpx;
						text-align: center;
						color: #fff;

						view {
							width: 100%;
							height: 100%;
							padding: 0 10rpx;
							background: #f36523;
						}
					}

					.test {
						flex: 1;
						background: #f36523;
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

					.train-btn {
						flex: 1;
						background: #f36523;
						text-align: center;

						text {
							color: #fff;
						}
					}
				}

				.overtime-btn {
					view {
						margin: 0 10rpx;
					}

					.test,
					.train-btn {
						background: #666;
					}

				}
			}
		}
	}

	.my-train-swiper {
		height: 100%;
	}
</style>
