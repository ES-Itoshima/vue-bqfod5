<template>
  <div>
    <input
      type="text"
      v-model="userInput"
      placeholder="質問を入力してください"
    />
    <button @click="submitQuestion">送信</button>
    <p v-if="response">{{ response }}</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      userInput: '',
      response: '',
    };
  },
  methods: {
    async submitQuestion() {
      const apiKey = 'AAAAAAAA';
      const apiUrl =
        'https://api.openai.com/v1/engines/davinci-codex/completions';

      const headers = {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${apiKey}`,
      };

      const prompt = `ChatGPTによる質問と回答:\n\n質問: ${this.userInput}\n回答:`;

      const data = {
        prompt: prompt,
        max_tokens: 50,
        n: 1,
        stop: null,
        temperature: 1,
      };

      try {
        const response = await axios.post(apiUrl, data, { headers: headers });
        this.response = response.data.choices[0].text.trim();
      } catch (error) {
        console.error('Error:', error);
        this.response = 'エラーが発生しました。もう一度お試しください。';
      }
    },
  },
};
</script>
