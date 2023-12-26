<template>
  <div id="app">
    <h1>Vue 3 + Tesseract.js</h1>
    <h2>Upload an image to extract text</h2>
    <hr />
    <h3>File Upload Component</h3>
    <hr />

    <FileUploadComponent @textExtracted="handleTextExtracted" />
    <div v-if="extractedText">
      <h2>Extracted Text:</h2>
      <p>{{ extractedText }}</p>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import FileUploadComponent from './components/FileUploadComponent.vue';

export default {
  components: {
    FileUploadComponent,
  },
  setup() {
    // Define reactive variables using ref
    const extractedText = ref('');

    // Handler for text extraction
    const handleTextExtracted = (text) => {
      extractedText.value = text;
    };

    // Return variables and functions in an object
    return {
      extractedText,
      handleTextExtracted,
    };
  },
};
</script>
