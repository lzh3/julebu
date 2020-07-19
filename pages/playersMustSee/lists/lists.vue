<template>
	<view class="internet-bar">
		<view class="items" v-for="(item,index) in lists" :key="index">
			<image :src="item.cover" mode="aspectFill" />
			<view class="items-content">
				<view class="title">{{item.title}}</view>
				<view class="operate-btn" @click="detail(item)">
					<text>查看详情</text>
					<image src="../../../static/image/internetBar/double-arrow-right.png" mode="" />
				</view>
			</view>
		</view>
	</view>
</template>

<script>
  import api from '../../../static/api.js'
	export default {
		data() {
			return {
        type: 1,
        son_type: 1,
        page: 1,
        limit: 10,
        lists: []
			}
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
				  url: '../detail/detail?type=' + item.info_type + '&url=' + url,
				});
			},
      getPlates () {
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
        	fail(e){
        		console.log(e)
        	}
        })
      }
		},
    onShow() {
      this.page = 1;
      this.getPlates();
    },
    onLoad(options){
      this.son_type = options.type;
      this.getPlates();
    },
    onReachBottom () {
      this.page += 1;
      this.getPlates();
    },
	}
</script>

<style scoped lang="scss">
	.internet-bar {
		.items {
			height: 233rpx;
			background: rgba(255, 255, 255, 1);
			margin-bottom: 10rpx;
			padding: 30rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;

			>image {
				// width: 100%;
				width: 130rpx;
				min-width: 130rpx;
				height: 130rpx;
				// background: rgba(243, 101, 35, 1);
			}

			.items-content {
        flex: 1;
				margin-left: 20rpx;

				.title {
					font-size: 26rpx;
					font-weight: bold;
					color: rgba(51, 51, 51, 1);
					line-height: 1.5;
				}

				.operate-btn {
					display: flex;
					align-items: center;
					justify-content: flex-end;
					margin-top: 22rpx;

					text {
						font-size: 22rpx;
						font-weight: bold;
						color: rgba(243, 101, 35, 1);
					}

					image {
						width: 18rpx;
						height: 22rpx;
						background: transparent;
						margin-left: 10rpx;
					}
				}
			}
		}

	}
</style>
