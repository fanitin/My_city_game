<script setup lang="ts">
import DiceHeader from '@/components/Main/DiceHeader.vue'
import PlayersScore from '@/components/Main/PlayersScore.vue'
import Legend from '@/components/Main/Legend.vue'
import GameBoard from '@/components/Main/GameBoard.vue'
import Buildings from '@/components/Main/Buildings.vue'
import Logs from '@/components/Main/Logs.vue'
import { ref } from 'vue'

const dice1Value = ref<number>()
const dice2Value = ref<number>()
const selectedBuilding = ref<string | null>(null)
const currentRound = ref(0)
const currentPhase = ref<'Preparation' | 'Building' | 'Scoring' | 'Bonus'>('Preparation')
const log = ref<string[]>([])

const handleLog = (message: string) => {
  log.value.push(message)
}

const handleDices = (dice1: number, dice2: number) => {
  dice1Value.value = dice1
  dice2Value.value = dice2
}
</script>

<template>
  <main class="w-full h-screen overflow-hidden">
    <div class="flex flex-col h-full bg-slate-100 text-slate-800">
      <div class="flex justify-between items-center px-6 py-4 bg-slate-200 shadow flex-none">
        <div class="w-1/5"></div>

        <div class="flex flex-col items-center gap-3 w-3/5">
          <DiceHeader @log="handleLog" @dices="handleDices"/>
        </div>

        <div class="w-1/5 flex justify-end items-center">
          <div class="rounded-xl bg-white px-4 py-2 shadow-md border border-slate-300">
            <PlayersScore :score="10" />
          </div>
        </div>
      </div>

      <div class="flex flex-1 px-6 py-2 my-4 gap-4 overflow-hidden justify-center items-stretch">
        <div class="w-1/5 overflow-hidden ml-8 flex flex-col">
          <Legend class="flex-1 overflow-hidden" />
        </div>

        <div class="flex-1 flex justify-center items-center overflow-hidden w-3/5">
          <GameBoard />
        </div>

        <div class="w-1/5 overflow-hidden mr-8 flex flex-col">
          <Buildings class="flex-1 overflow-hidden" />
        </div>
      </div>


      <div class="flex-none px-6 pb-2 flex justify-center items-center">
        <Logs :log="log"/>
      </div>
    </div>
  </main>
</template>

