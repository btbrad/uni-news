<template>
	<view class="container">
			<view class="title">
					{{ newDetail.title }}
			</view>
			<view class="info">
				<text class="author">{{ newDetail.author }}</text>
				<text class="time">{{ formatTime(newDetail.posttime) }}</text>
			</view>
			<view class="content">
				<rich-text :nodes="newDetail.content"></rich-text>	
			</view>
	</view>
</template>

<script>
	import dayjs from 'dayjs'
	
	export default {
		data() {
			return {
				categoryId: '',
				newsId: '',
				newDetail: {}
			};
		},
		async onLoad(e) {
			console.log('详情页', e)
			this.newsId = e.id
			this.categoryId = e.cid
			uni.showLoading({
				title: '加载中...',
				mask: true
			})
			await this.getDetail()
			uni.hideLoading()
			this.saveToStorage()
		},
		methods: {
			getDetail() {
				return new Promise((resolve, reject) => {
					uni.request({
						url: `https://ku.qingnian8.com/dataApi/news/detail.php?cid=${this.categoryId}&id=${this.newsId}`,
						success: (res) => {
							console.log('新闻详情', res)
							res.data.content = res.data.content.replace(/<img/ig, '<img style="width: 100%;"')
							this.newDetail = res.data
							uni.setNavigationBarTitle({
								title: res.data.title
							})
							resolve()
						},
						fail: (err) => {
							reject(err)
						}
					})
				})
			},
			formatTime(time) {
				return dayjs(time).format('YYYY-MM-DD HH:mm:ss')
			},
			saveToStorage() {
				const { id, author, title, picurl, classid } = this.newDetail
				const record = { id, author, title, picurl, classid, visitTime: dayjs().format('YYYY-MM-DD HH:mm:ss') }
				const records = uni.getStorageSync('news-history') || []
				if (records?.length >= 10) {
					records.pop()
				}
				const idx = records.findIndex(item => item.id === record.id)
				if (idx >= 0 ) {
					records.splice(idx, 1)
				}
				records.unshift(record)
				uni.setStorageSync('news-history', records)
			}
		}
	}
</script>

<style lang="scss">
.container {
	padding: 0 10px;
	.title {
		font-size: 20px;
		font-weight: 500;
		padding: 10px 10px;
	}
	.info {
		display: flex;
		padding: 0 10px;
		justify-content: space-between;
		.author {
			font-style: italic;
		}
		.time {
			color: #ccc;
		}
	}
	.content {
		padding: 20px 10px;
	}
}
</style>
