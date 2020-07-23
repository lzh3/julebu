<template>
	<view>
		<view class="card">
			<view class="card-bg-img">
				<image src="../../../static/image/personal/bg.png" mode=""></image>
			</view>
			<view class="card-info">
				<view class="info-bg-img">
					<image src="../../../static/image/personal/card.png" mode=""></image>
				</view>
				<view class="name">{{username}} 先生/女士</view>
				<view class="jf">
					<view>{{list[0].year}}Q{{list[0].q}}可用积分</view>
					<view>{{list[1].year}}Q{{list[1].q}}可用积分</view>
				</view>
				<view class="jf">
					<view>{{list[0].integral}}</view>
					<view>{{list[1].integral}}</view>
				</view>
			</view>
		</view>
		<view class="myjf">
			<view class="title">
				<image src="../../../static/image/personal/jf.png" mode=""></image>
				我的积分
			</view>
			<view class="sum">{{list[0].year}}Q{{list[0].q}}合计: {{showJfSum(jfList)}}</view>
		</view>
		<view class="record">
			<view class="item" v-for="item in jfList">
				<view>AMD CPU</view>
				<view>{{item.title}}</view>
				<view>{{item.integral}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: '*,',
				list: [{ // 当前季度
						"year": 2020,
						"q": 2,
						"integral": 550,
						"id": 2
					},
					{ // 上一季度
						"year": 2020,
						"q": 3,
						"integral": 550,
						"id": 1
					}
				],
				jfList: [{
						"title": "考试积分",
						"integral": 110
					},
					{
						"title": "考试积分",
						"integral": 110
					},
					{
						"title": "考试积分",
						"integral": 110
					},
					{
						"title": "考试积分",
						"integral": 110
					},
					{
						"title": "考试积分",
						"integral": 110
					}
				]
			}
		},
		async onLoad() {
			let token = uni.getStorageSync('token')
			let username = uni.getStorageSync('username')
			this.username = username;
			await this.getJfData(token)
			await this.getJfHistory(1, token)
		},
		methods: {
			getJfData(token) {
				let _this = this;
				uni.request({
					url: 'user/integral',
					method: 'get',
					header: {
						'authtoken': 'token ' + token,
					},
					success(res) {
						// console.log(res.data)
						// this.list=res.data.data;
					},
					fail(e) {
						console.log(e)
					}
				})
			},
			getJfHistory(page, token) {
				let _this = this;
				uni.request({
					url: 'https://amd.mcooks.cn/api/user/integral-log',
					method: 'post',
					header: {
						'authtoken': 'token ' + token,
					},
					data: {
						"year": _this.list[0].year, // 年
						"q": _this.list[0].q, // 季度
						page, // 页码
						"limit": 10 // 每页显示数量
					},
					success(res) {
						console.log(res.data)
						// this.list=res.data.data;
					},
					fail(e) {
						console.log(e)
					}
				})
			},
			showJfSum(arr){
				let sum = 0;
				arr.forEach((item)=>{
					sum+=item.integral;
				})
				return sum;
			}
		}
	}
</script>

<style lang="scss" scoped>
	.record {
		.item {
			display: flex;
			height: 80rpx;
			line-height: 80rpx;
			background-color: #fff;
			text-align: center;
			margin-bottom: 2px;
			font-size: 28rpx;

			view {
				width: 33.3333%;
				height: 100%;

				&:last-child {
					color: #f36523;
				}
			}
		}
	}

	.myjf {
		height: 40px;
		line-height: 40px;
		color: #f36523;
		padding: 0 30rpx;
		font-size: 28rpx;

		.title {
			float: left;

			image {
				width: 44rpx;
				height: 40rpx;
				vertical-align: middle;
			}
		}

		.sum {
			float: right;
		}
	}

	.card {
		position: relative;
		width: 100%;
		height: 280rpx;

		// background: url(../../../static/image/personal/bg.png) no-repeat 10%/cover;
		.card-bg-img {
			position: absolute;
			z-index: -1;
			width: 100%;
			height: 100%;
			top: 0;
			left: 0;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.card-info {
			position: relative;
			width: 690rpx;
			height: 100%;
			margin: 0 auto;
			color: white;
			padding: 30rpx;
			font-size: 32rpx;

			// background: url(../../../static/image/personal/card.png) no-repeat 100%/cover;
			.info-bg-img {
				position: absolute;
				z-index: -1;
				width: 100%;
				height: 100%;
				top: 0;
				left: 0;

				image {
					width: 100%;
					height: 100%;
				}
			}

			.jf {
				display: flex;
				margin: 28rpx 0;
				justify-content: space-around;

				view {
					width: 250rpx;
					text-align: center;
				}
			}
		}
	}
</style>
