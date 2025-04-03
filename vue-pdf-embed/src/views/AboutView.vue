<script setup>
import PDFViewer from '@/components/PDFViewer.vue'
import { ref } from 'vue'

const pdfUrl = ref('./0892360224.pdf')

const buttonClick = () => {
  pdfUrl.value = './Book 1 Chinese (Student Version) 20230731.pdf'
}
</script>

<template>
  <button @click="buttonClick"></button>
  <div class="about">
    <div class="container">
      <PDFViewer :pdf-url="pdfUrl" :initialPage="110"> </PDFViewer>
    </div>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}

.container {
  width: 550px;
  height: 770px;
  overflow: auto;
}
</style>
