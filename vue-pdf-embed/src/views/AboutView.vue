<script setup>
import PDFViewer from '@/components/PDFViewer.vue'
</script>

<template>
  <div class="about">
    <div class="container">
      <PDFViewer />
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
  width: 600px;
  height: 800px;
  overflow: auto;
}
</style>
