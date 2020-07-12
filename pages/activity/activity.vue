<template>
	<view>
		<view class="cu-card article no-card" v-for="item in list">
			<view class="cu-item shadow">
				<navigator :url="'../activity-detail/activity-detail?joined='+item.joined">
					<view class="content">
						<view class="pic">
							<image 
								v-if="item.start"
								class="on"
							    src="../../static/image/icon/personal/activity/start.png" mode=""></image>
							 <image
								 v-else
								 class="on"
								 src="../../static/image/icon/personal/activity/end.png" mode=""></image>
						</view>
						<view class="desc">
							<view class="text-content">主题:{{item.title}}</view>
							<view class="text-content">时间:{{item.time}}</view>
							<view class="text-content">
								活动名额: <text>{{item.num}}人</text>
								<text class="f-right">{{item.p_time}}</text>
							</view>
						</view>
					</view>
				</navigator>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list:[
					{
						title:'折磨生出苦难，苦难',
						time:' 9:00-10:00',
						num:30,
						p_time:' 2020-07',
						start:false,
						joined:true, // 已经参与
					},
					{
						title:'折磨生出苦难，苦难',
						time:' 9:00-10:00',
						num:30,
						p_time:' 2020-07',
						start:true,
						joined:true
					}
				]
			}
		},
		methods: {
			
		}
	}
</script>

<style lang="scss" scoped>
	.cu-card{
		margin-bottom:6rpx;
	}
	.cu-item{
		padding-top:30rpx;
		.pic{
			position: relative;
			width:150rpx;
			height:150rpx;
			background-color: lightblue;
			image{
				width:100%;
				height:100%;
			}
			.on{
				width:105rpx;
				height:75rpx;
				position: absolute;
				left:0;
				top:0;
			}
		}
		.content{
			.desc{
				padding:0;
				margin-left:10rpx;
				view.text-content{
					height:40rpx;
					line-height: 40rpx;
					.f-right{
						float:right;
					}
				}
			}
		}
	}
</style>
