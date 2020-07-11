<template>
	<view class="activity-detail">
		<image :src="item.pic" class="pic" />
		<view class="content">
			<view class="item">
				活动主题：
				<text class="theme">{{item.theme}}</text>
				<text class="status">{{item.status}}</text>
			</view>
			<view class="item">活动时间：<text class="date">{{item.date}}</text></view>
			<view class="item">活动形式：<text class="type">{{item.type}}</text></view>
			<view class="item">参与名额：<text class="limit">{{item.limit}}</text><text
					class="isReported">{{item.isReported}}</text>
			</view>
			<view class="item">活动介绍：<text class="desc">{{item.desc}}</text></view>
		</view>
		<button type="primary" @click="joinActivity">我要参与</button>
		<view class="kefu">
			<image src="../../static/image/kefu.png"></image>
			<text>客服</text>
		</view>
	</view>
</template>

<script>
	export default {

		data() {
			return {
				item: {
					pic: '../../static/image/activity-pic.png',
					status: '已开始',
					theme: 'AMD 618促销狂欢季全新上线',
					date: '06.01 12:00~06.18 24:00',
					type: '线上活动',
					limit: '50人',
					isReported: '40',
					desc: '以下是活动介绍以下是活动介绍以下是活动介绍以下是活动 介绍以下是活动介绍以下是活动介绍以下是活动介绍以下是 介绍以下是活动介绍',
				}
			};
		},
		//路由参数就收
		onLoad(opt) {
			console.log("TCL: onLoad -> opt", opt)
		},
		methods: {
			joinActivity() {

			}
		},
	}
</script>

<style lang="scss" scoped>
	.activity-detail {
		.pic {
			height: 350rpx;
			width: 100%;
		}

		.content {
			padding: 30rpx;
			background: #fff;

			.item {
				font-size: 26rpx;
				font-weight: 500;
				color: rgba(102, 102, 102, 1);

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
			}
		}
	}
</style>