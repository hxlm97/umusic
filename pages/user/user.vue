<template>
	<view>
		<view class="user">
			<image src="../../static/img/user_bg.jpg" mode=""></image>
			<view @click="phone()">联系电话：400-000-0000（点击拨打）</view>
			<view>通信地址：西安市新城区南大街1号</view>
		</view>
		<map class="map" :latitude="latitude" :longitude="longitude" :scale="scale" :markers="markers"></map>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				longitude: 108.95346,
				latitude: 34.265725,
				scale: 16,
				markers: [
					{
						longitude: 108.95346,
						latitude: 34.265725,
						iconPath: '../../static/img/logo.png',
						width: 30,
						height: 30
						
					}
				]
			}
		},
		methods: {
			phone() {
				uni.makePhoneCall({
					phoneNumber: '400-000-0000'
				})
			}
		}
	}
</script>

<style lang="less">
	.user {
		image {
			width: 750rpx;
			height: 200px;
		}
		view {
			padding: 30rpx 10rpx 0;
		}
	}
	.map {
		width: 750rpx;
		height: 190px;
		margin: 30px 0;
	}
</style>
