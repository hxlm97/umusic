<template>
	<view class="cate">
		<scroll-view scroll-y="true" class="left">
			<view class="cate_item" 
			:class="active===index?'active':''"
			v-for="(item,index) in cate" :key="item.name"
			@click="cateClick(item,index)">
				{{item.name}}
			</view>
		</scroll-view>
		
		<scroll-view scroll-y="true" class="right">
			<view class="item" v-for="item in catePlaylist">
				<image :src="item.al.picUrl"></image>
				<text>{{item.al.name}}</text>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cate: [],
				catePlaylist: [],
				active: 0
			}
		},
		onLoad() {
			this.getCate()
		},
		methods: {
			// 获取分类数据
			async getCate() {
				const res = await this.$myRequest({
					url: '/toplist'
				})
				this.cate = res.data.list.slice(0, 13)
				this.cateClick(this.cate[0], 0)
			}, 
			// 获取右侧的数据
			async cateClick(item,index) {
				this.active = index
				const res = await this.$myRequest({
					url: '/playlist/detail?id=' + item.id
				})
				
				this.catePlaylist = res.data.playlist.tracks
			}
		}
	}
</script>

<style lang="less">
	.cate {
		display: flex;
		.left {
			width: 280rpx;
			height: 600px;
			text-align: center;
			background: #f9f9f9;
			.cate_item {
				height: 60px;
				line-height: 60px;
				// border: 1px solid #007AFF;
			}
			.active {
				background: pink;
			}
		}
		.right {
			width: 470rpx;
			height: 600px;
			.item {
				margin-bottom: 10px;
				text-align: center;
			}
		}
	}
</style>
