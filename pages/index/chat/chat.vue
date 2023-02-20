<template>
  <view class="container">
    <view class="header">ChatGPT Demo</view>
    <view class="input-container">
      <input type="text" v-model="inputText" class="input" placeholder="请输入问题..." />
      <button type="primary" @click="infer" class="button">推断</button>
    </view>
    <view class="output-container">
      <view v-if="isLoading">正在推断，请稍候...</view>
      <view v-else-if="outputText">{{ outputText }}</view>
      <view v-else>请输入问题并点击“推断”按钮进行推断。</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      inputText: '',
      outputText: '',
      isLoading: false
    };
  },
  methods: {
    async infer() {
      this.isLoading = true;
      const response = await uni.request({
        url: "http://1.12.68.184:8090/gpt",
        method: 'POST',
        header: {
          'Content-Type': 'application/json'
        },
        data: {
          text: this.inputText
        }
      });
	  console.log(response)
      this.outputText = response.data;
      this.isLoading = false;
    },
	onPullDownRefresh(e) {
	//   this.loadHistoryMessage(false);
		setTimeout(function () {
			console.log('start pulldown');
		}, 1000);
		uni.startPullDownRefresh();
		setTimeout(function () {
			uni.stopPullDownRefresh();
		}, 1000);
	},
  }
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.header {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
}

.input-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
}

.input {
  width: 200px;
  height: 40px;
  margin-right: 10px;
}

.output-container {
  font-size: 16px;
  font-weight: bold;
  text-align: center;
}
</style>
