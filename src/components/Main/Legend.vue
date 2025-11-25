<script setup lang="ts">
</script>

<template>
  <div class="bg-white rounded-xl py-4 px-6 shadow-sm flex flex-col gap-4 h-full">
    <div class="flex items-center gap-3 flex-none">
      <div class="w-8 h-8 bg-slate-200 rounded"></div>
      <h3 class="font-bold text-lg">Game Legend and Rules</h3>
    </div>

    <div class="text-sm flex-1 overflow-y-auto pr-2 max-h-[70vh]">
      <h4 class="font-bold mb-2">Game Instructions</h4>
      <p class="mb-4">
        Welcome, Mayor! Grab a sheet and pencil — it's time to build your dream town!
        During the game, you will place Houses, Plant Forests, create Lakes, and construct Squares.
        Luck, strategy, and clever choices will decide if you become the Mayor of the Year!
      </p>

      <h4 class="font-bold mb-1">Preparation (Round 0)</h4>
      <p class="mb-3">
        Roll two dice. Place <strong>two buildings</strong> in the rows corresponding to the dice values:
        - First building: any type, row = value of the second dice.
        - Second building: any type, row = value of the first dice.
        The two buildings must be different types. Squares are <strong>not placed yet</strong> in this phase.
        No points are scored during preparation.
      </p>

      <h4 class="font-bold mb-1">Rounds 1–9</h4>
      <p class="mb-3">
        Each round consists of three phases. Roll two dice, then execute the phases in order:
      </p>

      <h5 class="font-bold mb-1">1. Construction Phase</h5>
      <p class="mb-3">
        Place <strong>two buildings</strong> according to the dice:
        - Building of type Dice A → row = value of Dice B
        - Building of type Dice B → row = value of Dice A
        If both dice show the same number, place the corresponding building and <strong>one Square</strong> anywhere on the map.
      </p>

      <h5 class="font-bold mb-1">2. Bonus Phase</h5>
      <p class="mb-3">
        Only in rounds 3, 6, and 9: place <strong>one bonus tile</strong> (House, Forest, or Lake)
        anywhere on the map. The bonus becomes inactive after placement.
      </p>

      <h5 class="font-bold mb-1">3. Scoring Phase</h5>
      <p class="mb-3">
        Sum the values of both dice to determine the row for scoring.
        Buildings score points <strong>only if placed on colored bonus tiles</strong>:
        - <span class="text-rose-500">Rose tile</span> → 3 points
        - <span class="text-sky-500">Sky tile</span> → 2 points
        - <span class="text-emerald-500">Emerald tile</span> → 1 point
        Squares are not counted in this phase.
      </p>

      <h5 class="font-bold mb-1">End of Game</h5>
      <p>
        After 9 rounds, sum all points from bonus tiles.
        Additionally, for each Square adjacent to at least one House, one Forest, and one Lake, add <strong>+10 points</strong>.
        The final score represents your solo play result.
      </p>
    </div>

    <ul class="text-sm text-slate-600 space-y-3 flex-none">
      <li class="flex items-center gap-2">
        <div class="w-4 h-4 border-2 rounded-sm border-rose-500 shadow-[0_0_6px_#f43f5e55]"></div>
        <div><span class="font-semibold"> — 3 points</span></div>
      </li>
      <li class="flex items-center gap-2">
        <div class="w-4 h-4 border-2 rounded-sm border-sky-500 shadow-[0_0_6px_#0ea5e955]"></div>
        <div><span class="font-semibold"> — 2 points</span></div>
      </li>
      <li class="flex items-center gap-2">
        <div class="w-4 h-4 border-2 rounded-sm border-emerald-500 shadow-[0_0_6px_#10b98155]"></div>
        <div><span class="font-semibold"> — 1 point</span></div>
      </li>
    </ul>
  </div>
</template>

<style scoped>
::-webkit-scrollbar {
  width: 6px;
}
::-webkit-scrollbar-thumb {
  background-color: #cbd5e1;
  border-radius: 3px;
}
::-webkit-scrollbar-thumb:hover {
  background-color: #94a3b8;
}
</style>
