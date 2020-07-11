<template>
	<view class="container">
		<news-list :dataList='dataList'></news-list>
	</view>
</template>

<script>
	import newsList from '../../common/newsList.vue'
	export default {
		data() {
			return {
				dataList:[
					{
						pic:'',
						title:'AMD出大招狙击英特尔',
						url:'./collectdetail/collectdetail'
					},
					{
						pic:'',
						title:'美国AMD半导体公司专门为计算机、通信和消费电子行业设计和制造各种创新的微处理器.',
						url:'./collectdetail/collectdetail'
					},
					{
						pic:'',
						title:'AMD出大招狙击英特尔',
						url:'./collectdetail/collectdetail'
					}
				]
			}
		},
		components:{
			newsList
		},
		methods: {
			
		}
	}
</script>

<style lang="scss" scoped>
.item{
	position: relative;
	width:100%;
	height:210rpx;
	margin-bottom:10rpx;
	padding:30rpx;
	box-sizing: border-box;
	.pic{
		float:left;
		width:250rpx;
		height: 150rpx;
		background-color: skyblue;
		image{
			width:100%;
			height:100%;
		}
	}
	.title{
		float:left;
		width:414rpx;
		margin-left:15rpx;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 3;
		-webkit-box-orient: vertical;
	}
	.del{
		position:absolute;
		right:30rpx;
		bottom:30rpx;
		width:16px;
		height:16px;
		image{
			width:100%;
			height:100%;
		}
	}
	
}
</style>
