<template>
	<view class="content">
		<view class="scroll-view" style="width: 100%;">
			<view v-for="message in messageList" class="message-item">
				<view class="icon-wrapper">
					<view class="icon-item">
						<view class="chatgpt" v-if="message.isGPT">
							<image src="../../static/openAI.png"></image>
						</view>
						<view class="user" v-else>
							<svg t="1676886528337" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2756" width="200" height="200"><path d="M515.541449 7.082899c-280.359429 0-508.458551 228.120391-508.458551 508.458551s228.120391 508.458551 508.458551 508.458551 508.458551-228.120391 508.458551-508.458551S795.900879 7.082899 515.541449 7.082899zM515.541449 981.864196c-257.132626 0-466.301477-209.190121-466.301477-466.322747 0-257.132626 209.168851-466.322747 466.301477-466.322747s466.301477 209.190121 466.301477 466.322747S772.674075 981.864196 515.541449 981.864196zM614.574414 524.177056 614.574414 524.177056c47.751075-31.96876 79.230625-86.398604 79.230625-148.187857 0-98.437405-79.804915-178.24232-178.24232-178.24232-98.437405 0-178.24232 79.804915-178.24232 178.24232 0 61.810523 31.479551 116.219097 79.251895 148.187857-100.266622 39.519598-171.244501 137.170014-171.244501 251.453545 0 0.23397 0 0.446669 0.02127 0.659369 0 0.04254-0.02127 0.10635-0.02127 0.14889 0 15.612155 12.65563 28.246516 28.267786 28.246516 15.590885 0 21.886796-12.63436 21.886796-28.246516 0-0.340319-0.08508-0.659369-0.10635-1.020958 0.10635-118.005774 102.159649-219.995264 220.207964-219.995264 118.112124 0 220.207964 102.095839 220.207964 220.207964 0 0.14889-1.467628 29.054774 21.971875 29.054774 15.505806 0 28.076356-12.57055 28.076356-28.055086 0-0.06381-0.02127-0.12762-0.02127-0.2127 0-0.25524 0.02127-0.510479 0.02127-0.786989C785.797645 661.34707 714.798496 563.696654 614.574414 524.177056zM515.541449 510.734437c-74.402343 0-134.723968-60.321625-134.723968-134.723968 0-74.423613 60.321625-134.723968 134.723968-134.723968 74.423613 0 134.723968 60.321625 134.723968 134.723968S589.943792 510.734437 515.541449 510.734437z" fill="#272636" p-id="2757"></path></svg>
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
		width: 60upx;
	}
	
	.icon-item{
		margin-top: 20%;
	}
		
	.icon-item .chatgpt{
		/* background-color: rgb(16, 163, 127); */
	}
	
	.icon-item .chatgpt image{
		width: 100%;
		height: 100%;
		color: #F1F1F1;
		background-color: rgb(16, 163, 127);
	}
	
	.icon-item .user svg{
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
