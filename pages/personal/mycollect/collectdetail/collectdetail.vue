<template>
	<!-- <view class="article">
		<view class="title"></view>
		<view class="info">
			<text><image src="../../../../static/image/icon/personal/look.png" mode=""></image>{{article.look}}</text>
			<text><image src="../../../../static/image/icon/personal/dz.png" mode=""></image>{{article.dz}}</text>
			<text><image src="../../../../static/image/icon/personal/shoucang.png" mode=""></image>{{article.collect}}</text>
			<text><image src="../../../../static/image/icon/personal/time.png" mode=""></image>{{article.time}}</text>
		</view>
		<view class="content">{{article.content}}</view>
		<view class="pic"></view>
		<view class="comments"></view>
	</view> -->
	<view class="cu-card dynamic" :class="isCard?'no-card':''">
		<view class="cu-item shadow">
			<view class="text-content">
				折磨生出苦难，苦难又会加剧折磨，凡间这无穷的循环，将有我来终结！
			</view>
			<view class="grid flex-sub padding-lr" :class="isCard?'col-3 grid-square':'col-1'">
				<view class="bg-img" :class="isCard?'':'only-img'" style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg);"
				 v-for="(item,index) in isCard?9:1" :key="index">
				</view>
			</view>
			<view class="text-gray text-sm text-right padding">
				<text class="cuIcon-attentionfill margin-lr-xs"></text> 10
				<text class="cuIcon-appreciatefill margin-lr-xs"></text> 20
				<text class="cuIcon-messagefill margin-lr-xs"></text> 30
			</view>
	
			<view class="cu-list menu-avatar comment solids-top">
				<view class="cu-item">
					<view class="cu-avatar round" style="background-image:url(https://ossweb-img.qq.com/images/lol/img/champion/Morgana.png);"></view>
					<view class="content">
						<view class="text-grey">莫甘娜</view>
						<view class="text-gray text-content text-df">
							凯尔，你被自己的光芒变的盲目。
						</view>
						<view class="margin-top-sm flex justify-between">
							<view class="text-gray text-df">2018年12月4日</view>
							<view>
								<text class="cuIcon-appreciatefill text-red"></text>
								<text class="cuIcon-messagefill text-gray margin-left-sm"></text>
							</view>
						</view>
					</view>
				</view>
	
				<view class="cu-item">
					<view class="cu-avatar round" style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg);"></view>
					<view class="content">
						<view class="text-grey">凯尔</view>
						<view class="text-gray text-content text-df">
							妹妹，如果不是为了飞翔，我们要这翅膀有什么用?
						</view>
						
						<view class="margin-top-sm flex justify-between">
							<view class="text-gray text-df">2018年12月4日</view>
							<view>
								<text class="cuIcon-appreciate text-gray"></text>
								<text class="cuIcon-messagefill text-gray margin-left-sm"></text>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				article:{
					title:'AMD再出大招狙击英特尔?有三款Ryzen“XT强化版本',
					look:10,
					dz:20,
					collect:20,
					time:'刚刚',
					content:'目前，法国AMD零售商透露了一系列AMD处理器新品的消息，包括三代Ryzen的加强版本Ryzen 9 3900XT、Ryzen 7 3800XT、Ryzen 5 3600XT三款Zen 2架构处理器。这三款处理器中规格最强的是Ryzen 9 3900X',
				}
			}
		},
		onLoad(opt){
			console.log(opt)
			// 动态设置标题
			uni.setNavigationBarTitle({
			　　title:opt.title
			})
		},
		methods: {
			
		}
	}
</script>

<style lang="scss" scoped>
	.article{
		width:100%;
		height:100vh;
		padding:15rpx;
		.title{
			font-size: 28rpx;
		}
		.info{
			image{
				width:14px;
				height:14px;
			}
		}
	}
</style>
