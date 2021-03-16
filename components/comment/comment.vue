<template>
	<view class="comment">
		<!-- <view class="hot-comment" v-if="flag">
			<text>精彩评论</text>
			<view class="list">
				<view class="item" v-for="item in hotComments" :key="item.userId">
					<view class="info">
						<view class="icon">
							<image :src="item.user.avatarUrl" mode=""></image>
						</view>
						<view class="desc">
							<view class="user">
								{{item.user.nickname}}
							</view>
							<view class="time">
								{{formatMsgTime(item.time)}}
							</view>
						</view>
					</view>
					<view class="content">
						{{item.content}}
					</view>
				</view>
			</view>
		</view> -->
		<view>全部评论 ({{comment.total}})</view>
		<view class="list comments">
			<view class="item" v-for="item in comments" :key="item.userId">
				<view class="info">
					<view class="icon">
						<image :src="require('../../static/img/user.png')" mode=""></image>
						<!-- <image :src="item.user.avatarUrl" mode=""></image> -->
					</view>
					<view class="desc">
						<view class="user">
							匿名用户
							<!-- {{item.user.nickname}} -->
						</view>
						<view class="time">
							{{formatMsgTime(item.time)}}
						</view>
					</view>
				</view>
				<view class="content">
					{{item.content}}
				</view>
			</view>
		</view>
		<view class="comment-area">
			<image src="../../static/img/default_avatar.jpg" mode=""></image>
			<input type="text"
			placeholder="把所有的喜怒哀乐都放进评论里" 
			v-model="content"/>
			<button type="default" @click="postComment">发表</button>
		</view>
	</view>
</template>

<script>
	// import {formatDate} from "../../util/utils.js"
	export default {
		data() {
			return {
				comment: {},
				comments: [],
				hotComments: [],
				content: '',
				c: "",
				flag: true,
				id: 0,
				pageIndex :1
			}
		},
		methods: {
			// 获取歌单评论
			async getComment() {
				const res = await this.$myRequest({
					url: '/comment/playlist?id=' + this.id + '&limit=' + this.pageIndex * 20 
				})
				console.log(res)
				this.comment = res.data
				// this.hotComments = res.data.hotComments
				this.comments = [...this.comments,...res.data.comments]
				// if(this.hotComments.length == 0) return this.flag = false
				
				// console.log(this.hotComments.length)
				console.log(res)
			},
			// 发表评论
			
			postComment() {
				if (this.content.trim().length === 0) {
					return ("评论的内容不能为空！")
				}
				uni.request({
					url: '/comment?t=1&type=2&id=' + this.id + '&content=' + this.content ,
					success: (res) => {
						var cmt = {
							// avatarUrl: 'static/img/user.png'
							nickname: '匿名用户',
							avatarUrl:  require('../../static/img/user.png'),
							time: Date.now(),
							content: this.content.trim()
						}
						// if(this.comments.user.avatarUrl==='') return avatarUrl
						// this.nickname.unshift(this.comments.user)
						// this.avatarUrl.unshift(this.comments.user)
						// this.time.push(this.comments)
						this.comments.unshift(cmt)
						this.content = ''
					}
				})
			},
			// 时间戳
			formatMsgTime (timespan) {
			  var dateTime = new Date(timespan) // 将传进来的字符串或者毫秒转为标准时间
			  var year = dateTime.getFullYear()
			  var month = dateTime.getMonth() + 1
			  var day = dateTime.getDate()
			  var hour = dateTime.getHours()
			  var minute = dateTime.getMinutes()
			  // var second = dateTime.getSeconds()
			  var millisecond = dateTime.getTime() // 将当前编辑的时间转换为毫秒
			  var now = new Date() // 获取本机当前的时间
			  var nowNew = now.getTime() // 将本机的时间转换为毫秒
			  var milliseconds = 0
			  var timeSpanStr
			  milliseconds = nowNew - millisecond
			  if (milliseconds <= 1000 * 60 * 1) { // 小于一分钟展示为刚刚
			    timeSpanStr = '刚刚'
			  } else if (1000 * 60 * 1 < milliseconds && milliseconds <= 1000 * 60 * 60) { // 大于一分钟小于一小时展示为分钟
			    timeSpanStr = Math.round((milliseconds / (1000 * 60))) + '分钟前'
			  } else if (1000 * 60 * 60 * 1 < milliseconds && milliseconds <= 1000 * 60 * 60 * 24) { // 大于一小时小于一天展示为小时
			    timeSpanStr = Math.round(milliseconds / (1000 * 60 * 60)) + '小时前'
			  } else if (1000 * 60 * 60 * 24 < milliseconds && milliseconds <= 1000 * 60 * 60 * 24 * 15) { // 大于一天小于十五天展示位天
			    timeSpanStr = Math.round(milliseconds / (1000 * 60 * 60 * 24)) + '天前'
			  } else if (milliseconds > 1000 * 60 * 60 * 24 * 15 && year === now.getFullYear()) {
			    timeSpanStr = month + '-' + day + ' ' + hour + ':' + minute
			  } else {
			    timeSpanStr = year + '-' + month + '-' + day + ' ' + hour + ':' + minute
			  }
			  return timeSpanStr
			}

		},
		onLoad(options) {
			this.id = options.id
			this.getComment()
			if(this.hotComments.length = 0) return this.flag = false
		},
		onReachBottom() {
			// if(this.hotComments.length > 0) return this.flag = true
			if(this.comment.comments.length < this.pageIndex*20) return
			this.pageIndex++
			this.getComment()
			console.log(this.comment.comments.length)
			console.log(this.hotComments.length)
		},
		onPullDownRefresh() {
			this.pageIndex = 1
			this.comment = []
			this.hotComments = []
			this.comments = []
			this.getComment()
		}
	}
</script>

<style lang="less">
	.comment {
		padding: 10px 10px 0;
		.hot-comment {
			margin-bottom: 20px;
		}
		.comments {
			margin-bottom: 50px;
		}
		.list {
			.item {
				padding-top: 10px;
				border-bottom: 1px solid #eee;
				.info {
					display: flex;
					// height: 60px;
					// line-height: 60px;
					.icon {
						margin-right: 10px;
						image {
							width: 50px;
							height: 50px;
						}
					} 
					.desc {
						// padding: 5px 0;
						height: 50px;
						// line-height: 20px;
						// box-sizing: border-box;
						.user {
							font-size: 14px;
						}
						.time {
							font-size: 12px;
							color: #666;
						}
					}
				}
				.content {
					margin: 0 0 10px 60px;
					padding: 5px 0;
				}
			}
		}
		.comment-area {
			display: flex;
			width: 750rpx;
			height: 50px;
			line-height: 50px;
			position: fixed;
			bottom: 0;
			background: #fff;
			image {
				margin-top: 10px;
				width: 30px;
				height: 30px;
			}
			input {
				margin: 10px;
				width: 500rpx;
				height: 30px;
				font-size: 14px;
				background: #f9f9f9;
				border-radius: 15px;
			}
			.uni-input-wrapper, 
			.uni-input-input, 
			.uni-input-form, 
			.uni-input-placeholder {
				// margin-left: 15px!important;
				padding-left: 5px;
			}
			button {
				margin: 10px 20px 10px 0;
				width: 60px;
				height: 30px;
				font-size: 12px;
				border-radius: 15px;
				
			}
			.uni-button:after {
				border: 0!important;
			}
		}
	}
</style>
