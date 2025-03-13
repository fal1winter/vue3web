<template>
  <div class="chat-app">
    <h1>Chat Application</h1>
    <div class="chat-history">
      <div v-for="(message, index) in messages" :key="index" class="message">
        <p class="input">Input: {{ message.input }}</p>
        <p class="output" v-html="message.output"></p>
      </div>
    </div>
    <div class="input-section">
      <input
        v-model="inputContent"
        placeholder="Enter your message"
        @keyup.enter="sendMessage"
      />
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputContent: '',
      messages: []
    };
  },
  methods: {
    async sendMessage() {
      if (!this.inputContent.trim()) {
        return; // 防止发送空消息
      }

      const userInput = this.inputContent;
      this.inputContent = ''; // 清空输入框

      this.messages.push({ input: userInput, output: '' });

      try {
        const response = await this.fetchAPI(userInput);
        await this.parseStreamResponse(response, this.messages.length - 1);
      } catch (error) {
        this.messages[this.messages.length - 1].output = 'Error: Unable to fetch response.';
        console.error(error);
      }
    },
    async fetchAPI(input) {
      const apiUrl = 'https://chat.zju.edu.cn/api/ai/v1/chat/completions';
      const apiKey = 'sk-kN6W7yfTPK5BP5ED2dB8AaEf26C648888e1bEa715dDb35C4'; // 替换为你的API密钥

      const requestBody = {
        model: "deepseek-r1-671b",
        messages: [
          { role: "system", content: "You are a helpful assistant." },
          { role: "user", content: input }
        ],
        stream: true
      };

      const response = await fetch(apiUrl, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${apiKey}`
        },
        body: JSON.stringify(requestBody)
      });

      if (!response.ok) {
        throw new Error('Network response was not ok');
      }

      return response.body;
    },
    async parseStreamResponse(responseBody, messageIndex) {
      const reader = responseBody.getReader();
      let content = '';

      try {
        while (true) {
          const { done, value } = await reader.read();
          if (done) {
            break;
          }

          const chunk = new TextDecoder().decode(value);
          const lines = chunk.split('\n').filter(line => line.trim() !== '');

          for (const line of lines) {
            if (line.startsWith('data: ')) {
              const data = line.substring(6).trim();
              if (data === '[DONE]') {
                break;
              }

              const chunkData = JSON.parse(data);
              if (chunkData.choices[0].delta.content) {
                content += chunkData.choices[0].delta.content;
                this.messages[messageIndex].output = content;
                await this.$nextTick(); // 确保视图更新
              }
            }
          }
        }
      } catch (error) {
        console.error('Error parsing stream response:', error);
        throw error;
      }

      return content;
    }
  }
};
</script>

<style scoped>
.chat-app {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.chat-history {
  height: 400px;
  overflow-y: auto;
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 20px;
}

.message {
  margin-bottom: 10px;
}

.input-section {
  display: flex;
  gap: 10px;
}

input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>