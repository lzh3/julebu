<template>
	<view>
		<view class="cu-card article no-card" v-for="item in list">
			<view class="cu-item shadow">
				<navigator :url="'../../activity-detail/activity-detail?joined='+item.joined+'&id='+item.id">
					<view class="content">
						<view class="pic">
							<image 
								v-if="item.start==='in_start'"
								class="on"
							    :src="item.image" mode=""></image>
							 <image
								 v-else
								 class="on"
								 :src="item.image" mode=""></image>
						</view>
						<view class="desc">
							<view class="text-content">主题:{{item.title}}</view>
							<view class="text-content">时间:{{item.start_time}}-{{item.end_time}}</view>
							<view class="text-content">
								<!-- 活动名额: <text>{{item.num}}人</text> -->
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
						id:2,
						p_time:' 2020-07',
						start:false,
						joined:false, // 已经参与
					},
					{
						title:'折磨生出苦难，苦难',
						time:' 9:00-10:00',
						num:30,
						id:1,
						p_time:' 2020-07',
						start:true,
						joined:false
					}
				]
			}
		},
		onLoad(){
			let token = uni.getStorageSync('token')
			this.getList(1,token)
		},
		methods: {
			getList(page,token){
				let _this=this;
				uni.request({
					url:'sign-events',
					method:'POST',
					header:{
						'authtoken':'token '+token,
					},
					data:{
						page,
						limit:10
					},
					success(res){
						console.log(res.data)
						// _this.list=res.data.data;
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
