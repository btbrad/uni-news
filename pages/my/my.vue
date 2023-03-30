<template>
	<view class="container">
		<view class="title">
			<image src="../../static/history.png" mode="widthFix"></image>
			<text>浏览历史记录</text>
		</view>
		<view class="history-list">
			<NewsItem v-for="news in history" :news="news" :key="news.id" @click.native="showDetail(news.id, news.classid)"></NewsItem>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				history: []
			};
		},
		onLoad() {
			console.log('onLoad')
			this.history = uni.getStorageSync('news-history')
		},
		onShow() {
			console.log('onShow')
			this.history = uni.getStorageSync('news-history')
		},
		methods: {
			showDetail(id, classid) {
				uni.navigateTo({
					url: `/pages/detail/detail?id=${id}&cid=${classid}`
				})
			}
		}
	}
</script>

<style lang="scss">
.container {
	padding-top: 20px;
	.title {
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		image {
			width: 50%;
			margin-bottom: 10px;
		}
		text{
			font-size: 20px;
		}
	}
}
</style>
