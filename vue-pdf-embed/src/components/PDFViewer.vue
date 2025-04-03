<script setup>
import { computed, nextTick, onBeforeUnmount, ref, watch } from 'vue'
import VuePdfEmbed, { useVuePdfEmbed } from 'vue-pdf-embed'

// optional styles
import 'vue-pdf-embed/dist/styles/annotationLayer.css'
import 'vue-pdf-embed/dist/styles/textLayer.css'

const pageRefs = ref([])
const pageVisibility = ref({})
let pageIntersectionObserver

const { doc } = useVuePdfEmbed({
  source:
    'https://raw.githubusercontent.com/mozilla/pdf.js/ba2edeae/web/compressed.tracemonkey-pldi-09.pdf',
})

const pageNums = computed(() =>
  doc.value ? [...Array(doc.value.numPages + 1).keys()].slice(1) : [],
)

const resetPageIntersectionObserver = () => {
  pageIntersectionObserver?.disconnect()
  pageIntersectionObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        const index = pageRefs.value.indexOf(entry.target)
        const pageNum = pageNums.value[index]
        pageVisibility.value[pageNum] = true
      }
    })
  })
  pageRefs.value.forEach((element) => {
    pageIntersectionObserver.observe(element)
  })
}

watch(pageNums, (newPageNums) => {
  pageVisibility.value = { [newPageNums[0]]: true }
  nextTick(resetPageIntersectionObserver)
})

onBeforeUnmount(() => {
  pageIntersectionObserver?.disconnect()
})
</script>

<template>
  <div class="app-content">
    <div v-for="pageNum in pageNums" :key="pageNum" ref="pageRefs">
      <VuePdfEmbed v-if="pageVisibility[pageNum]" text-layer :source="doc" :page="pageNum" />
    </div>
  </div>
</template>

<style>
.app-content {
  padding: 24px 16px;
}

.app-content > * {
  margin: 0 auto;
  padding-bottom: 8px;
}

.vue-pdf-embed__page {
  box-shadow: 0 2px 8px 4px rgba(0, 0, 0, 0.1);
}
</style>
