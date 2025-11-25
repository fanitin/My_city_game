<script setup lang="ts">
import { Building2, Home, TreePine, Waves } from 'lucide-vue-next'
import { reactive, watch } from 'vue'

const buildings = reactive([
  {
    key: 'house',
    name: 'House (1 and 4)',
    color: 'bg-amber-400',
    icon: Home,
    isActive: false,
    used: false,
    rows: [],
  },
  {
    key: 'forest',
    name: 'Forest (2 and 5)',
    color: 'bg-green-500',
    icon: TreePine,
    isActive: false,
    used: false,
    rows: [],
  },
  {
    key: 'lake',
    name: 'Lake (3 and 6)',
    color: 'bg-blue-400',
    icon: Waves,
    isActive: false,
    used: false,
    rows: [],
  },
  {
    key: 'square',
    name: 'Square (same dices)',
    color: 'bg-purple-400',
    icon: Building2,
    isActive: false,
    used: false,
    rows: [],
  },
])

const bonuses = reactive([
  {
    key: 'house',
    name: 'House',
    activeColor: 'bg-amber-100 border-amber-400 text-amber-800',
    isActive: true,
    used: false,
    rows: [],
  },
  {
    key: 'forest',
    name: 'Forest',
    activeColor: 'bg-green-100 border-green-500 text-green-800',
    isActive: true,
    used: false,
    rows: [],
  },
  {
    key: 'lake',
    name: 'Lake',
    activeColor: 'bg-blue-100 border-blue-400 text-blue-800',
    isActive: true,
    used: false,
    rows: [],
  },
])

const props = defineProps<{
  dice1: number
  dice2: number
  round: number
  phase: 'Preparation' | 'Building' | 'Scoring' | 'Bonus'
  usedBuildings: string[]
  usedRows: number[]
  toBuild: number
}>()

const emit = defineEmits<{
  (e: 'selectBuilding', building: string, rows: number[]): void
  (e: 'cancelBuilding'): void
}>()

const selectedBuilding = reactive({
  key: null as string | null,
  bonus: false,
  rows: [] as number[],
})

function clickBuilding(buildingKey: string, isBonus = false) {
  const target = isBonus
    ? bonuses.find((b) => b.key === buildingKey)
    : buildings.find((b) => b.key === buildingKey)
  if (!target || !target.isActive) return
  if (selectedBuilding.key === buildingKey && selectedBuilding.bonus === isBonus) {
    selectedBuilding.key = null
    selectedBuilding.bonus = false
    selectedBuilding.rows = []
    emit('cancelBuilding')
    return
  }
  selectedBuilding.key = buildingKey

  selectedBuilding.bonus = isBonus
  if (props.phase == 'Bonus' || buildingKey == 'square') {
    selectedBuilding.rows = [1, 2, 3, 4, 5, 6]
  } else if (props.phase == 'Preparation') {
    if(!props.usedRows.includes(props.dice1)) selectedBuilding.rows.push(props.dice1)
    if(!props.usedRows.includes(props.dice2) || props.dice1 == props.dice2) selectedBuilding.rows.push(props.dice2)
  } else {
    const d1 = props.dice1
    const d2 = props.dice2

    let rows: number[] = []

    if (buildingKey === 'house') {
      if ([1, 4].includes(d1) && !props.usedRows.includes(props.dice2)) rows.push(d2)
      if ([1, 4].includes(d2) && !props.usedRows.includes(props.dice1)) rows.push(d1)
    }

    if (buildingKey === 'forest') {
      if ([2, 5].includes(d1) && !props.usedRows.includes(props.dice2)) rows.push(d2)
      if ([2, 5].includes(d2) && !props.usedRows.includes(props.dice1)) rows.push(d1)
    }

    if (buildingKey === 'lake') {
      if ([3, 6].includes(d1) && !props.usedRows.includes(props.dice2)) rows.push(d2)
      if ([3, 6].includes(d2) && !props.usedRows.includes(props.dice1)) rows.push(d1)
    }

    selectedBuilding.rows = [...new Set(rows)]
  }
  emit('selectBuilding', selectedBuilding.key, selectedBuilding.rows)
}

watch(
  () => [props.dice1, props.dice2, props.round, props.phase],
  ([d1, d2, round, phase]) => {
  if(d1 == null && d2 == null){
    buildings.forEach((b) => {
      b.isActive = false
      b.used = false
      b.rows = []
    })
    if(round && [3, 6, 9].includes(round)){
      bonuses.forEach((b) => {
        b.isActive = !b.used
        b.rows = []
      })
    }
    return
  }

    buildings.forEach((b) => {
      b.isActive = false
      b.used = false
      b.rows = []
    })
    if(round && [3, 6, 9].includes(round)){
      bonuses.forEach((b) => {
        b.isActive = !b.used
        b.rows = []
      })
    }

    if (!phase) return

    if (phase === 'Preparation' && round === 0) {
      buildings.forEach((b) => (b.isActive = false))

      buildings.forEach((b) => {
        if (b.key !== 'square' && !b.used) b.isActive = true
      })
    }

    if (phase === 'Building' && round && round > 0 && round < 10 && d1 && d2) {
      buildings.forEach((b) => {
        if (!b.used) {
          b.isActive =
            ((b.key === 'house' && ([1, 4].includes(d1) || [1, 4].includes(d2))) ||
            (b.key === 'forest' && ([2, 5].includes(d1) || [2, 5].includes(d2))) ||
            (b.key === 'lake' && ([3, 6].includes(d1) || [3, 6].includes(d2))) ||
            (b.key === 'square' && d1 === d2))
        }
      })
    }

    if (phase === 'Bonus' && round && [3, 6, 9].includes(round)) {
      bonuses.forEach((b) => {
        if (!b.used) b.isActive = true
      })
    }
  },
)

watch(
  () => props.usedBuildings,
  (used) => {
    selectedBuilding.key = null
    selectedBuilding.bonus = false
    selectedBuilding.rows = []
    buildings.forEach((b) => {
      if (used.includes(b.key) && !(props.phase == 'Building' && props.dice1 != props.dice2 && (
        ([1,4].includes(props.dice1) && [1,4].includes(props.dice2)) ||
        ([2,5].includes(props.dice1) && [2,5].includes(props.dice2)) ||
        ([3,6].includes(props.dice1) && [3,6].includes(props.dice2))
      ))) {
        b.used = true
        b.isActive = false
      }
    })

    if(props.phase == 'Bonus'){
      bonuses.forEach((b) => {
        if (used.includes(b.key)) {
          b.used = true
        }
      })
    }
  },
  { deep: true, immediate: true },
)
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
          selectedBuilding.key === b.key && !selectedBuilding.bonus
            ? 'ring-2 ring-offset-2 ring-indigo-400'
            : '',
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
          @click="phase == 'Bonus' ? clickBuilding(b.key, true) : null"
          :class="[
            'px-4 py-1 border rounded-full text-sm font-medium shadow-sm transition-all duration-300',
            b.isActive ? b.activeColor : 'bg-slate-100 border-slate-300 text-slate-400',
            phase == 'Bonus' ? 'cursor-pointer' : '',
            selectedBuilding.key === b.key && selectedBuilding.bonus
              ? 'ring-2 ring-offset-2 ring-indigo-400'
              : '',
          ]"
        >
          {{ b.name }}
        </div>
      </div>
    </div>
  </div>
</template>
