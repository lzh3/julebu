<template>
	<view class="designer-in">
		<view class="items" v-for="(item,index) in lists" :key="index">
			<image :src="item.cover" mode="aspectFill" />
			<view class="items-content">
				<view class="title">{{item.title}}</view>
				<image src="../../static/image/arrow-right.png" mode="" class="operate-btn" @click="detail(item)" />
			</view>
		</view>
	</view>
</template>

<script>
	import api from '../../static/api.js'
	export default {
		data() {
			return {
				list: [{
					title: 'AMD锐龙-瞬间响应你的火花迸线',
					pic: "../../static/image/temp_designerin_pic.png",
				}, {
					title: 'AMD锐龙-瞬间响应你的火花迸线',
					pic: "../../static/image/temp_designerin_pic.png",
				}, {
					title: 'AMD锐龙-瞬间响应你的火花迸线',
					pic: "../../static/image/temp_designerin_pic.png",
				}],
				type: 3,
				son_type: 1,
				page: 1,
				limit: 10,
				lists: []
			};
		},
		methods: {
			detail(item) {
				console.log("TCL: detail -> item", item)
				let url = '';
				if (item.info_type == 1) {
					url = item.image;
				} else if (item.info_type == 2) {
					url = item.video;
				} else if (item.info_type == 3) {
					url = item.outer_url;
				}
				uni.navigateTo({
					url: 'detail/detail?type=' + item.info_type + '&url=' + url,
				});
			},
			getPlates() {
				uni.request({
					url: api.getPlates,
					method: 'POST',
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					data: {
						type: this.type,
						son_type: this.son_type,
						page: this.page,
						limit: this.limit
					},
					success: (res) => {
						console.log(res)
						// console.log(res.data.data.token)
						if (res.data.code == 200) {
							if (this.page == 1) {
								this.lists = res.data.data;
							} else {
								if (res.data.data && res.data.data.length != 0) {
									this.lists = [...this.lists, ...res.data.data];
								} else {
									this.page -= 1;
								}
							}
						}
					},
					fail(e) {
						console.log(e)
					}
				})
			}
		},
		onShow() {
			this.page = 1;
			this.getPlates();
		},
		onLoad(options) {
			this.getPlates();
		},
		onReachBottom() {
			this.page += 1;
			this.getPlates();
		},
	}
</script>

<style scoped lang="scss">
	.designer-in {
		.items {
			margin-bottom: 30rpx;
			padding: 0 30rpx;
			display: flex;
			justify-content: flex-start;
			height: 170rpx;
			background: rgba(255, 255, 255, 1);

			>image {
				flex: 0 0 170rpx;
				height: 170rpx;
				border-radius: 4rpx;
			}

			.items-content {
				flex: 1;
				margin-left: 40rpx;
				display: flex;
				justify-content: space-between;
				align-items: center;

				.title {
					font-size: 34rpx;
					font-weight: bold;
					color: rgba(51, 51, 51, 1);
					line-height: 1.5;
				}

				.operate-btn {
					width: 30rpx;
					height: 54rpx;
					background: transparent;
					margin-left: 115rpx;
				}
			}
		}

	}
</style>