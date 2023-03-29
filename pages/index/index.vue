<template>
	<view class="container">
		<NavBar :nav-list="navList" @change="handleNavChange" />
		<view class="news-list">
			<NewsItem v-for="(news, index) in newsList" :key="news.id" :news="news" />
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				currentCategory: '', // 当前分类
				navList: [], // 分类列表
				newsList: [], // 新闻列表
				currentPage: 1 // 当前页码
			}
		},
		async onLoad() {
			uni.showLoading({
				title: "加载中...",
				mask: true
			})
			await this.getNavList()
			await this.getNewsList()
			uni.hideLoading()
		},
		onReachBottom() {
			this.currentPage++
			this.getNewsList()
		},
		methods: {
			async handleNavChange(item) {
				console.log('当前新闻分类', item)
				this.currentCategory = item.id
				uni.showLoading({
					title: "加载中...",
					mask: true
				})
				this.newsList = []
				await this.getNewsList()
				uni.hideLoading()
			},
			getNavList() {
				return new Promise((resolve, reject) => {
					uni.request({
						url: 'https://ku.qingnian8.com/dataApi/news/navlist.php',
						success: (res) => {
							console.log(res)
							if (res.statusCode === 200) {
								this.navList = res.data
								this.currentCategory = res.data[0].id
								console.log(this.navList)
								resolve()
							} else {
								uni.showToast({
									title: '出错了！'
								})
							}
						}
					})
				})
			},
			getNewsList() {
				return new Promise((resolve, reject) => {
					uni.request({
						url: `https://ku.qingnian8.com/dataApi/news/newslist.php?cid=${this.currentCategory}&num=10&page=${this.currentPage}`,
						success: (res) => {
							console.log('新闻', res)
							if (res.statusCode === 200) {
								this.newsList = [...this.newsList, ...res.data]
								resolve()
							}
						}
					})
				})
			}
		}
	}
</script>

<style>
	
</style>
