<script setup lang="ts">
import { ref } from 'vue'

const dice1 = ref('⚀')
const dice2 = ref('⚀')
const rolling = ref(false)

const rollDice = () => {
  if (rolling.value) return

  rolling.value = true
  const diceFaces = ['⚀', '⚁', '⚂', '⚃', '⚄', '⚅']

  const interval = setInterval(() => {
    dice1.value = diceFaces[Math.floor(Math.random() * 6)]
    dice2.value = diceFaces[Math.floor(Math.random() * 6)]
  }, 150)

  setTimeout(() => {
    clearInterval(interval)
    dice1.value = diceFaces[Math.floor(Math.random() * 6)]
    dice2.value = diceFaces[Math.floor(Math.random() * 6)]
    rolling.value = false
  }, 3000)
}
</script>

<template>
  <div class="flex items-center justify-between w-full max-w-4xl mx-auto gap-6">
    <div class="flex flex-col items-start text-left">
      <div class="text-sm text-slate-600">
        <span class="font-semibold text-slate-800">Round: </span>
        <span class="ml-1">1 / 9</span>
      </div>
      <div class="text-sm text-slate-600">
        <span class="font-semibold text-slate-800">Phase: </span>
        <span class="ml-1">IDK TESTUJE</span>
      </div>
    </div>

    <div
      class="flex items-center justify-center flex-1 transition-transform duration-300 hover:scale-105"
    >
      <div @click="rollDice" class="flex items-center justify-center gap-8 cursor-pointer">
        <div
          :class="[
            'w-24 h-24 bg-white border-2 border-slate-300 rounded-2xl grid place-items-center text-7xl shadow-lg transition-transform duration-300',
            rolling ? 'animate-spin-slow' : '',
          ]"
        >
          {{ dice1 }}
        </div>
        <div
          :class="[
            'w-24 h-24 bg-white border-2 border-slate-300 rounded-2xl grid place-items-center text-7xl shadow-lg transition-transform duration-300',
            rolling ? 'animate-spin-slow' : '',
          ]"
        >
          {{ dice2 }}
        </div>
      </div>
    </div>

    <div class="flex flex-col items-end">
      <button
        @click="() => console.log('Restarting game...')"
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
