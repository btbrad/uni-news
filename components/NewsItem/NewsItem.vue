<template>
	<view class="news-item">
		<view class="news-pic">
			<image :src="news.picurl" mode="scaleToFill"></image>
		</view>
		<view class="news-content">
			<view class="news-title">{{ news.title }}</view>
			<view class="news-info">
				<text class="author">{{ news.author }}</text>
				<text class="time">{{ news.posttime }}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"NewsItem",
		props: {
			news: {
				type: Object,
				default: () => {}
			}
		},
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">
.news-item {
	width: 750rpx;
	height: 100px;
	display: flex;
	padding: 10px;
	justify-content: flex-start;
	align-items: center;
	.news-pic {
		width: 100px;
		height: 80px;
		margin-right: 10px;
		image {
			width: 100%;
			height: 100%;
		}
	}
	.news-content {
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		.news-title {
			font-weight: 700;
			font-size: 14px;
		}
	}
	.news-info {
		display: flex;
		justify-content: space-between;
	}
}
</style>