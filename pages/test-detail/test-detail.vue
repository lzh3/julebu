<template>
	<view class="test-detail">
		<view class="test-name">
			{{title}}
		</view>
		<view class="content">
			<view v-if="list.length==0">
				<no-data></no-data>
			</view>
			<view v-else class="item" v-for="(v,index) of list" :key="v.id">
				<view class="test-option-name">{{index+1}}. {{v.title}} ({{v.multiple==0?'单选':'多选'}})</view>
				<view class="test-options" v-if="v.multiple === 0">
					<radio-group @change="radioChange">
						<label class="uni-list-cell" v-for="(u, m) in v.answer" :key="u.code">
							<view class="radio">
								<radio :value="`${v.id}-${index}-${m}-${u.code}-${u.title}`" />
							</view>
							<view class="radio-content">{{u.title}}</view>
						</label>
					</radio-group>
				</view>
				<!-- 多选 -->
				<view v-else class="test-options">
						<checkbox-group @change="checkboxChange" class="checkbox-group">
						<label v-for="(u, m) in v.answer" :key="u.title">
								<checkbox class="checkout-content"  :value="`${v.id}-${index}-${m}-${u.code}-${u.title}`" color="#F36523" style="transform:scale(0.7)" />{{u.title}}
						</label>
					</checkbox-group>
				</view>
				<view class="right">
					
				</view>
			</view>
		</view>
		<button type="primary" @click="submit" class="btn">提交答案</button>
		<Modal ref="modal" :status="status" :title="modalTitle" :desc="msg" btn btnText="立即查看" :cb="cb" />

	</view>
</template>

<script>
	import Modal from "@/components/Modal";
	import NoData from '../common/nodata.vue'
	export default {
		components: {
			Modal,
			NoData
		},
		data() {
			return {
				bid:0,
				token: null,
				title: null,
				modalTitle: '提交成功',
				list: [],
				startTime: 0,
				answers: [],
				msg: `请在“我的培训”查看得分`,
				status: "success",
				type: 'h5',
				examinationIndexUrl: this.type == 'h5' ? '/examination/index' : 'https://amd.mcooks.cn/api/examination/index',
				examinationSubmitUrl: this.type == 'h5' ? '/examination/submit' : 'https://amd.mcooks.cn/api/examination/submit'
			};
		},
		onLoad(opt) {
			let token = uni.getStorageSync("token");
			console.log(opt)
			let bid=opt.id;
			this.bid=Number(opt.id);
			if (token) {
				this.token = token;
				let that = this;
				uni.request({
					url: that.examinationIndexUrl,
					method: "post",
					header: {
						authtoken: "token " + token
					},
					data: {
						bid // 培训列表的id
					},
					success({
						data: {
							data
						}
					}) {
						console.log(data)
						that.title = data.title;
						that.list = data.examination;
						that.startTime = Date.now();
					}
				});
			}
		},
		methods: {
			// 提交答案
			submit() {
				let that = this;
				console.log(this.answers)
				if (this.answers.length) {
					uni.request({
						url: that.examinationSubmitUrl,
						method: "post",
						header: {
							authtoken: "token " + this.token
						},
						data: {
							bid: that.bid, // 培训id
							confirm: 1, // 是否确认提交 1、提交之后不允许修改 0、提交之后仅保存草稿
							second: (Date.now() - this.startTime) / 1000, // 答题用的时间 单位(秒) 从获取考题接口成功开始计算
							answers: that.answers
						},
						success(res) {
							console.log("TCL: success -> res", res.data);
							if (res.data.code == 402) {
								that.status = "error";
								that.modalTitle = res.data.msg;
							} else {
								that.status = "success";
								that.msg = res.data.msg;
							}
							that.$refs.modal.open();
						}
					});
				} else {
					// 提示
				}
			},
			// 请在“我-我的培训”查看得分
			cb() {
				// 跳转到我的培训
			},
			radioChange(EventHander) {
				let arr = EventHander.detail.value.split("-");
				let id = arr[0];
				let curIndex = this.answers.findIndex(v => v.id == id);
				if (curIndex != -1) {
					this.answers[curIndex].answer = arr[3];
				} else {
					this.answers.push({
						id,
						answer: arr[3]
					});
				}
			},
			checkboxChange: function({detail:{value}}) {
				console.log("value--", value)
				if(!value.length)return;
				let id = Number(value[0].split("-")[0]);
				let curIndex = this.answers.findIndex(v => v.id == id);
				if (curIndex != -1) {
					this.answers[curIndex].answer = value.map(v => v.split("-")[3]);
				} else {
					this.answers.push({
						id,
						answer: value.map(v => v.split("-")[3])
					});
				}
        console.log("this.answers", this.answers)
			}
		}
	};
</script>

<style lang="scss" scoped>
	/deep/.evan-checkbox__inner {
		background-color: #fff !important;
		border-color: #d1d1d1 !important;
	}

	.checkout-disabled {
		/deep/.evan-checkbox__inner {
			background-color: #F36523 !important;
			border-color: #F36523 !important;
		}

		/deep/.uni-icons {
			border-radius: 50%;

			span {
				color: #fff;
			}
		}
	}

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
						font-size: 30rpx;
						font-weight: 500;
						color: rgba(51, 51, 51, 1);
						margin: 46rpx 0 28rpx 0;
						line-height: 1;
					}

					.uni-list-cell {
						display: flex;
						justify-content: flex-start;
						align-items: center;
						line-height: 1;

						.radio {
							margin-right: 10rpx;

							/deep/.uni-radio-input-checked {
								background-color: #f36523 !important;
								border-color: #f36523 !important;
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
		.checkbox-group{
			display: flex;
			flex-direction:column;
		}
</style>
