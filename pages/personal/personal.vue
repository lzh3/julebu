<template>
	<view class="container">
		<view class="info-card">
			<view class="info">
				<view class="touxiang">
					<!-- <image src="" mode=""></image> -->
				</view>
				<view class="status">
					<view class="name">
						name ID:-
					</view>
					<view class="jifen">
						积分：0
					</view>
				</view>
			</view>
			<view class="login" @click="loginFn">立即登录</view>
		</view>
		<view class="list">
			<view class="list-card bg-white">
				<view class="item" v-for="item in list">
					<navigator :url='item.url'>
						<view class="img">
							<image :src="item.icon" mode=""></image>
						</view>
						<view class="txt">{{item.title}}</view>
					</navigator>
				</view>
			</view>
		</view>
		<!-- <view class="reward">
			<view class="reward-title">
				<image src="../../static/image/personal/reward.png" mode=""></image>
				我的奖励
			</view>
		</view> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [
					{
						icon: '../../static/image/icon/personal/1.png',
						title: '店铺详情'
					},
					{
						icon: '../../static/image/icon/personal/2.png',
						title: '店面形象'
					},
					{
						icon: '../../static/image/icon/personal/3.png',
						title: '店员管理'
					},
					{
						icon: '../../static/image/icon/personal/4.png',
						title: '我的活动'
					},
					{
						icon: '../../static/image/icon/personal/5.png',
						title: '我的培训'
					},
					{
						icon: '../../static/image/icon/personal/6.png',
						title: '我的帖子',
						url:'./mypost/mypost'
					},
					{
						icon: '../../static/image/icon/personal/7.png',
						title: '我的积分',
						url:'./myjf/myjf'
					},
					{
						icon: '../../static/image/icon/personal/8.png',
						title: '积分奖励'
					},
					{
						icon: '../../static/image/icon/personal/9.png',
						title: '我的收藏',
						url:'./mycollect/mycollect'
					},
					{
						icon: '../../static/image/icon/personal/10.png',
						title: '我的客服'
					},
				]
			}
		},
		methods: {
			loginFn() {
				uni.navigateTo({
					url: '../login/login'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.reward{
		.reward-title{
			width:100%;
			height:60rpx;
			line-height: 60rpx;
			text-align: center;
			background-color: #fff;
			image{
				width:40rpx;
				height:40rpx;
				vertical-align: middle;
			}
		}
	}
	.list {
		width: 100%;
		height: 342rpx;
		position: relative;

		.list-card {
			display: flex;
			display: -webkit-flex;
			position: absolute;
			align-items: center;
			flex-wrap: wrap;
			flex-direction: row;
			width: 690rpx;
			top: -120rpx;
			left: 50%;
			margin-left:-345rpx;
			border-radius: 10px;
			padding:10rpx;
			box-sizing: border-box;
			overflow: hidden;

			.item {
				width: 142.5rpx;
				height: 82rpx;
				padding:8rpx 0;
				margin:10rpx 10rpx;
				box-sizing: border-box;
				text-align: center;
				.img{
					width:40rpx;
					height:40rpx;
					margin :0 auto;
					image{
						width:100%;
						height:100%;
					}
				}
			}
		}
	}

	.info-card {
		position: relative;
		width: 100%;
		height: 274rpx;
		padding: 15rpx 30rpx;
		box-sizing: border-box;
		font-size: 24rpx;
		background:url(../../static/image/personal/bg.png) no-repeat fixed top;
		background-size: 100% 342rpx;
		color:#fff;

		.touxiang {
			width: 98rpx;
			height: 98rpx;
			float: left;
			border: 1px solid #fff;
		}

		.status {
			float: left;
			margin: 10rpx;

			.jifen {
				margin-top: 3px;
			}
		}

		.login {
			position: absolute;
			width: 145rpx;
			height: 48rpx;
			line-height: 48rpx;
			text-align: center;
			right: 30rpx;
			top: 15rpx;
			color: #fff;
			font-size: 28rpx;
			border-radius: 10rpx;
			background-color: #f39d23;
		}
	}
</style>
