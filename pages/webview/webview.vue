<template>
	<view>
		<web-view :src="url"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				url: ''
			};
		},
		onLoad(data) {
			console.log("TCL: onLoad -> data", data)
			this.url = data.url
		}
	}
</script>

<style lang="scss">

</style>