<template>
	<view class="container">
		<view class="info-card">
			<view class="info">
				<view class="touxiang">
					<!-- <image src="" mode=""></image> -->
				</view>
				<view class="status">
					<view class="name">
						name  ID:-
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
				<view class="item"></view>
				<view class="item"></view>
				<view class="item"></view>
				<view class="item"></view>
				<view class="item"></view>
				<view class="item"></view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			loginFn(){
				uni.navigateTo({
					url:'../login/login'
				})
			}
		}
	}
</script>

<style lang="scss">
	.list{
		width:100%;
		height:342rpx;
		position:relative;
		border:1px solid blue;
		.list-card{
			display: flex;
			display: -webkit-flex;
			position:absolute;
			align-content: flex-start;
			flex-wrap: wrap;
			flex-direction: row;
			width:690rpx;
			height:342rpx;
			top:-60rpx;
			left:30rpx;
			.item{
				width:152.5rpx;
				height:72rpx;
				margin:10rpx 10rpx;
				box-sizing: border-box;
				border:1px solid black;
			}
		}
	}
	.info-card{
		position: relative;
		width:100%;
		height:274rpx;
		padding:15rpx 30rpx;
		box-sizing: border-box;
		border:1px solid blue;
		font-size: 24rpx;
		.touxiang{
			width:98rpx;
			height: 98rpx;
			float:left;
			border:1px solid red;
		}
		.status{
			float:left;
			margin:10rpx;
			.jifen{
				margin-top:3px;
			}
		}
		.login{
			position:absolute;
			width:145rpx;
			height:48rpx;
			line-height: 48rpx;
			text-align: center;
			right:30rpx;
			top:15rpx;
			color:#fff;
			font-size: 28rpx;
			border-radius: 10rpx;
			background-color: #f39d23;
		}
	}
</style>
