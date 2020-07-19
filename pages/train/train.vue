<template>
	<view>
		<view class="activity-wrap" v-if='showLogin'>
			<NotLogin pageName="培训"></NotLogin>
		</view>
		<view class="train">
			<wuc-tab :tab-list="tabList" :tabCur.sync="TabCur" @change="tabChange" :textFlex="true">
			</wuc-tab>
			<swiper :current="TabCur" duration="300" @change="swiperChange">
				<swiper-item v-for="(item,index) in tabList" :key="index">
					<view class="item" v-for="(v,index) in item.list" :key="index">
						<view class="content">
							<view class="top-pic">
								<image :src="v.topPic" mode="" class="main-pic" />
								<view :class="['jiao-biao',v.trainStatus]">{{trainStatus(v.trainStatus)}}</view>
							</view>
							<view class="text-area">
								<text class="title">{{v.title}}</text>
								<view class="train-time">培训时间：<text>{{v.date}}</text></view>
								<view class="train-info">
									<view>培训名额：<text>{{v.limit}}</text></view>
									<view class="type">培训方式：<text>{{v.type}}</text></view>
								</view>
							</view>
						</view>
						<view class="operate">
							<view class="time-wrap">
								<image src="../../static/image/home/time.png" mode="" />
								<text class="time">{{v.time}}</text>
								<text
									:class="['sign-number',v.trainStatus === 'overtime' && 'sign-number-failed']">已有{{v.signNumber}}人报名参加</text>
							</view>
							<view :class="['operate-btn',v.trainStatus === 'overtime' && 'overtime-btn']">
								<view class="test" @click="test(v)"><text>考试</text></view>
								<view class="divide"></view>
								<view class="train-btn" @click="train(v)"><text>培训</text></view>
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
	export default {
		data() {
			return {
				token: '',
				showLogin: false,
				TabCur: 0,
				tabList: [{
						name: '季度培训',
						list: [{
								id: Date.now(),
								topPic: '../../static/image/train-pic.png',
								title: 'AMD最新三代处理器全渠道销售培训',
								date: '06.04 8:00-06.06 12:00',
								limit: 100,
								type: '视频',
								time: '刚刚',
								signNumber: 90,
								trainStatus: 'online', //已经开始
							},
							{
								id: Date.now(),
								topPic: '../../static/image/train-pic.png',
								title: 'AMD最新三代处理器全渠道销售培训',
								date: '06.04 8:00-06.06 12:00',
								limit: 50,
								type: '课件',
								time: '刚刚',
								signNumber: 90,
								trainStatus: 'notyet', //未开始
							},
							{
								id: Date.now(),
								topPic: '../../static/image/train-pic.png',
								title: 'AMD最新三代处理器全渠道销售培训',
								date: '06.04 8:00-06.06 12:00',
								limit: 10,
								type: '课件',
								time: '刚刚',
								signNumber: 99,
								trainStatus: 'overtime', //过期
							},
						]
					},
					{
						name: '产品知识',
						list: [{
								id: Date.now(),
								topPic: '../../static/image/train-pic.png',
								title: '产品知识',
								date: '06.04 8:00-06.06 12:00',
								limit: 100,
								type: '视频',
								time: '刚刚',
								signNumber: 90,
								trainStatus: 'online', //已经开始
							},
							{
								id: Date.now(),
								topPic: '../../static/image/train-pic.png',
								title: '产品知识',
								date: '06.04 8:00-06.06 12:00',
								limit: 50,
								type: '课件',
								time: '刚刚',
								signNumber: 90,
								trainStatus: 'notyet', //未开始
							},
							{
								id: Date.now(),
								topPic: '../../static/image/train-pic.png',
								title: '产品知识',
								date: '06.04 8:00-06.06 12:00',
								limit: 10,
								type: '课件',
								time: '刚刚',
								signNumber: 99,
								trainStatus: 'overtime', //过期
							},
						]
					},
					{
						name: 'ACCSP认证',
						list: [{
								id: Date.now(),
								topPic: '../../static/image/train-pic.png',
								title: 'ACCSP认证',
								date: '06.04 8:00-06.06 12:00',
								limit: 100,
								type: '视频',
								time: '刚刚',
								signNumber: 90,
								trainStatus: 'online', //已经开始
							},
							{
								id: Date.now(),
								topPic: '../../static/image/train-pic.png',
								title: 'ACCSP认证',
								date: '06.04 8:00-06.06 12:00',
								limit: 50,
								type: '课件',
								time: '刚刚',
								signNumber: 90,
								trainStatus: 'notyet', //未开始
							},
							{
								id: Date.now(),
								topPic: '../../static/image/train-pic.png',
								title: 'ACCSP认证',
								date: '06.04 8:00-06.06 12:00',
								limit: 10,
								type: '课件',
								time: '刚刚',
								signNumber: 99,
								trainStatus: 'overtime', //过期
							},
						]
					}
				],
			}
		},
		components: {
			WucTab,
			Kefu
		},
		onShow() {
			let token = uni.getStorageSync('token')
			if (token) {
				this.token = token
				this.getTrainList();
			} else {
				this.showLogin = true
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
			// 考试
			test(item) {
				if (item.trainStatus !== 'overtime') {
					uni.navigateTo({
						url: `/pages/test-detail/test-detail?id=${item.id}`
					});
				}
			},
			// 培训
			train(item) {
				if (item.trainStatus !== 'overtime') {
					uni.navigateTo({
						url: `/pages/course-detail/course-detail?id=${item.id}&type=${item.type}`
					});
				}
			},
			trainStatus(status) {
				return {
					online: '已开始',
					notyet: '未开始',
					overtime: '已过期',
				} [status]
			},
			getTrainList() {
				console.log("TCL: getTrainList -> this.token", this.token)
				let that = this
				uni.request({
					url: '/trained/list',
					method: 'post',
					header: {
						'authtoken': 'token ' + this.token,
					},
					success(res) {
						console.log('train-----------', res.data)
						that.tabList[0].list = res.data.data // 季度培训
					}
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
						color: rgba(255, 76, 76, 1);
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