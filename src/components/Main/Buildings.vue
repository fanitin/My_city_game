<script setup>
import { Home, TreePine, Waves, Building2 } from 'lucide-vue-next'

const list = [
  { name: 'Farm', color: 'bg-amber-400', icon: Home },
  { name: 'Forest', color: 'bg-green-500', icon: TreePine },
  { name: 'Lake', color: 'bg-blue-400', icon: Waves },
  { name: 'Square', color: 'bg-purple-400', icon: Building2 },
]
</script>

<template>
  <div class="bg-white rounded-xl py-4 px-12 shadow-sm flex flex-col gap-3">
    <h3 class="font-semibold">Buildings</h3>
    <div class="flex flex-col gap-2">
      <button
        v-for="(b, i) in list"
        :key="i"
        class="flex items-center gap-3 p-2 rounded hover:bg-slate-50 transition"
      >
        <div :class="['w-8 h-8 flex items-center justify-center rounded', b.color]">
          <component :is="b.icon" class="w-4 h-4 text-white" />
        </div>
        <div class="text-sm">{{ b.name }}</div>
      </button>
    </div>
  </div>
</template>
