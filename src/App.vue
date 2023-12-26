<template>
  <div id="app" class="app-container">
    <h1>PHOTO TEXT EXTRACTOR</h1>
    <div class="card">
      <h2>Upload an image to extract text</h2>
      <FileUploadComponent @textExtracted="handleTextExtracted" />
      <div v-if="extractedText" class="extracted-text">
        <h2>Extracted Text:</h2>
        <p>{{ extractedText }}</p>
        <button @click="copyToClipboard">Copy Text</button>
      </div>
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

    // Copy text to clipboard
    const copyToClipboard = () => {
      const textToCopy = extractedText.value;
      const textarea = document.createElement('textarea');
      textarea.value = textToCopy;
      document.body.appendChild(textarea);
      textarea.select();
      document.execCommand('copy');
      document.body.removeChild(textarea);
      alert('Text copied to clipboard!');
    };

    // Return variables and functions in an object
    return {
      extractedText,
      handleTextExtracted,
      copyToClipboard,
    };
  },
};
</script>

<style>
.app-container {
  padding: 20px;
  text-align: center;
}

.card {
  background-color: white;
  margin: 20px auto;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  width: 100%;
}

.extracted-text {
  margin-top: 20px;
  background-color: #f9f9f9;
  padding: 15px;
  border-radius: 5px;
}

button {
  margin-top: 10px;
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
</style>