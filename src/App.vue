<template>
  <div id="app">
    <SparrowTiptap :content="content" />
  </div>
</template>

<script setup lang="ts">
  import SparrowTiptap from './components/SparrowTiptap.vue'
  import { ref } from 'vue'
  const content = ref<string>("hello world");
</script>
<style scoped lang="sass">
</style>