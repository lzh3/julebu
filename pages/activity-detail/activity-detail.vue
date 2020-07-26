<template>

	<view class="container" :class="['activity-detail',activityStatus]">
		<view class="activity-wrap container" v-if="isLogin">
			<NotLogin pageName="活动"></NotLogin>
		</view>
		<view v-else class="detail">
			<image :src="item.image" class="pic" />
			<view class="content">
				<view class="item">
					<view class="maintitle">
						活动主题：
						<text class="theme">{{item.title}}</text>
					</view>
					<view class="status">
						<text>
							{{item.status==='in_start'?'进行中':(item.status=='end'?'已结束':'未开始')}}
						</text>
					</view>
				</view>
				<view class="item">活动时间：
					<text class="date" :class="{end:item.status=='end'}">
						{{item.start_time}}-{{item.end_time}}
					</text>
				</view>
				<view class="item">
					活动形式：<text class="type ora" :class="{end:end}">{{item.type==1?'线上活动':'其它'}}</text>
				</view>
				<!-- <view class="item">参与名额：<text class="limit"  :class="{end:end}">{{item.limit}}</text>
					 <text class="isReported" :class="{end:end}">已有{{item.isReported}}人报名参加</text> 
				</view> -->
				<view class="item">
					活动介绍：<view class="desc" v-html='item.description'></view>
				</view>
				<button type="primary" @click="joinActivity" class="btn" v-if="joined" :class="{over:item.status!=='in_start'}"
					:disabled="item.status!=='in_start'">
					我要参与
				</button>
			</view>
			<Kefu />
		</view>
		<Modal ref="modal" :status="modalStatus" :title="modalTitle" :desc="modalDesc" :btn="false" />
	</view>
</template>

<script>
	import Kefu from '@/components/Kefu'
	import Modal from '@/components/Modal'
	export default {
		components: {
			Kefu,
			Modal
		},
		data() {
			return {
				isLogin: true,
				token: '',
				id: -1,
				joined: true,
				end: true,
				item: {
					/* pic: '../../static/image/activity-pic.png',
					status: '已开始',
					theme: 'AMD 618促销狂欢季全新上线',
					date: '06.01 12:00~06.18 24:00',
					type: '线上活动',
					limit: '50人',
					isReported: '40',
					ing:'true',
					desc: '以下是活动介绍以下是活动介绍以下是活动介绍以下是活动 介绍以下是活动介绍以下是活动介绍以下是活动介绍以下是 介绍以下是活动介绍', */
				},
				modalStatus: 'success',
				modalTitle: '参与成功',
				modalDesc: '',
				activityStatus: 'online', //活动状态 online 在线 not-yet还没开始 over过期了
			};
		},
		//路由参数就收
		onLoad(opt) {
			let token = uni.getStorageSync('token')
			if (token) {
				this.isLogin = false;
			}
			this.token = token;
			console.log(opt)
			this.id = opt.id;
			if (opt.joined) {
				this.joined = false;
			}
			this.getDetail(opt.id, token)
		},
		methods: {
			showJoin(status) {
				console.log(status)
			},
			joinActivity() {
				let _this = this;
				uni.request({
					url: 'https://amd.mcooks.cn/api/event/sign',
					method: 'POST',
					header: {
						'authtoken': 'token ' + _this.token,
					},
					data: {
						id: _this.id
					},
					success(res) {
						console.log(res.data)
						if (res.data.code == 200) {
							_this.modalStatus = 'success'
							_this.modalTitle = res.data.msg
						} else {
							_this.modalStatus = 'error'
							_this.modalTitle = res.data.msg
						}
						_this.$refs.modal.open();
					}
				})
			},
			getDetail(id, token) {
				let _this = this;
				uni.request({
					url: 'https://amd.mcooks.cn/api/events/show',
					method: 'POST',
					header: {
						'authtoken': 'token ' + token,
					},
					data: {
						id
					},
					success(res) {
						console.log(res.data)
						if (res.data.data.is_sign == 0) {
							_this.end = false;
						}
						_this.item = res.data.data;
					}
				})
			}
		},
	}
</script>

<style lang="scss" scoped>
	.detail {
		width: 100%;
		height: 100%;
	}

	.over {
		background-color: #5E5C5C;
	}

	.detail {
		display: flex;
		flex-direction: column;
		justify-content: flex-start;

		.pic {
			height: 350rpx;
			width: 100%;
		}

		.content {
			position: relative;
			padding: 30rpx;
			background: #fff;
			flex: 1;
			height: calc(100vh-350rpx);

			.item {
				font-size: 26rpx;
				font-weight: 500;
				color: rgba(102, 102, 102, 1);
				margin-bottom: 30rpx;
				line-height: 1;
				&::after{
					content: '';
					display: block;
					height: 0;
					clear: both;
				}
				.maintitle{
					width:560rpx;
					float: left;
				}
				.theme {
					font-size: 26rpx;
					font-weight: bold;
					color: #333;
					line-height: 34rpx;
					width:330rpx;
				}

				.status {
					font-size: 22rpx;
					font-weight: normal;
					color: rgba(243, 157, 35, 1);
					float: right;
					line-height: 34rpx;
				}


				.limit {
					font-size: 26rpx;
					font-weight: 500;
					color: rgba(243, 157, 35, 1);
				}

				.isReported {
					font-size: 22rpx;
					font-weight: 500;
					color: rgba(255, 76, 76, 1);
					float: right;
				}

				.desc {
					margin-top: 30rpx;
					font-size: 26rpx;
					font-weight: 500;
					color: rgba(51, 51, 51, 1);
					line-height: 48rpx;
				}

				.end {
					color: #333;
				}

				.date,
				.type {
					font-size: 26rpx;
					font-weight: 500;
					color: rgba(243, 101, 35, 1);
				}

				.ora {
					color: rgba(243, 157, 35, 1);
				}
			}

			.btn {
				height: 64rpx;
				line-height: 64rpx;
				background-color: rgba(243, 101, 35, 1);
				border-radius: 10rpx;
				position: fixed;
				width:92%;
				bottom: 10rpx;
				left:4%;
			}
		}
	}

	.not-yet,
	.over {
		filter: grayscale(100%);
	}
</style>