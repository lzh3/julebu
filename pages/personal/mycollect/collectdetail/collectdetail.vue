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
	<view class="cu-card dynamic no-card">
		<view class="cu-item shadow">
			<view class="text-content text-lg" id='main-title'>
				标题：折磨生出苦难，苦难又会加剧折磨，凡间这无穷的循环，将有我来终结！
			</view>
			<view class="text-gray text-sm text-left padding">
				<text class="cuIcon-attention margin-lr-xs icon"></text> {{article.look}}
				<text class="cuIcon-appreciate margin-lr-xs icon"></text> {{article.dz}}
				<text class="cuIcon-like margin-lr-xs icon"></text> {{article.collect}}
				<text class="f-right">
					<text class="cuIcon-time margin-lr-xs"></text> {{article.time}}
				</text>
			</view>
			<view class="text-content">
				{{article.content}}
			</view>
			<view class="grid flex-sub padding-lr col-1">
				<view class="bg-img only-img" style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg);"
				 v-for="(item,index) in 1" :key="index">
				</view>
			</view>
			<!-- 评论 -->
			<view class="cu-bar bg-white solid-bottom margin-top">
				<view class="action">
					<text class="cuIcon-comment margin-lr-xs"></text> 网友热评
				</view>
			</view>
			<view class="cu-list menu-avatar comment solids-top">
				<view class="cu-item">
					<view class="cu-avatar round" style="background-image:url(https://ossweb-img.qq.com/images/lol/img/champion/Morgana.png);"></view>
					<view class="content">
						<view class="text-grey">莫甘娜</view>
						<view class="text-gray text-content text-df">
							凯尔，你被自己的光芒变的盲目。
							
							<view class="comment-dz">
								<text class="cuIcon-appreciate"></text>12
							</view>
						</view>
						<view class="margin-top-sm flex justify-between">
							<view class="text-gray text-df">
								<text>2018年12月4日</text>
								<text>·</text>
								<text>回复</text>
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
							
							<view class="comment-dz">
								<text class="cuIcon-appreciate text-gray"></text>22
							</view>
						</view>
						
						<view class="margin-top-sm flex justify-between">
							<view class="text-gray text-df">
								<text>2018年12月4日</text>
								<text>·</text>
								<text>回复</text>
								<!-- <text class="cuIcon-message text-gray margin-left-sm"></text> -->
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="comment-footer">
			<view class="to-comment">
				<view class="uni-form-item uni-column uni-inp">
					<view class="touxiang"></view>
					<input class="uni-input" placeholder="我也来说两句" />
				</view>
				<view>
					<image src="../../../../static/image/icon/news/dz.png" mode=""></image>
				</view>
				<view>
					<image src="../../../../static/image/icon/news/love.png" mode=""></image>
				</view>
				<view>
					<image src="../../../../static/image/icon/news/share.png" mode=""></image>
				</view>
			</view>
		</view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				comment:'',
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
	.comment-footer{
		width:100%;
		height:80rpx;
		.to-comment{
			position: fixed;
			bottom:0;
			left:0;
			width:100%;
			height:60rpx;
			padding:10rpx 0;
			background-color: #fff;
			box-sizing: content-box;
			view{
				float: left;
				&+view{
					width:50rpx;
					height: 45rpx;
					margin-left:70rpx;
					margin-top:7.5rpx;
					image{
						width:100%;
						height:100%;
					}
				}
			}
			.uni-inp{
				position:relative;
				margin-left:20rpx;
				.uni-input{
					width:335rpx;
					height:60rpx;
					text-align: center;
					font-size: 12px;
					background-color: #f2f2f2;
					border-radius: 30rpx;
				}
				.touxiang{
					position: absolute;
					left:2px;
					top:10rpx;
					width:40rpx;
					height: 40rpx;
					border-radius: 50%;
					background-color: red;
				}
			}
		}
	}
	.cu-item{
		.icon+.icon{
			margin-left:10px;
		}
	}
	.content{
		postion:relative;
		.comment-dz{
			position: absolute;
			right:0;
			top:0;
		}
	} 
	.action{
		color:#f36523;
	}
	#main-title{
		margin-top:15rpx;
		margin-bottom:0;
		font-size: 14px;
		line-height: 48rpx;
	}
	.f-right{
		float:right;
	}
</style>
