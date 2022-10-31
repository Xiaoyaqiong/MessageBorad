<template>
	<view class="content">
		<!-- 其他功能（未开发） -->
		<view class="wrap">
			<u-row gutter="16">
				<u-col span="4">
					<view class="demo-layout bg-purple">私信</view>
				</u-col>
				<u-col span="4">
					<view class="demo-layout bg-purple-light">附近</view>
				</u-col>
				<u-col span="4">
					<view class="demo-layout bg-purple-dark">好友</view>
				</u-col>
			</u-row>

		</view>
		<!-- 发表留言 -->
		<view class="LeaveMessage" v-for="(res, index) in commentList" :key="res.id">
			<view class="information">
				<u-avatar :src="res.url" mode="square" class="avatar"></u-avatar>

				<text class="UserName">{{res.name}}</text>
				<text class="Signature">个性签名</text>
				
			</view>
			<view class="editmessage">
				
				<textarea name="" id="" cols="30" rows="10" v-model="MessageContent"></textarea>
				
				<button @click="SendMessage()">发表留言</button>
				
			</view>
			
		</view>

		<!-- 个人留言记录展示 -->
		<view>
			<view class="allselfRecord" v-if="flag">
				<view class="comment" v-for="(res, index) in commentList" :key="res.id">
					<view class="selfRecord">
						<view class="messageRecord">
							<ul v-for="(item,index) in testlist">
								<li>
									<view class="top">
										<image :src="res.url" mode="aspectFill"></image>
										<view class="right" >
											<view class="UserName"><text>{{ res.name }}</text></view>
											<view class="like" :class="{ highlight: res.isLike }">
												<view class="num">{{ res.likeNum }}</view>
												<u-icon v-if="!res.isLike" name="thumb-up" :size="30" color="#9a9a9a"
													@click="getLike(index)"></u-icon>
												<u-icon v-if="res.isLike" name="thumb-up-fill" :size="30"
													@click="getLike(index)">
												</u-icon>
											</view>
										</view>
									</view>
								</li>
								<li class="everyMessageRecord">{{item.txt}}</li>
							</ul>
						</view>
					</view>
				</view>
			</view>

			<view class="showtips" v-else>
				<view>快来发表你的留言吧~</view>
			</view>

		</view>
	</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				commentList: [],//个人信息
				testlist: [],//留言记录+个人id
				MessageContent: '',//当前留言内容
				flag:false
			}
		},
		onLoad() {
			this.getComment();
		},
		methods: {
			// 点赞功能
			getLike(index) {
				this.commentList[index].isLike = !this.commentList[index].isLike;
				if (this.commentList[index].isLike == true) {
					this.commentList[index].likeNum++;
				} else {
					this.commentList[index].likeNum--;
				}
			},
			// 个人信息
			getComment() {
				this.commentList = [{
					id: 1,
					name: '叶轻眉',
					url: 'https://cdn.uviewui.com/uview/template/SmilingDog.jpg',
					likeNum: 33,
					isLike: false,
					
				}]

			},
			
			// 发布留言功能
			SendMessage() {
				this.flag=true
				var usercom = {
					txt: this.MessageContent,
					user: this.commentList[0].name
				}
				this.testlist.unshift(usercom)
				this.MessageContent = "";
				console.log(usercom)
			},
			
		}
	}
</script>

<style lang="scss">
	.content{
		background: -webkit-linear-gradient(top,#E5EED3,white);
		height: 100%;
		width: 100%;
		padding: 0rpx;
		margin: 0rpx;
		position: absolute;
	}
	// 其他功能（未开发）
	.wrap {
		padding: 24rpx;
		.u-row {
			margin: 40rpx 0;
		}
		
		.demo-layout {
			height: 80rpx;
			border-radius: 8rpx;
			text-align: center;
			line-height: 80rpx;
			background: #F1E6A7;
			color: gray;
		}
		
	}

	
	// 发表留言
	.LeaveMessage {
		.img{
			height: 200px;
			width: 200px;
		}
		padding-inline: 50rpx;
		.information{
			// width: 100%;

			.UserName {
				font-size: 18px;
				padding-left: 24rpx;
				font-weight: bold;
				bottom: 0rpx;
			}
		
			.Signature {
				padding-left: 24rpx;
				color: gray;
			}
			
			.hiddenInput{
				display: none;
			}
		}
		.editmessage {
			margin-top: 24rpx;
			height: 100%;
			width: 100%;
			border-radius: 20px;
			background: -webkit-linear-gradient(top,#F1E6A7,white);
			
		}
		
		.editmessage textarea {
			padding: 24rpx;
		}
		.editmessage button{
			background-color: #FBDA95;
			color: #9a9a9a;
			border: 0rpx;
		}
	}
	
	
	// 个人留言记录展示
	.allselfRecord {
		margin-top: 50rpx;

		.comment {
			.selfRecord {
				padding-inline: 50rpx;

				.top {
					display: flex;

					image {
						width: 64rpx;
						height: 64rpx;
						border-radius: 50%;
					}

					.right {
						width: 100%;
						padding-left: 20rpx;
						font-size: 30rpx;
						display: flex;
						justify-content: space-between;
						.UserName text{
							line-height:64rpx ;
						}
						.like {
							align-items: center;
							color: #9a9a9a;
							font-size: 26rpx;

							.num {
								margin-right: 4rpx;
								color: #9a9a9a;
							}
						}

						.highlight {
							color: #5677fc;

							.num {
								color: #5677fc;
							}
						}
					}


				}
			}

			.messageRecord ul {
				padding: 0;
			}

			.messageRecord ul li {

				list-style: none;

			}

			.messageRecord ul .everyMessageRecord {
				height: 300rpx;
				background: -webkit-linear-gradient(top,#CDE1A7,white);
				margin-top: 14rpx;
				margin-bottom: 22rpx;
				list-style: none;
				border-radius: 20px;
				padding: 18rpx;
			}
		}
	}
	.showtips{
		height: 300rpx;
		
		width: 100%;
		display: flex;
		color: #9a9a9a;
		view{
			margin: auto;
		}
	}
</style>
