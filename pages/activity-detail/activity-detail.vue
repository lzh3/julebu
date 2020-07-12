<template>
	<view :class="['activity-detail',activityStatus]">
		<image :src="item.pic" class="pic" />
		<view class="content">
			<view class="item">
				活动主题：
				<text class="theme">{{item.theme}}</text>
				<text class="status" :class="{end:end}">{{item.status}}</text>
			</view>
			<view class="item">活动时间：<text class="date" :class="{end:end}">{{item.date}}</text></view>
			<view class="item">活动形式：<text class="type" :class="{end:end}">{{item.type}}</text></view>
			<view class="item">参与名额：<text class="limit"  :class="{end:end}">{{item.limit}}</text>
				<text class="isReported" :class="{end:end}">已有{{item.isReported}}人报名参加</text>
			</view>
			<view class="item">活动介绍：
				<view class="desc">{{item.desc}}</view>
			</view>
			<button type="primary" 
				@click="joinActivity" 
				class="btn" 
				v-if="joined"
				:class="{over:end}"
				:disabled="activityStatus !== 'online'">
				我要参与
			</button>
		</view>
		<Kefu />
		<Modal ref="modal" :status="modalStatus" title="参与成功" desc="" />
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
				joined:true,
				end:true,
				item: {
					pic: '../../static/image/activity-pic.png',
					status: '已开始',
					theme: 'AMD 618促销狂欢季全新上线',
					date: '06.01 12:00~06.18 24:00',
					type: '线上活动',
					limit: '50人',
					isReported: '40',
					ing:'true',
					desc: '以下是活动介绍以下是活动介绍以下是活动介绍以下是活动 介绍以下是活动介绍以下是活动介绍以下是活动介绍以下是 介绍以下是活动介绍',
				},
				modalStatus: 'success',
				activityStatus: 'online', //活动状态 online 在线 not-yet还没开始 over过期了
			};
		},
		//路由参数就收
		onLoad(opt) {
			console.log(opt)
			if(opt.joined==='false'){
				this.joined=false;
			}
			
		},
		methods: {
			joinActivity() {
				this.$refs.modal.open()
			},

			login() {

			},

		},
	}
</script>

<style lang="scss" scoped>
	.over{
		background-color: #5E5C5C;
	}
	.activity-detail {
		height: calc(100vh - 88rpx);
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

			.item {
				font-size: 26rpx;
				font-weight: 500;
				color: rgba(102, 102, 102, 1);
				margin-bottom: 30rpx;
				line-height: 1;

				.theme {
					font-size: 26rpx;
					font-weight: bold;
					color: rgba(51, 51, 51, 1);
				}

				.status {
					font-size: 22rpx;
					font-weight: 500;
					color: rgba(243, 157, 35, 1);
					float: right;
				}

				.date,
				.type {
					font-size: 26rpx;
					font-weight: 500;
					color: rgba(243, 101, 35, 1);
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
				.end{
					color:#333;
				}
			}

			.btn {
				height: 64rpx;
				line-height: 64rpx;
				background-color: rgba(243, 101, 35, 1);
				border-radius: 10rpx;
				position: absolute;
				bottom: 20rpx;
				width: calc(100% - 60rpx);
			}
		}
	}

	.not-yet,
	.over {
		filter: grayscale(100%);
	}
</style>