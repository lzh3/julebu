<template>
	<view>
		<view class="activity-wrap container" v-if='showLogin'>
			<NotLogin pageName="培训"></NotLogin>
		</view>
		<view class="train container" v-else>
			<wuc-tab :tab-list="tabList" :tabCur.sync="TabCur" @change="tabChange" :textFlex="true">
			</wuc-tab>
			<swiper class="swiper" :current="TabCur" duration="300" @change="swiperChange">
				<swiper-item class="s-item" v-for="(item,index) in tabList" :key="index">
					<view class="item" v-for="(v,index) in item.list" :key="index" @click="train(v)">
							<view class="content">
								<view class="top-pic">
									<image :src="v.image" mode="" class="main-pic" />
									<view :class="['jiao-biao',v.trainStatus]">{{trainStatus(v.trainStatus)}}</view>
								</view>
								<view class="text-area">
									<text class="title">{{v.title}}</text>
									<view class="train-time">培训时间：<text>{{v.start_time}}-{{v.end_time}}</text></view>
									<view class="train-info">
										<view>培训名额：<text>{{v.quota_count}}</text></view>
										<view class="type">培训方式：<text>{{v.mode == 1 ? "视频" : "课件"}}</text></view>
									</view>
								</view>
						</view>
						<view class="operate">
							<view class="time-wrap">
								<image src="../../static/image/home/time.png" mode="" />
								<text class="time">{{v.time}}</text>
								<text :class="['sign-number',v.trainStatus === 'overtime' && 'sign-number-failed']">已有{{v.sign_count}}人报名参加</text>
							</view>
							<view :class="['operate-btn',v.trainStatus === 'overtime' && 'overtime-btn']">
								<view class="test" @click.stop="test(v)"><text>考试</text></view>
								<view class="divide"></view>
								<view class="train-btn"  v-if="v.mode!=3"><text>培训</text></view>
							</view>
						</view>
					</view>
				</swiper-item>
			</swiper>
			<Kefu />
		</view>
	</view>
</template>

<script>
	import WucTab from '@/components/wuc-tab/wuc-tab.vue';
	import swiper from '@/components/wuc-tab/wuc-tab.vue';
	import Kefu from '@/components/Kefu'
	import NotLogin from "@/components/NotLogin";
	export default {
		data() {
			return {
				token: '',
				showLogin: false,
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
				trainedSignUrl: 'https://amd.mcooks.cn/api/trained/sign',
				trainedListUrl: 'https://amd.mcooks.cn/api/trained/list',
			}
		},
		components: {
			WucTab,
			Kefu,
			NotLogin
		},
		onShow() {
			console.log('onshow')
			let token = uni.getStorageSync('token')
			if (token) {
				this.token = token
				this.getTrainList()
				this.showLogin = false;
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
			tabChange(index) {
				this.TabCur = index;
			},
			swiperChange(e) {
				let current = e.detail.current
				this.TabCur = current;
			},
			// 报名培训 
			trainedSign(id, cb) {
				let that = this
				uni.request({
					url: that.trainedSignUrl,
					method: 'post',
					header: {
						'authtoken': 'token ' + this.token,
					},
					data: {
						id
					},
					success(res) {
						console.log('trainedSign --------------> ', res.data)
						cb()
					}
				})
			},
			// 考试
			test(item) {
        console.log("test -> item", item)
				this.trainedSign(item.id, () => {
					if (item.trainStatus !== 'overtime') {
						uni.navigateTo({
							url: `/pages/test-detail/test-detail?id=${item.id}`
						});
					}
				})

			},
			// 培训
			train(item) {
        console.log("train -> item", item)
				this.trainedSign(item.id, () => {
					if (item.status !== 'overtime') {
						uni.navigateTo({
							url: `/pages/course-detail/course-detail?id=${item.id}&type=${item.type}`
						});
					}
				})
			},
			trainStatus(status) {
				return {
					in_start: '已开始',
					notyet: '未开始',
					overtime: '已过期',
				} [status]
			},
			getTrainList() {
				let that = this
				uni.request({
					url: that.trainedListUrl,
					method: 'post',
					header: {
						'authtoken': 'token ' + this.token,
					},
					data: {
						page: that.page,
						limit: that.limit,
						type: that.type,
					},
					success(res) {
						console.log(res.data)
						that.tabList[that.TabCur].list = res.data.data // 季度培训
					}
				})
			}
		},
	}
</script>

<style scoped lang="scss">
	.activity-wrap{
		height:100vh;
		background-color: #fff;
	}
	.train {
		height: calc(100vh);
		background: $uni-bg-color-grey;

		/deep/.text-blue {
			color: #F36523
		}

		/deep/.uni-swiper-wrapper {
			height: calc(100vh - 90rpx);
		}

		.swiper {
			height: 100vh;
			overflow-y: scroll;
		}

		.s-item {
			height: 150rpx;
		}

		.item {
			padding: 30rpx 30rpx 0 30rpx;
			height: 260rpx;
			box-sizing: border-box;
			margin-bottom: 20rpx;
			background: #fff;

			.content {
				display: flex;
				justify-content: space-between;

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

					>view {
						margin: 15rpx 0;
					}

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

					.time {
						font-size: 22rpx;
						font-family: PingFang;
						font-weight: 500;
						color: #bbb;
						margin-left: 10rpx;
					}

					.sign-number {
						font-size: 22rpx;
						font-weight: 500;
						color: #ff4c4c;
						margin-left: 20rpx;
					}

					.sign-number-failed {
						color: #666;
					}
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

					.train-btn {
						flex: 1;
						background: #f36523;
						border-radius: 0 22rpx 22rpx 0;
						text-align: center;

						text {
							color: #fff;
						}
					}
				}

				.overtime-btn {

					.test,
					.train-btn {
						background: #666;
					}

				}
			}
		}

	}
</style>
