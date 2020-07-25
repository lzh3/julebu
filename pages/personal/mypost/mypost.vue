<template>
	<view class="container">
		<view class="list">
			<view class="item" v-for="item in dataList">
				<navigator :url="item.url">

					<view class="title">AMD yes最早起源于ryzen发布后老莱的视频，经过AMD吧和b站
						弹幕发扬光大能不能战未来？我觉得OK</view>
					<view class="pic-list">
						<view class="pic" v-for="pic in item.pic">
							{{pic}}
						</view>
					</view>
					<view>
						<view class="text-gray text-sm text-left padding info">
							<view>
								<text class="cuIcon-time margin-lr-xs"></text> {{item.time}}
							</view>
							<view>
								<text class="cuIcon-attention margin-lr-xs icon"></text> {{item.look}}
							</view>
							<view>
								<text class="cuIcon-comment margin-lr-xs icon"></text>{{item.comment}}
							</view>
							<view>
								<text class="cuIcon-appreciatefill text-red margin-lr-xs icon"></text>
								{{item.dz}}
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
				dataList: [{
						title: '',
						pic: [1, 2, 4, 3],
						time: 'gg',
						look: 22,
						comment: 20,
						dz: 20,
						url:'/pages/common/postdetail/postdetail'
					},
					{
						title: '',
						pic: [1, 2, 4, 3],
						time: 'gg',
						look: 22,
						comment: 20,
						dz: 20,
						url:'/pages/common/postdetail/postdetail'
					}
				]
			}
		},
		onLoad(opt){
			console.log(opt)
		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.list {
		.item {
			width: 100%;
			height: 333rpx;
			margin-bottom: 5px;
			padding: 30rpx;
			background-color: #fff;

			.info {
				display: flex;
				justify-content: space-between;
			}

			.pic-list {
				display: flex;
				margin-top: 10px;
				justify-content: space-between;

				.pic {
					width: 156rpx;
					height: 112rpx;
					background-color: lightblue;
				}
			}
		}
	}
</style>
