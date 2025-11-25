<script setup lang="ts">
import { ref, watch } from 'vue'

const emit = defineEmits<{
  (e: 'log', message: string): void
  (
    e: 'mainInfo',
    dice1: number,
    dice2: number,
    round: number,
    phase: 'Preparation' | 'Building' | 'Scoring' | 'Bonus',
    buildingsNumber: number,
  ): void
  (e: 'phase', phase: 'Preparation' | 'Building' | 'Scoring' | 'Bonus'): void
  (e: 'resetOnRound'): void
}>()

const props = defineProps<{
  toBuild: number
}>()

const currentRound = ref(-1)
const currentPhase = ref<'Preparation' | 'Building' | 'Scoring' | 'Bonus'>('Preparation')

const diceFaces = ['⚀', '⚁', '⚂', '⚃', '⚄', '⚅']
const dice1Face = ref('⚀')
const dice2Face = ref('⚀')
const dice1Value = ref(1)
const dice2Value = ref(1)
const rolling = ref(false)
const toBuild = ref(0)
const canBeClicked = ref(true)

const rollDice = () => {
  emit('resetOnRound')
  canBeClicked.value = false
  currentRound.value++
  if(currentRound.value > 0) currentPhase.value = 'Building'
  rolling.value = true

  const interval = setInterval(() => {
    const i1 = Math.floor(Math.random() * 6)
    const i2 = Math.floor(Math.random() * 6)
    dice1Face.value = diceFaces[i1]
    dice2Face.value = diceFaces[i2]
    dice1Value.value = i1 + 1
    dice2Value.value = i2 + 1
  }, 150)

  setTimeout(() => {
    clearInterval(interval)
    const i1 = Math.floor(Math.random() * 6)
    const i2 = Math.floor(Math.random() * 6)
    dice1Face.value = diceFaces[i1]
    dice2Face.value = diceFaces[i2]
    dice1Value.value = i1 + 1
    dice2Value.value = i2 + 1
    rolling.value = false

    if(currentRound.value == 3 || currentRound.value == 6 || currentRound.value == 9) toBuild.value = 3
    else toBuild.value = 2

    emit('log', `You've rolled ${dice1Value.value} and ${dice2Value.value}`)
    emit('mainInfo', dice1Value.value, dice2Value.value, currentRound.value, currentPhase.value, toBuild.value)

    if (currentRound.value === 0) {
      currentPhase.value = 'Preparation'
    } else {
      currentPhase.value = 'Building'
    }
  }, 3000)
}

const restartGame = () => {
  globalThis.location.reload()
}

watch(
  () => props.toBuild,
  (remain) => {
    if(currentRound.value > 0 && currentRound.value % 3 == 0 && remain == 1){
      currentPhase.value = 'Bonus'
    } else if(currentRound.value > 0 && remain == 0){
      currentPhase.value = 'Scoring'
    }
    if(remain == 0){
      canBeClicked.value = true
    }
    emit('phase', currentPhase.value)
  },
)

watch(
  () => currentRound.value,
  (round) => {
    if(round == 9) canBeClicked.value = false
  }
)
</script>

<template>
  <div class="flex items-center justify-between w-full max-w-4xl mx-auto gap-6">
    <div class="flex flex-col items-start text-left">
      <div class="text-sm text-slate-600">
        <span class="font-semibold text-slate-800">Round: </span>
        <span class="ml-1">{{(currentRound >= 0 ? currentRound : '0') }}/9</span>
      </div>
      <div class="text-sm text-slate-600">
        <span class="font-semibold text-slate-800">Phase: </span>
        <span class="ml-1">{{ currentPhase }}</span>
      </div>
    </div>

    <div
      class="flex items-center justify-center flex-1 transition-transform duration-300 hover:scale-105"
    >
      <div @click="canBeClicked ? rollDice() : null" class="flex items-center justify-center gap-8 cursor-pointer">
        <div
          :class="[
            'w-24 h-24 bg-white border-2 border-slate-300 rounded-2xl grid place-items-center text-7xl shadow-lg transition-transform duration-300',
            rolling ? 'animate-spin-slow' : '',
          ]"
        >
          {{ dice1Face }}
        </div>
        <div
          :class="[
            'w-24 h-24 bg-white border-2 border-slate-300 rounded-2xl grid place-items-center text-7xl shadow-lg transition-transform duration-300',
            rolling ? 'animate-spin-slow' : '',
          ]"
        >
          {{ dice2Face }}
        </div>
      </div>
    </div>

    <div class="flex flex-col items-end">
      <button
        @click="restartGame"
        class="px-4 py-2 bg-red-500 text-white text-sm font-medium rounded-lg shadow hover:bg-red-600 transition"
      >
        Restart Game
      </button>
    </div>
  </div>
</template>

<style scoped>
@keyframes spin-slow {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(720deg);
  }
}

.animate-spin-slow {
  animation: spin-slow 3s ease-in-out;
}
</style>
