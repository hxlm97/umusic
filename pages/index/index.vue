<template>
	<view class="home">
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
			<swiper-item v-for="item in swiper" :key="item.id">
				<image :src="item.imageUrl" mode=""></image>
			</swiper-item>
		</swiper>
		<view class="recommend">
			<view class="tit">
				<text>热门推荐</text>
			</view>
			<playlist :playlist="playlist" @playlistItemClick="playlistDetail"></playlist>
		</view>
	</view>
</template>

<script>
	import playlist from '../../components/playlist/playlist.vue'
	export default {
		data() {
			return {
				swiper: [],
				playlist: []
			}
		},
		components: {"playlist": playlist},
		onLoad() {
			this.getSwiper()
			this.getHot()
		},
		methods: {
			// 获取轮播图的数据
			async getSwiper () {
				const res = await this.$myRequest({
					url: '/banner'
				})
				
				// console.log(res);
				this.swiper = res.data.banners
				
			},
			// 获取热门推荐的数据
			async getHot () {
				const res = await this.$myRequest({
					url: '/personalized'
				})
				this.playlist = res.data.result
				// console.log(this.playlist);
				// uni.request({
				// 	url: '/api/personalized',
				// 	success: (res) => {
						// console.log(res);
				// 	}
				// })
			},
			// 跳转到歌单详情页
			playlistDetail(id) {
				uni.navigateTo({
					url: '../../components/playlistDetail/playlistDetail?id=' + id
				});
				console.log('dj')
				// console.log(id)
			},
			
		}
	}
</script>

<style lang="less">
	.home {
		swiper {
			width: 750rpx;
			height: 380rpx;
			image {
				width: 100%;
				height: 100%;
			}
		}
	}
	.recommend {
		.tit {
			margin: 10px 0;
			padding: 10px 0 ;
			text-align: center;
			// color: #fff;
			border-bottom: 1px solid #ffafc9;
			text {
				font-size: 34rpx;
				letter-spacing: 20px;
			}
		}
		
	}
	
</style>
