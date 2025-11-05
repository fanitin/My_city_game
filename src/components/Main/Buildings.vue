<script setup lang="ts">
import { Home, TreePine, Waves, Building2 } from 'lucide-vue-next'

const list = [
  { name: 'House (1 and 4)', color: 'bg-amber-400', icon: Home },
  { name: 'Forest (2 and 5)', color: 'bg-green-500', icon: TreePine },
  { name: 'Lake (3 and 6)', color: 'bg-blue-400', icon: Waves },
  { name: 'Square (same dices)', color: 'bg-purple-400', icon: Building2 },
]

const bonuses = [
  {
    name: 'House',
    activeColor: 'bg-amber-100 border-amber-400 text-amber-800',
    isActive: true,
  },
  {
    name: 'Forest',
    activeColor: 'bg-green-100 border-green-500 text-green-800',
    isActive: true,
  },
  {
    name: 'Lake',
    activeColor: 'bg-blue-100 border-blue-400 text-blue-800',
    isActive: true,
  },
]
</script>

<template>
  <div class="bg-white rounded-xl py-4 px-12 shadow-sm flex flex-col gap-4">
    <h3 class="font-semibold mx-auto text-2xl">Buildings</h3>

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

    <div class="mt-4 border-t pt-3 flex flex-col items-center gap-2">
      <h4 class="font-semibold text-lg">Bonuses</h4>
      <div class="flex justify-center gap-3">
        <div
          v-for="(bonus, i) in bonuses"
          :key="i"
          :class="[
            'px-4 py-1 border rounded-full text-sm font-medium shadow-sm transition-all duration-300',
            bonus.isActive ? bonus.activeColor : 'bg-slate-100 border-slate-300 text-slate-400',
          ]"
        >
          {{ bonus.name }}
        </div>
      </div>
    </div>
  </div>
</template>
