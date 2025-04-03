<script>
import pdf from 'vue-pdf'

export default {
  components: {
    pdf,
  },
}
</script>

<template>
  <div class="about">
    <pdf src="./0892360224.pdf"></pdf>
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
</style>
