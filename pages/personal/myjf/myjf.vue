<template>
	<view>
		<view class="card">
			<view class="card-info">
				<view class="name">A先生/女士</view>
				<view class="jf">
					<view>2020Q1可用积分</view>
					<view>2020Q2可用积分</view>
				</view>
				<view class="jf">
					<view>200</view>
					<view>2000</view>
				</view>
			</view>
		</view>
		<view class="myjf">
			<view class="title">
				<image src="../../../static/image/personal/jf.png" mode=""></image>
				我的积分
			</view>
			<view class="sum">2020Q2合计: 3000</view>
		</view>
		<view class="record">
			<view class="item" v-for="item in jfList">
				<view>{{item.name}}</view>
				<view>{{item.a}}</view>
				<view>{{item.num}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				jfList:[
					{
						name:'日常签到',
						a:'签到积分',
						num:30
					},{
						name:'AMD CPU',
						a:'级别积分',
						num:30
					},{
						name:'AMD CPU',
						a:'签到积分',
						num:30
					},
				]
			}
		},
		methods: {
			
		}
	}
</script>

<style lang="scss" scoped>
	.record{
		.item{
			display: flex;
			height:80rpx;
			line-height: 80rpx;
			background-color: #fff;
			text-align: center;
			margin-bottom: 2px;
			font-size: 28rpx;
			view{
				width: 33.3333%;
				height:100%;
				&:last-child{
					color:#f36523;
				}
			}
		}
	}
	.myjf{
		height:40px;
		line-height: 40px;
		color:#f36523;
		padding:0 30rpx;
		font-size: 28rpx;
		.title{
			float: left;
			image{
				width:44rpx;
				height:40rpx;
				vertical-align: middle;
			}
		}
		.sum{
			float:right;
		}
	}
	.card{
		width:100%;
		height:280rpx;
		background: url(../../../static/image/personal/bg.png) no-repeat 10%/cover;
		.card-info{
			width:690rpx;
			height: 100%;
			margin:0 auto;
			color:white;
			padding:30rpx;
			font-size: 32rpx;
			background: url(../../../static/image/personal/card.png) no-repeat 100%/cover;
			.jf{
				display: flex;
				margin:28rpx 0;
				justify-content: space-between;
				view{
					width:230rpx;
					text-align: center;
				}
			}
		}
	}
</style>
