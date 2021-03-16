<template>
	<view class="detail">
		<view class="info">
			<view class="info-bg" :style="{backgroundImage: 'url(' + (playlistDetil.coverImgUrl) + ')'}">
			</view>
			<view class="info-m">
				<view class="tit">
					<view class="tit-l">
						<image :src="playlistDetil.coverImgUrl"></image>
					</view>
					<view class="tit-r">
						<text class="name">{{playlistDetil.name}}</text>
						<view class="desc">介绍：{{playlistDetil.description==''?'暂无歌单简介':playlistDetil.description}}</view>
					</view>
					
				</view>
				<view class="btn">
					<view class="btn-m">
						<view class="share">
							<image src="/static/img/share.png" mode=""></image>
							<view>分享</view>
						</view>
						<view class="comment" @click="commentClick(playlistDetil)">
							<image src="/static/img/comment.png" mode=""></image>
							<view>评论{{playlistDetil.commentCount}}</view>
						</view>
						<view class="collect">
							<image src="/static/img/collect.png" mode=""></image>
							<view>收藏</view>
						</view>
						<!-- <image src="../../static/img/comment.png" mode=""></image>
						<image src="../../static/img/collect.png" mode=""></image> -->
					</view>
					<!-- <view class="text">
						<text>分享</text>
						<text>评论{{playlistDetil.commentCount}}</text>
						<text>收藏</text>
					</view> -->
				</view>
			</view>
		</view>
		
		<view class="list">
			<view class="item" 
			v-for="(item, index) in playlistDetil.tracks" :key="item.id"
			:class="active===index?'active':''"
			@click="songClick(item,index)">
				{{item.index}}
				<view class="songname">{{item.name}}</view>
				<view class="songinfo">
					<text class="songers">
						<text v-for="(items, index) in item.ar" :key="items.id">{{items.name}}{{index === item.ar.length-1 ? "":"、" }}</text>
					</text>
					<text class="album">-{{item.al.name}}</text>
				</view>
			</view>
			<view class="ply">
				<!-- <audio :src="item.url" :poster="" :name="" :author="" :action="" controls></audio> -->
			</view>
			
		</view>

</view>
</template>

<script>
	export default {
		data() {
			return {
				id: 0,
				playlistDetil: [],
				song: [],
				active: 0
			}
		},
		methods: {
			// 获取歌单详情
			async getDetail() {
				const res = await this.$myRequest({
					url: '/playlist/detail?id=' + this.id
				})
				console.log(res)
				this.playlistDetil = res.data.playlist
				// this.song = res.data.playlist.tracks
				
			},
			
			commentClick(playlistDetil) {
				uni.navigateTo({
					url:'../comment/comment?id=' + this.id
				})
				console.log(playlistDetil) 
			},
			songClick(item,index) {
				this.active = index
				// async getSongUrl(id) {
				// 	const res = await this.$myRequest({
				// 	url: '/song/url?id=' + id
				// 	})
				// }
			},
			
		},
		onLoad(options) {
			this.id = options.id
			this.getDetail()
		}
	}
</script>

<style lang="less">
	.detail {
		.info {
			height: 240px;
			color: #fff;
			.info-bg {
				height: 240px;
				background-repeat: no-repeat;
				background-position: left;
				filter: blur(3px);
			}
			.info-m {
				position: absolute;
				left: 30px;
				top: 30px;
				.tit {
					display: flex;
					.tit-l {
						margin-right: 20px;
						image {
							width: 240rpx;
							height: 240rpx;
						}
					}
					.tit-r {
							width: 200px;
						.name {
							font-size: 18px;
						}
						.desc {
							margin: 20px 0;
							font-size: 14px;
							color: #f8f8f8;
							overflow: hidden;
							text-overflow: ellipsis;
							white-space: nowrap;
						}
					}
				}
				.name {
					font-size: 20px;
				}
				.btn {
					margin: 10px 0;
					.btn-m, .text {
						display: flex;
						justify-content: space-around;
						text-align: center;
					}
					image {
						width: 60rpx;
						height: 60rpx;
					}
				}
			}
		}
		.list {
			padding: 20rpx;
			background: #fdfdfd;
			.active {
				color: pink;
				.songinfo {
				color: pink!important;
				}
			}
			.item {
				height: 70px;
				line-height: 28px;
				.songinfo {
					display: flex;
					color: #666;
					.songers {
						height: 35px;
						overflow: hidden;
						text-overflow: ellipsis;
						white-space: nowrap;
					}
					.album {
						height: 35px;
						overflow: hidden;
						text-overflow: ellipsis;
						white-space: nowrap;
					}
				}
			}
			
		}
	}
</style>
