<template>
	<view>
		<view v-if="list.length==0">
			<no-data />
		</view>
		<view v-else class="cu-card article no-card" v-for="item in list">
			<view class="cu-item shadow">
				<navigator :url="'../../activity-detail/activity-detail?joined='+item.joined+'&id='+item.id">
					<view class="content">
						<view class="pic">
							<image 
								class="main-pic"
							    :src="item.image" 
								mode=""></image>
							 <!-- <image
								 v-else
								 class="on"
								 :src="item.image" mode=""></image> -->
								 <image class="on" v-if='item.status==="in_start"' src="../../../static/image/activity/in_start.png" mode="">
								 </image>
								 <image class="on" v-else-if="item.status==='not_start'" src="../../../static/image/activity/not_start.png"
								 	mode=""></image>
								 <image class="on" v-else-if="item.status==='end'" src="../../../static/image/activity/end.png" mode="">
						</view>
						<view class="desc">
							<view class="text-content">主题:{{item.title}}</view>
							<view class="text-content">时间:{{item.start_time}}-{{item.end_time}}</view>
							<view class="text-content"></view>
							<!-- <view class="text-content">
								活动名额: <text>{{item.num}}人</text> 
								<text class="f-right">{{item.start_time}}-{{item.end_time}}</text>
							</view> -->
						</view>
					</view>
				</navigator>
			</view>
		</view>
	</view>
</template>

<script>
	import Nodata from '../../common/nodata.vue'
	export default {
		data() {
			return {
				list:[
					/* {
						title:'折磨生出苦难，苦难',
						time:' 9:00-10:00',
						num:30,
						id:2,
						p_time:' 2020-07',
						start:false,
						joined:false, // 已经参与
					} */
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
					url:'https://amd.mcooks.cn/api/sign-events',
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
			.main-pic{
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
					.f-right{
						float:right;
					}
				}
			}
		}
	}
</style>
