<template>
	<view class="test-detail">
		<view class="test-name">
			{{title}}
		</view>
		<view class="content">
			<view class="item" v-for="(v,index) of list" :key="index">
				<view class="test-option-name">
					{{index+1}}. {{v.title}}
					<image class="pd" src="../../../static/image/icon/personal/right.png"></image>
					<image class="pd" src="../../../static/image/error.png"></image>
				</view>
				<view class="test-options" v-if="v.type === '单选'">
					<radio-group @change="radioChange">
						<label class="uni-list-cell" v-for="(u, m) in v.options" :key="u.value">
							<view class="radio">
								<radio :value="`${index}-${m}-${u.value}-${u.content}`" />
							</view>
							<view class="radio-content">{{u.content}}</view>
						</label>
					</radio-group>
				</view>
				<!-- 多选 -->
				<view v-else class="test-options">
					<evan-checkbox-group v-model="v.checked" @change="checkboxChange">
						<evan-checkbox class="checkout-content" primary-color="#F36523" v-for="(u, m) in v.options" :key="u.value"
							:label="`${index}-${m}-${u.value}-${u.content}`">
							{{u.content}}
						</evan-checkbox>
					</evan-checkbox-group>
				</view>
			</view>
		</view>

	</view>
</template>

<script>
	import Modal from '@/components/Modal'

	export default {
		components: {
			Modal
		},
		data() {
			return {
				token: null,
				title: null,
				list: []
			}
		},
		onLoad(opts) {
			console.log("考试详情页: onLoad -> opt", opts)
			let token = uni.getStorageSync('token')
			if (token) {
				this.token = token
				let that = this
				uni.request({
					// url: 'https://amd.mcooks.cn/api/examination/index', //仅为示例，并非真实接口地址。
					url: '/examination/index', //仅为示例，并非真实接口地址。
					data: {
						"bid": opts.id // 培训列表的id
					},
					method: 'post',
					header: {
						'authtoken': 'token ' + this.token,
					},
					success: ({
						data
					}) => {
						console.log("TCL: onLoad -> data", data)
						that.title = data.data.title
						that.list = data.data.examination
					}
				});
			}
		},
		methods: {
			// 提交答案
			submit() {
				this.$refs.modal.open()
			},
			// 请在“我-我的培训”查看得分
			cb() {
				// 跳转到我的培训
			},
			radioChange(EventHander) {
				let arr = EventHander.detail.value.split('-')
				this.list[arr[0]].checked = this.list[arr[0]].options[arr[1]]
				console.log("TCL: radioChange -> this.list", this.list)
			},
			// value "2-1-B-AMD Ryzen 7 3700X" 第三题第二个选项B内容是AMD Ryzen 7 3700X
			checkboxChange: function (value) {
				console.log("TCL: this.list", this.list)
				console.log("TCL: value", value)
			}
		},
	}
</script>

<style lang="scss" scoped>
	.test-detail {
		background: #fff;
		min-height: calc(100vh - 88rpx);
		padding-bottom: 84rpx;

		.test-name {
			font-size: 32rpx;
			font-weight: bold;
			color: rgba(51, 51, 51, 1);
			text-align: center;
			padding-top: 50rpx;
			line-height: 1;
		}

		.content {
			padding: 0 28rpx;

			.item {

				.checkout-content,
				.uni-list-cell {
					margin-bottom: 22rpx;
				}

				.test-option-name {
					position: relative;
					font-size: 30rpx;
					font-weight: 500;
					color: rgba(51, 51, 51, 1);
					margin: 46rpx 0 28rpx 0;
					line-height: 1;

					.pd {
						width: 36rpx;
						height: 36rpx;
						vertical-align: middle;
					}
				}

				.uni-list-cell {
					display: flex;
					justify-content: flex-start;
					align-items: center;
					line-height: 1;

					.radio {
						margin-right: 10rpx;

						/deep/.uni-radio-input-checked {
							background-color: #F36523 !important;
							border-color: #F36523 !important;
						}

						radio {
							// width: 28rpx;
							// height: 28rpx;
							transform: scale(0.7);

						}
					}

					.radio-content {
						font-size: 30rpx;
						font-weight: 500;
						color: rgba(51, 51, 51, 1);
					}


				}
			}
		}

		button {
			width: 690rpx;
			height: 64rpx;
			line-height: 64rpx;
			background: rgba(243, 101, 35, 1);
			border-radius: 10rpx;
			position: fixed;
			bottom: 20rpx;
			transform: translateX(-50%);
			left: 50%;
		}
	}
</style>