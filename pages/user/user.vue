<template>
	<view class="user">
		<text class="tit">欢迎来到umusic</text>
		<view class="login-box">
			<view class="avatar">
				<image src="/static/img/default_avatar.jpg" mode=""></image>
			</view>
			<view class="uni-form-item uni-column">
				<input type="text" value="" 
				placeholder="请输入手机号码"
				v-model="phone"/>
			</view>
			<view class="uni-form-item uni-column verify">
				<input type="text" value="" 
				placeholder="请输入验证码"
				v-model="captcha"/>
				<button type="default" style="border: 0;">获取验证码</button>
			</view>
		</view>
		<view class="login">登录</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				phone: '',
				captcha: ''
			}
		},
		methods: {
		}
	}
</script>

<style lang="less">
	.user {
		padding: 10px;
		.tit {
			display: inline-block;
			width: 750rpx;
			font-size: 22px;
			text-align: center;
		}
		.login-box {
			margin-top: 10px;
			padding: 20px;
			// height: 200px;
			// background: pink;
			.avatar {
				text-align: center;
				image {
					width: 60px;
					height: 60px;
					margin-bottom: 10px;
				}
			}
			input {
				// background: lightblue;
				height: 50px;
				border-bottom: 1px solid #f9f9f9;
			}
			.verify {
				position: relative;
			}
			button {
				position: absolute;
				top: 0;
				right: 0;
				font-size: 16px;
				width: 120px;
				height: 50px;
				line-height: 50px;
				background: lightblue;
			}
			button:after {
				border: 0!important;
			}
		}
		.login {
			margin-top: 20px;
			text-align: center;
			color: #fff;
			height: 40px;
			line-height: 40px;
			background: lightblue;
			border-radius: 20px;
		}
	}
</style>
