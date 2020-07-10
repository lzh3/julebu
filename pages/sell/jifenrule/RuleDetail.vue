<template>
	<view>
		<view class="list">

		</view>
		<view class="box">
			<view class="title main-color">
				<view>型号</view>
				<view>级别积分</view>
				<view>加镑积分</view>
			</view>
			<t-table>
				<t-tr v-for="item in tableList" :key="item.id">
					<t-td>{{ item.id + 1 }}</t-td>
					<t-td>{{ item.name }}</t-td>
					<t-td>{{ item.age }}</t-td>
				</t-tr>
			</t-table>
		</view>
	</view>
</template>

<script>
	import tTable from '../../table/t-table.vue';
	import tTr from '../../table/t-tr.vue';
	import tTd from '../../table/t-td.vue';
	import tTh from '../../table/t-th.vue';
	export default {
		data() {
			return {
				tableList: [{
						id: 0,
						name: '张三',
						age: '19'
					},
					{
						id: 1,
						name: '李四',
						age: '21'
					},
					{
						id: 2,
						name: '王二',
						age: '29'
					},
					{
						id: 3,
						name: '码字',
						age: '20'
					}
				]
			}
		},
		components: {
			tTable,
			tTh,
			tTr,
			tTd
		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.title{
		display: flex;
		justify-content: space-around;
		text-align: center;
		padding:5px 0;
		border-bottom:1px solid #f36523;
		view{
			flex:1;
		}
	}
</style>
