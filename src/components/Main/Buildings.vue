<script setup lang="ts">
import { Home, TreePine, Waves, Building2 } from 'lucide-vue-next'
import { reactive, watch } from 'vue'

const buildings = reactive([
  { key: 'house', name: 'House (1 and 4)', color: 'bg-amber-400', icon: Home, isActive: false, used: false },
  { key: 'forest', name: 'Forest (2 and 5)', color: 'bg-green-500', icon: TreePine, isActive: false, used: false },
  { key: 'lake', name: 'Lake (3 and 6)', color: 'bg-blue-400', icon: Waves, isActive: false, used: false },
  { key: 'square', name: 'Square (same dices)', color: 'bg-purple-400', icon: Building2, isActive: false, used: false },
])

const bonuses = reactive([
  { key: 'house', name: 'House', activeColor: 'bg-amber-100 border-amber-400 text-amber-800', isActive: true, used: false },
  { key: 'forest', name: 'Forest', activeColor: 'bg-green-100 border-green-500 text-green-800', isActive: true, used: false },
  { key: 'lake', name: 'Lake', activeColor: 'bg-blue-100 border-blue-400 text-blue-800', isActive: true, used: false },
])

const props = defineProps<{
  dice1: number | undefined
  dice2: number | undefined
  round: number | undefined
  phase: 'Preparation' | 'Building' | 'Scoring' | 'Bonus' | undefined
}>()

const emit = defineEmits<{
  (e: 'selectBuilding', building: string, row: number[]): void
}>()

const selectedBuilding = reactive({
  key: null as string | null,
  bonus: false
})

watch(
  () => [props.dice1, props.dice2, props.round, props.phase],
  ([d1, d2, round, phase]) => {
    buildings.forEach(b => b.isActive = false)
    bonuses.forEach(b => b.isActive = !b.used)

    if (!phase) return

    if (phase === 'Preparation' && round === 0) {
      buildings.forEach(b => b.isActive = false)

      buildings.forEach(b => {
        if (b.key !== 'square' && !b.used) b.isActive = true
      })
    }


    if (phase === 'Building' && round && round > 0 && round < 10 && d1 && d2) {
      buildings.forEach(b => {
        if (!b.used) {
          const matchD =
            (b.key === 'house' && [1,4].includes(d1) || [1,4].includes(d2)) ||
            (b.key === 'forest' && [2,5].includes(d1) || [2,5].includes(d2)) ||
            (b.key === 'lake' && [3,6].includes(d1) || [3,6].includes(d2)) ||
            (b.key === 'square' && d1 === d2)
          b.isActive = !!matchD
        }
      })
    }

    if (phase === 'Bonus' && round && [3,6,9].includes(round)) {
      bonuses.forEach(b => { if (!b.used) b.isActive = true })
    }
  }
)

function clickBuilding(buildingKey: string, isBonus = false) {
  const target = isBonus ? bonuses.find(b => b.key === buildingKey) : buildings.find(b => b.key === buildingKey)
  if (!target || !target.isActive) return

  if (selectedBuilding.key === buildingKey && selectedBuilding.bonus === isBonus) {
    selectedBuilding.key = null
    selectedBuilding.bonus = false
  } else {
    selectedBuilding.key = buildingKey
    selectedBuilding.bonus = isBonus
  }

  emit('selectBuilding', selectedBuilding.key)
}
</script>

<template>
  <div class="bg-white rounded-xl py-4 px-12 shadow-sm flex flex-col gap-4">
    <h3 class="font-semibold mx-auto text-2xl">Buildings</h3>

    <div class="flex flex-col gap-2">
      <button
        v-for="(b, i) in buildings"
        :key="i"
        @click="clickBuilding(b.key)"
        :class="[
          'flex items-center gap-3 p-2 rounded transition',
          b.isActive ? 'hover:bg-slate-50 cursor-pointer' : 'opacity-40 cursor-not-allowed',
          selectedBuilding.key === b.key && !selectedBuilding.bonus ? 'ring-2 ring-offset-2 ring-indigo-400' : ''
        ]"
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
          v-for="(b, i) in bonuses"
          :key="i"
          @click="clickBuilding(b.key, true)"
          :class="[
            'px-4 py-1 border rounded-full text-sm font-medium shadow-sm transition-all duration-300',
            b.isActive ? b.activeColor : 'bg-slate-100 border-slate-300 text-slate-400',
            selectedBuilding.key === b.key && selectedBuilding.bonus ? 'ring-2 ring-offset-2 ring-indigo-400' : ''
          ]"
        >
          {{ b.name }}
        </div>
      </div>
    </div>
  </div>
</template>
