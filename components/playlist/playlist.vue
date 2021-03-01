<template>
	<view>
		<view class="playlist">
			<view class="playlist_item"
			 v-for="item in playlist" :key="item.id"
			 @click="navigator(item)">
				<image :src="item.picUrl" mode=""></image>
				<text>{{item.name}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: ['playlist'],
		data() {
			return {
				
			}
		},
		methods: {
			navigator(item) {
				this.$emit('playlistItemClick', item)
			}
		}
	}
</script>

<style lang="less">
	.playlist {
		width: 100%;
		padding: 0 15rpx;
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		.playlist_item {
			width: 49%;
			padding-bottom: 15rpx;
			image {
				width: 100%;
			}
		}
	}
</style>
