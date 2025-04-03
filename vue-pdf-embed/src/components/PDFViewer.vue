<template>
  <div class="app-content">
    <div v-for="pageNum in pageNums" :key="pageNum" ref="pageRefs">
      <vue-pdf-embed v-if="pageVisibility[pageNum]" :source="doc" :page="pageNum" />
    </div>
  </div>
</template>

<script setup>
import { computed, nextTick, onBeforeUnmount, ref, watch } from 'vue'
import VuePdfEmbed, { useVuePdfEmbed } from 'vue-pdf-embed'

// optional styles
import 'vue-pdf-embed/dist/styles/annotationLayer.css'
import 'vue-pdf-embed/dist/styles/textLayer.css'

const props = defineProps({
  pdfUrl: { type: String, required: true },
  initialPage: { type: Number, default: 1 },
})

const pdfSource = ref(props.pdfUrl)
const pageRefs = ref([])
const pageVisibility = ref({})
let pageIntersectionObserver

const { doc } = useVuePdfEmbed({ source: pdfSource })

const pageNums = computed(() =>
  doc.value ? [...Array(doc.value.numPages + 1).keys()].slice(1) : [],
)

watch(props.pdfUrl, () => {})

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

<style>
body {
  margin: 0;
  padding: 0;
  background-color: #ccc;
}

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
