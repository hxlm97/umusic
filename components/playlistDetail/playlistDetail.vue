<template>
	<view class="detail">
		<view class="info">
			<image :src="playlistDetil.coverImgUrl" mode=""></image>
			<view class="name">{{playlistDetil.name}}</view>
			<view class="desc">介绍：{{playlistDetil.description}}</view>
			
		</view>
		<view class="list">
			<view class="item" v-for="item in playlistDetil.tracks">
				<image :src="item.al.picUrl" mode=""></image>
				<view class="name">{{item.al.name}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: 0,
				playlistDetil: []
			}
		},
		methods: {
			async getDetail() {
				const res = await this.$myRequest({
					url: '/playlist/detail?id=' + this.id
				})
				this.playlistDetil = res.data.playlist
			}
		},
		onLoad(options) {
			this.id = options.id
			this.getDetail()
		}
	}
</script>

<style lang="less">
	.detail {
		padding: 0 20rpx;
		.info {
			image {
				width: 100%;
			}
			.name {
				font-size: 20px;
			}
			.desc {
				margin: 20px 0;
				color: #666;
			}
		}
		.list {
			display: flex;
			flex-wrap: wrap;
			justify-content: space-between;
			.item {
				width: 350rpx;
				image {
					width: 100%;
				}
				.name {
					text-align: center;
				}
			}
		}
	}
</style>
