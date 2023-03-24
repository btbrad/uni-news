<template>
	<view class="container">
		<scroll-view class="scroll-box" :scroll-x="true" :scrollbar-visibility="'hidden'">
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
	.scroll-box {
		::-webkit-scrollbar {
			display: none;
		}
	}
	.nav-bar {
		height: 50px;
		line-height: 50px;
		white-space: nowrap;
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