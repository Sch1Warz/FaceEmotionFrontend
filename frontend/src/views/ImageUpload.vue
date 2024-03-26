<template>
  <div class="container">
    <h1 class="text-center my-5">Image Prediction using Vue 3 and Flask</h1>
    <div class="image-upload">
      <input type="file" @change="previewImage" accept="image/*" />
      <button @click="uploadImage" class="btn-upload">Upload</button>
    </div>
    <div v-if="imageUrl" class="image-preview my-3">
      <img :src="imageUrl" alt="Image preview" />
    </div>
    <div v-if="predictionResult" class="prediction-result">
      <h5>Prediction Result: {{ predictionResult }}</h5>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      selectedFile: null,
      imageUrl: null,
      predictionResult: "",
    };
  },
  methods: {
    previewImage(event) {
      this.selectedFile = event.target.files[0];
      this.imageUrl = URL.createObjectURL(this.selectedFile);
    },
    async uploadImage() {
      if (!this.selectedFile) {
        alert("Please select an image first.");
        return;
      }

      const formData = new FormData();
      formData.append("file", this.selectedFile);

      try {
        const response = await axios.post("http://localhost:5000/upload_file", formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        });
        this.predictionResult = response.data.result;
      } catch (error) {
        console.error("Error uploading image:", error);
      }
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: auto;
  text-align: center;
}
.image-upload {
  margin-bottom: 20px;
}
.btn-upload {
  display: inline-block;
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  cursor: pointer;
  border: none;
  border-radius: 5px;
}
.btn-upload:hover {
  background-color: #0056b3;
}
.image-preview img {
  max-width: 100%;
  height: auto;
}
</style>
