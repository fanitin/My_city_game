<script setup>
const dice = ['⚀', '⚁', '⚂', '⚃', '⚄', '⚅']
const rows = ['3,4', '5,6', '7', '8,9', '10,11']

const bonusMap = {
  '0-0': 3,
  '0-2': 2,
  '0-3': 2,
  '0-5': 3,
  '1-1': 1,
  '1-4': 1,
  '2-0': 2,
  '2-2': 1,
  '2-3': 1,
  '2-5': 2,
  '3-1': 1,
  '3-4': 1,
  '4-0': 3,
  '4-2': 2,
  '4-3': 2,
  '4-5': 3,
}

const getBonusClass = (points) => {
  switch (points) {
    case 3:
      return 'border-rose-500 shadow-[0_0_6px_#f43f5e55]'
    case 2:
      return 'border-sky-500 shadow-[0_0_6px_#0ea5e955]'
    case 1:
      return 'border-emerald-500 shadow-[0_0_6px_#10b98155]'
    default:
      return 'border-slate-300'
  }
}
</script>

<template>
  <div class="bg-white rounded-xl px-16 py-8 shadow-sm flex flex-col items-center">
    <div class="flex gap-3 items-center">
      <div class="flex flex-col justify-center gap-1">
        <div
          v-for="r in rows"
          :key="r"
          class="h-16 flex items-center justify-end text-sm text-slate-600 pr-1 w-10 translate-y-[3px]"
        >
          {{ r }}
        </div>
      </div>

      <div class="flex flex-col items-center">
        <div class="grid grid-cols-6 gap-1 mb-1 text-center">
          <div v-for="d in dice" :key="d" class="w-16 h-8 flex items-center justify-center text-3xl">
            {{ d }}
          </div>
        </div>

        <div class="grid grid-cols-6 gap-1">
          <template v-for="(r, i) in rows">
            <div
              v-for="(d, j) in dice"
              :key="`${i}-${j}`"
              class="w-16 h-16 border-2 rounded-md bg-slate-50 flex items-center justify-center text-sm transition-all duration-300"
              :class="getBonusClass(bonusMap[`${i}-${j}`])"
            >

            </div>
          </template>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.grid > div {
  position: relative;
}
</style>
