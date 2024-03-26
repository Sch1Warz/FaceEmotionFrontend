<template>
  <div>
    <input v-model="message" placeholder="Enter a message" />
    <button @click="sendMessage">Send</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      message: "",
    };
  },
  methods: {
    async sendMessage() {
      try {
        const response = await axios.post("http://localhost:5000/api/message", {
          message: this.message,
        });
        console.log(response); // 确认响应内容
        alert(response.data.response); // 确保这一行被执行
      } catch (error) {
        console.error("Error sending message: ", error);
        alert("Failed to send message"); // 如果出错，给出提示
      }
    },
  },
};
</script>
