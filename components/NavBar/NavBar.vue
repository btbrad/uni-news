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
		props: {
			navList: {
				type: Array,
				default: () => []
			}
		},
		data() {
			return {
				currentChannelId: ''
			};
		},
		methods: {
			handleClick(item) {
				this.currentChannelId = item.id
				this.$emit('change', item)
			}
		},
		mounted() {},
		watch: {
			navList: function(val) {
				this.currentChannelId = val[0]?.id
			}
		}
	}
</script>

<style lang="scss" scoped>
.container {
	position: fixed;
	top: var(--window-top);
	left: 0;
	background: #fff;
	z-index: 10;
	width: 100%;
	overflow-x: auto;
	.scroll-box {
		::-webkit-scrollbar {
			display: none;
		}
	}
	.nav-bar {
		// width: 1000rpx;
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