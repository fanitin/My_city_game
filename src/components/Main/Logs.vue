<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps<{
  log: string[]
}>()
</script>

<template>
  <div class="bg-white rounded-xl p-3 shadow-sm min-w-2xl mx-auto">
    <div class="flex flex-col items-center text-center">
      <h4 class="text-sm font-medium mb-2">Logs</h4>
      <div class="text-xs text-slate-600 space-y-1 overflow-y-auto min-h-12 max-h-12 w-full">
        <div v-for="(entry, i) in log" :key="i">{{ entry }}</div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
