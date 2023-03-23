<template>
	<view class="container">
		<NavBar @change="handleNavChange" />
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {
			handleNavChange(item) {
				console.log('当前新闻分类', item)
			}
		}
	}
</script>

<style>
	
</style>
