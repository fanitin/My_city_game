<script setup lang="ts">
defineProps({
  score: {
    type: Number,
    default: 0
  }
})
</script>

<template>
  <div class="flex flex-col items-end">
    <div class="bg-white rounded-xl px-4 py-2 text-right">
      <div class="text-sm text-slate-500">Score</div>
      <div class="text-2xl font-semibold text-slate-800 text-center">{{ score }}</div>
    </div>
  </div>
</template>

<style scoped>
</style>
