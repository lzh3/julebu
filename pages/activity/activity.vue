<template>
	<view>
		
		<view class="activity-wrap" v-if='showLogin'>
			<NotLogin pageName="活动"></NotLogin>
		</view>
		<view class="cu-card article no-card"
				v-for="(item,key) in list" :key='key'>
			<view class="cu-item shadow">
				<navigator :url="'../activity-detail/activity-detail?id='+item.id">
					<view class="content">
						<view class="pic">
							<image class="img" :src="item.image" mode=""></image>
							<image class="on"
								v-if='item.status==="in_start"'
								src="../../static/image/activity/in_start.png" mode=""></image>
							<image class="on"
								v-else-if="item.status==='not_start'"
								src="../../static/image/activity/not_start.png" mode=""></image>
							<image class="on"
								v-else-if="item.status==='end'"
								src="../../static/image/activity/end.png" mode=""></image>
							
						</view>
						<view class="desc">
							<view class="text-content">主题:{{item.title}}</view>
							<view class="text-content">
								时间:<text class="main-color">
									{{item.start_time}}-{{item.end_time}}</text>
							</view>
							<view class="text-content ">
								活动名额:<text class="main-color"> {{item.quota_count}}人</text>
								<text class="f-right">已有{{item.partic_count}}人参与</text>
							</view>
						</view>
					</view>
				</navigator>
			</view>
		</view>
	</view>
</template>

<script>
	import NotLogin from "@/components/NotLogin";
	export default {
		components: {
			NotLogin
		},
		data() {
			return {
				showLogin: true,
				list: []
			}
		},
		onLoad() {
			console.log(123)
			let token = uni.getStorageSync('token')
			console.log(token)
			this.showLogin = token ? true: false;
			this.getActivityList(1,token);
		},
		methods: {
			// 获取活动列表
			getActivityList(page,token){
				let _this=this;
				uni.request({
					url:'https://amd.mcooks.cn/api/events/list',
					method:'POST',
					header:{
						'authtoken':'token '+token,
						'content-type': 'application/x-www-form-urlencoded'
					},
					data:{
						page,
						limit:10
					},
					success(res){
						console.log(res.data)
						_this.list=res.data.data;
					},
					fail(e){
						console.log(e)
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.cu-card {
		margin-bottom: 6rpx;
	}

	.cu-item {
		padding-top: 30rpx;

		.pic {
			position: relative;
			width: 150rpx;
			height: 150rpx;
			background-color: lightblue;

			.img {
				width: 100%;
				height: 100%;
			}

			.on {
				width: 105rpx;
				height: 75rpx;
				position: absolute;
				left: 0;
				top: 0;
			}
		}

		.content {
			.desc {
				padding: 0;
				margin-left: 10rpx;

				view.text-content {
					height: 40rpx;
					line-height: 40rpx;

					.f-right {
						float: right;
					}
				}
			}
		}
	}
</style>
