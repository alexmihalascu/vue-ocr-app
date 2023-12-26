<template>
    <div class="upload-card">
      <input type="file" @change="onFileChange" />
      <button v-if="imageSrc" @click="recognizeText">Extract Text</button>
      <img v-if="imageSrc" :src="imageSrc" alt="Uploaded Image" class="uploaded-image"/>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  import Tesseract from 'tesseract.js';
  
  export default {
    emits: ['textExtracted'],
    setup(_, { emit }) {
      const imageSrc = ref(null);
  
      const onFileChange = (e) => {
        const files = e.target.files;
        if (files.length > 0) {
          const fileReader = new FileReader();
          fileReader.readAsDataURL(files[0]);
          fileReader.onload = (event) => {
            imageSrc.value = event.target.result;
          };
        }
      };
  
      const recognizeText = () => {
        if (imageSrc.value) {
          Tesseract.recognize(
            imageSrc.value,
            'eng',
            { logger: m => console.log(m) }
          ).then(({ data: { text } }) => {
            emit('textExtracted', text);
          });
        }
      };
  
      return { imageSrc, onFileChange, recognizeText };
    }
  };
  </script>
  
  <style>
  .upload-card {
    padding: 20px;
    background-color: white;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
    margin: 20px;
  }
  
  input[type="file"] {
    margin: 15px 0;
  }
  
  button {
    padding: 10px 15px;
    border: none;
    border-radius: 5px;
    background-color: #4CAF50;
    color: white;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #45a049;
  }
  
  .uploaded-image {
    max-width: 100%;
    height: auto;
    border-radius: 5px;
    margin-top: 15px;
  }
  </style>
  