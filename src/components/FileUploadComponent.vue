<template>
    <div>
      <input type="file" @change="onFileChange" />
      <button v-if="imageSrc" @click="recognizeText">Extract Text</button>
      <img v-if="imageSrc" :src="imageSrc" alt="Uploaded Image" style="max-width: 100%; height: auto;"/>
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
  /* Add your component styling here if necessary */
  </style>
  