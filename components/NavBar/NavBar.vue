<template>
	<view class="container">
		<scroll-view :scroll-x="true" >
			<view class="nav-bar">
				<text 
					class="nav-item"
					@click="handleClick(item)" v-for="(item, index) in navList" 
					:key="item.id"
					:class="{'active': item.id === currentChannelId}"
				>
					{{ item && item.classname }}
				</text>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		name:"NavBar",
		data() {
			return {
				navList: [],
				currentChannelId: ''
			};
		},
		methods: {
			handleClick(item) {
				this.currentChannelId = item.id
				this.$emit('change', item)
			}
		},
		mounted() {
			console.log('mounted')
			uni.request({
				url: 'https://ku.qingnian8.com/dataApi/news/navlist.php',
				success: (res) => {
					console.log(res)
					if (res.statusCode === 200) {
						this.navList = res.data
						this.currentChannelId = res.data[0].id
						console.log(this.navList)
					} else {
						uni.showToast({
							title: '出错了！'
						})
					}
				}
			})
		}
	}
</script>

<style lang="scss" scoped>
.container {
	.nav-bar {
		display: flex;
		flex-wrap: nowrap;
		align-items: center;
		height: 50px;
		width: 500px;
		.nav-item {
			margin: 0 20px;
			font-size: 18px;
			&.active {
				color: #f40;
			}
		}
	}
}
</style>