<template>
	<view class="cu-card dynamic no-card">
		<view class="cu-item shadow">
			<view class="text-content text-lg" id='main-title'>
				标题：折磨生出苦难，苦难又会加剧折磨，凡间这无穷的循环，将有我来终结！12
			</view>
			<view class="grid flex-sub padding-lr col-1">
				<view class="bg-img only-img" style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg);"
				 v-for="(item,index) in 1" :key="index">
				</view>
			</view>
			<view class="text-gray text-sm text-left padding">
				<text class="f-right">
					<text class="cuIcon-attention margin-lr-xs icon"></text> {{article.look}}
					<text class="cuIcon-appreciate margin-lr-xs icon"></text> {{article.dz}}
					<text class="cuIcon-comment margin-lr-xs icon"></text> {{article.collect}}
				</text>
				<text class="cuIcon-time margin-lr-xs"></text> {{article.time}}
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
		methods: {
			
		}
	}
</script>

<style lang="scss" scoped>
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
		padding-bottom:15rpx;
		margin-bottom:0;
		font-size: 14px;
		line-height: 48rpx;
   }
   .f-right{
		float:right;
   }
</style>
