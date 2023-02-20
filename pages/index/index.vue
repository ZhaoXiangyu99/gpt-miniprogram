<template>
	<view class="content">
		<view class="scroll-view" style="width: 100%;">
			<view v-for="message in messageList" class="message-item" :key="message.text">
				<view class="icon-wrapper">
					<view class="icon-item">
						<view class="chatgpt" v-if="message.isGPT">
							<image src="../../static/openAI.png"></image>
						</view>
						<view class="user" v-else>
							<image src="../../static/user.png"></image>
						</view>
					</view>
				</view>
				<view class="message-content">
					{{message.text}}
				</view>
			</view>
		</view>
		<view class="input-container">
			<input type="text" v-model="inputText" class="input" placeholder="请输入问题..." />
			<button type="primary" @click="requestChatGPT" class="button">推断</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				inputText: '',
				testText: "Hello World",
				messageData: {
					isGPT:true, //标记是用户的输入还是gpt返回
					text: '',     //文本信息
				},
				messageList: [
				],
			}
		},
		onLoad() {
			this.messageList = [
				{
					isGPT:false, //标记是用户的输入还是gpt返回
					text: 'hello world',     //文本信息
				},
				{
					isGPT:true, //标记是用户的输入还是gpt返回
					text: '你好',     //文本信息
				}
			];
		},
		onPullDownRefresh(e) {
		//   this.loadHistoryMessage(false);
			setTimeout(function () {
				console.log('start pulldown');
			}, 1000);
			uni.startPullDownRefresh();
			this.messageList = [];
			setTimeout(function () {
				uni.stopPullDownRefresh();
			}, 1000);
		},
		methods: {
			requestChatGPT(){
				let message = {
					isGPT: false,
					text: this.inputText,
				}
				this.messageList.push(message)
				console.log(this.inputText)
				uni.request({
					url: "http://1.12.68.184:8090/gpt",
					method:'POST',
					header: {
						'content-type': "application/json"
					},
					data: {
						"text": this.inputText,
					},
					success: (res) => {
						// let ret = this.messageData
						// ret.isUser = false
						// ret.text = res.data
						var ret = {
							text: res.data,
							isGPT: true,
						}
						this.messageList.push(ret)
						console.log(res)
					},
					fail: (error) => {
						console.log(error)
					}
				})
				this.inputText = ''
			}
			
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		/* justify-content: center; */
		overflow-y: scroll;
	}
	
	.scroll-view{
		padding-left: 20rpx;
		padding-right: 20rpx;
		box-sizing: border-box;
		-webkit-overflow-scrolling: touch;
		padding-bottom: 40rpx;
		background-color: #F1F1F1;
		margin-bottom: 25%;
	}
	
	.icon-wrapper{
		height: 100%;
		width: 10%;
	}
	
	.icon-item{
		margin-top: 20%;
	}
		
	.icon-item .chatgpt{
		/* background-color: rgb(16, 163, 127); */
		width: 80rpx;
		height: 80rpx;
		flex-shrink: 0;
		flex-grow: 0;
	}
	
	.icon-item .chatgpt image{
		width: 100%;
		height: 100%;
		color: #F1F1F1;
		background-color: rgb(16, 163, 127);
	}
	
	.icon-item .user {
		width: 80rpx;
		height: 80rpx;
		flex-shrink: 0;
		flex-grow: 0;
	}
	.icon-item .user image{
		width: 100%;
		height: 100%;
	}
	
	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
	
	.icon{
		width: 80rpx;
		height: 80rpx;
		flex-shrink: 0;
		flex-grow: 0;
	}
	
	.messageList{
		height: 40upx;
		width: 100%;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		background-color: rgba(247,247,248,var(--tw-bg-opacity));
	}
	
	.message-item{
		width: 100%;
		display: flex;
		flex-direction: row;
		font-size: 40upx;
		overflow: hidden;
		justify-content: center;
		/* flex-direction: row-reverse; */
		border-top: #8f8f94 1upx solid;
		margin: 20rpx 0;
		padding-top: 3%;
	}
	
	.message-content{
		flex: 1;
/* 		overflow: hidden; */		
        display: flex;
		text-align: left;
		/* line-height: 120%; */
		overflow: hidden;
		margin-left: 3%;
	}
	
	.input-container{
		display: flex;
		backdrop-filter: blur(0.27rpx);
		width: 100%;
		position: fixed;
		bottom: 0;
		left: 0;
		flex-direction: row;
		background-color: #F1F1F1;
	}
	
	.input-container .input{
		height: 100%;
		font-size: 46upx;
		line-height: 120%;
		width: 80%;
	}
	
	.button{
		background-color: red;
	}
</style>
