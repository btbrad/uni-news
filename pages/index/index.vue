<template>
	<view class="container">
		<NavBar :nav-list="navList" @change="handleNavChange" />
		<view class="news-list">
			<NewsItem v-for="(news, index) in newsList" :key="news.id" :news="news" @click.native="showDetail(news.id)" />
		</view>
		<view v-if="noData" class="no-data">
			<image src="../../static/empty.png" mode="widthFix"></image>
			<text>暂无数据</text>
		</view>
		<view v-if="loading" class="state">
			<text>加载中...</text>
		</view>
		<view v-if="noMore" class="state">
			<text>没有更多了...</text>
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
				currentPage: 1, // 当前页码
				loading: false, // 下拉加载中
				noMore: false, // 没有更多了
				noData: false, // 没有数据
			}
		},
		async onLoad() {
			console.log('index.vue onload')
			uni.showLoading({
				title: "加载中...",
				mask: true
			})
			await this.getNavList()
			await this.getNewsList()
			uni.hideLoading()
		},
		async onReachBottom() {
			this.currentPage++
			this.loading = true
			await this.getNewsList()
			this.loading = false
		},
		methods: {
			async handleNavChange(item) {
				console.log('当前新闻分类', item)
				this.currentCategory = item.id
				this.currentPage = 1
				this.noMore = false
				this.noData = false
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
								if (this.currentPage !== 1 && !res.data?.length) {
									this.noMore = true
								}
								if (this.currentPage === 1 && !res.data?.length) {
									this.noData = true
								}
								this.newsList = [...this.newsList, ...res.data]
								resolve()
							}
						}
					})
				})
			},
			showDetail(id) {
				uni.navigateTo({
					url: `/pages/detail/detail?id=${id}&cid=${this.currentCategory}`
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
.container {
	.state {
		width: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		color: #ccc;
	}
	.news-list {
		margin-top: 50px;
	}
	.no-data {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-between;
		padding-top: 50px;
		image {
			width: 60%;
		}
		text {
			margin-top: 20px;
		}
	}
}	
</style>
