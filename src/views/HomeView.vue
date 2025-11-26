<script setup lang="ts">
import DiceHeader from '@/components/Main/DiceHeader.vue'
import PlayersScore from '@/components/Main/PlayersScore.vue'
import Legend from '@/components/Main/Legend.vue'
import GameBoard from '@/components/Main/GameBoard.vue'
import Buildings from '@/components/Main/Buildings.vue'
import Logs from '@/components/Main/Logs.vue'
import { reactive, ref, watch } from 'vue'

type Cell = {
  bonus?: 1 | 2 | 3
  building?: 'House' | 'Forest' | 'Lake' | 'Square'
  bonusActive: boolean
}

const dice1Value = ref<number>()
const dice2Value = ref<number>()
const currentRound = ref<number>()
const currentPhase = ref<'Preparation' | 'Building' | 'Scoring' | 'Bonus'>()
const log = ref<string[]>([])
const toBuild = ref(0)
const usedBuildings = ref<string[]>([])
const usedRows = ref<number[]>([])
const totalScore = ref(0)
const currentRoundScore = ref(0)
const gameBoardState = ref<Cell[][]>([])
const showEndGameModal = ref(false)
const endGameTitle = ref('')

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
    if ([3, 6, 9].includes(currentRound.value) && val == 1) {
      usedBuildings.value = []
      usedRows.value = []
    } else if (val == 0) {
      if (currentRound.value > 0) {
        calculateScore()
      }
    }
  },
)

const handleBoardUpdated = (board: Cell[][]) => {
  gameBoardState.value = board
}

const calculateScore = () => {
  const board = gameBoardState.value
  if (!board || board.length === 0) return

  const targetRowIndex = getRowFromDice()
  if (targetRowIndex === null) return

  let rowScore = 0
  const used = new Set<string>()

  const stack = []
  const dirs = [
    [1, 0],
    [-1, 0],
    [0, 1],
    [0, -1],
  ]

  const add = (r: number, c: number) => `${r}-${c}`

  const explore = (r: number, c: number, type: string) => {
    stack.push([r, c])

    while (stack.length) {
      const [cr, cc] = stack.pop()
      const id = add(cr, cc)

      if (used.has(id)) continue
      used.add(id)

      const cell = board[cr][cc]

      if (cell.bonus) rowScore += cell.bonus

      for (const [dr, dc] of dirs) {
        const nr = cr + dr
        const nc = cc + dc
        if (board[nr]?.[nc]?.building === type) {
          const nid = add(nr, nc)
          if (!used.has(nid)) {
            stack.push([nr, nc])
          }
        }
      }
    }
  }

  for (let col = 0; col < board[targetRowIndex].length; col++) {
    const cell = board[targetRowIndex][col]

    if (cell.building) {
      const id = add(targetRowIndex, col)
      if (!used.has(id)) {
        explore(targetRowIndex, col, cell.building)
      }
    }
  }

  currentRoundScore.value = rowScore
  totalScore.value += rowScore

  handleLog(`You earned ${rowScore} points this round`)

  if (currentRound.value === 9) {
    let squareBonus = 0

    for (let r = 0; r < board.length; r++) {
      for (let c = 0; c < board[r].length; c++) {
        const cell = board[r][c]
        if (cell.building === 'Square') {
          const types = new Set()

          if (board[r - 1]?.[c]?.building) types.add(board[r - 1][c].building)
          if (board[r + 1]?.[c]?.building) types.add(board[r + 1][c].building)
          if (board[r]?.[c - 1]?.building) types.add(board[r][c - 1].building)
          if (board[r]?.[c + 1]?.building) types.add(board[r][c + 1].building)

          if (types.size === 4) {
            squareBonus += 10
          }
        }
      }
    }

    totalScore.value += squareBonus
    handleLog(`Square bonus: +${squareBonus}`)
  }
  if (currentRound.value === 9) {
    showEndGameModal.value = true
    endGameTitle.value = calculateTitle(totalScore.value)
  }
}

const calculateTitle = (score: number) => {
  if (score <= 60) return 'Zarządca chodników'
  if (score <= 75) return 'Władca rond i placów zabaw'
  if (score <= 90) return 'Szef wszystkiego po trochu'
  return 'Wizjoner metropolii'
}

const getRowFromDice = () => {
  if (dice1Value.value == null || dice2Value.value == null) return null

  const sum = dice1Value.value + dice2Value.value

  switch (sum) {
    case 3:
    case 4:
      return 0
    case 5:
    case 6:
      return 1
    case 7:
      return 2
    case 8:
    case 9:
      return 3
    case 10:
    case 11:
      return 4
    default:
      return null
  }
}

const handleResetOnRound = () => {
  dice1Value.value = null
  dice2Value.value = null
  selectedBuilding.key = null
  selectedBuilding.rows = []
  usedRows.value = []
  usedBuildings.value = []
}

const restartGame = () => {
  globalThis.location.reload()
}
</script>

<template>
  <main class="w-full h-screen overflow-hidden">
    <div
      v-if="showEndGameModal"
      class="fixed inset-0 bg-black/50 flex justify-center items-center z-50"
    >
      <div class="bg-white rounded-2xl p-8 w-2/3 max-w-lg shadow-lg text-center text-black">
        <h2 class="text-2xl font-bold mb-4">Koniec gry!</h2>
        <p class="text-xl mb-4">Twoje końcowe punkty: {{ totalScore }}</p>
        <p class="text-lg font-semibold mb-6">Tytuł: {{ endGameTitle }}</p>
        <button
          class="px-6 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600"
          @click="restartGame"
        >
          Restart Game
        </button>
      </div>
    </div>
    <div class="flex flex-col h-full bg-slate-100 text-slate-800">
      <div class="flex justify-between items-center px-6 py-4 bg-slate-200 shadow flex-none">
        <div class="w-1/5"></div>

        <div class="flex flex-col items-center gap-3 w-3/5">
          <DiceHeader
            :toBuild="toBuild"
            @log="handleLog"
            @mainInfo="handleMainInfo"
            @phase="handlePhase"
            @resetOnRound="handleResetOnRound"
          />
        </div>

        <div class="w-1/5 flex justify-end items-center">
          <div class="rounded-xl bg-white px-4 py-2 shadow-md border border-slate-300">
            <PlayersScore :score="totalScore" />
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
            @boardUpdated="handleBoardUpdated"
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

      <div class="flex-none px-4 flex justify-center items-center">
        <Logs :log="log" />
      </div>
    </div>
  </main>
</template>
