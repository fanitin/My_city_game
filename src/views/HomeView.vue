<script setup lang="ts">
import DiceHeader from '@/components/Main/DiceHeader.vue'
import PlayersScore from '@/components/Main/PlayersScore.vue'
import Legend from '@/components/Main/Legend.vue'
import GameBoard from '@/components/Main/GameBoard.vue'
import Buildings from '@/components/Main/Buildings.vue'
import Logs from '@/components/Main/Logs.vue'
import { reactive, ref, watch } from 'vue'

const dice1Value = ref<number>()
const dice2Value = ref<number>()
const currentRound = ref<number>()
const currentPhase = ref<'Preparation' | 'Building' | 'Scoring' | 'Bonus'>()
const log = ref<string[]>([])
const toBuild = ref(0)
const usedBuildings = ref<string[]>([])
const usedRows = ref<number[]>([])

const selectedBuilding = reactive({
  key: null as string | null,
  rows: [] as number[],
})

const handleLog = (message: string) => {
  log.value.push(message)
}

const handleMainInfo = (
  dice1: number,
  dice2: number,
  round: number,
  phase: 'Preparation' | 'Building' | 'Scoring' | 'Bonus',
  buildingsNumber: number,
) => {
  dice1Value.value = dice1
  dice2Value.value = dice2
  currentRound.value = round
  currentPhase.value = phase
  toBuild.value = buildingsNumber
}

const handleSelectedBuilding = (building: string, rows: []) => {
  selectedBuilding.key = building
  selectedBuilding.rows = rows
}

const handleCancelBuilding = () => {
  selectedBuilding.key = null
  selectedBuilding.rows = []
}

const handleBuildingPlaced = (row: number) => {
  usedBuildings.value.push(selectedBuilding.key)
  usedRows.value.push(row)
  handleLog(`You've placed ${selectedBuilding.key}`)
  selectedBuilding.key = null
  selectedBuilding.rows = []
  toBuild.value--
}

const handlePhase = (phase: 'Preparation' | 'Building' | 'Scoring' | 'Bonus') => {
  currentPhase.value = phase
}

watch(
  () => toBuild.value,
  (val) => {
    if([3, 6, 9].includes(currentRound.value) && val == 1){
      usedBuildings.value = []
      usedRows.value = []
    } else if([3, 6, 9].includes(currentRound.value) && val == 0){
      return
    } else if(val == 0){
      dice1Value.value = null
      dice2Value.value = null
      selectedBuilding.key = null
      selectedBuilding.rows = []
      usedRows.value = []
      usedBuildings.value = []
    }
  }
)
</script>

<template>
  <div>
    {{ dice1Value }}
    {{ dice2Value }}
    {{ currentPhase }}
    {{ currentRound }}
    {{ selectedBuilding }}
    {{ toBuild }}
    {{ usedBuildings }}
    {{usedRows}}
  </div>
  <main class="w-full h-screen overflow-hidden">
    <div class="flex flex-col h-full bg-slate-100 text-slate-800">
      <div class="flex justify-between items-center px-6 py-4 bg-slate-200 shadow flex-none">
        <div class="w-1/5"></div>

        <div class="flex flex-col items-center gap-3 w-3/5">
          <DiceHeader :toBuild="toBuild" @log="handleLog" @mainInfo="handleMainInfo" @phase="handlePhase"/>
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
          <GameBoard
            :selectedBuilding="selectedBuilding.key"
            :rows="selectedBuilding.rows"
            :round="currentRound"
            :phase="currentPhase"
            @buildingPlaced="handleBuildingPlaced"
          />
        </div>

        <div class="w-1/5 overflow-hidden mr-8 flex flex-col">
          <Buildings
            class="flex-1 overflow-hidden"
            :toBuild="toBuild"
            :dice1="dice1Value"
            :dice2="dice2Value"
            :round="currentRound"
            :phase="currentPhase"
            :usedBuildings="usedBuildings"
            :usedRows="usedRows"
            @selectBuilding="handleSelectedBuilding"
            @cancelBuilding="handleCancelBuilding"
          />
        </div>
      </div>

      <div class="flex-none px-6 pb-2 flex justify-center items-center">
        <Logs :log="log" />
      </div>
    </div>
  </main>
</template>
