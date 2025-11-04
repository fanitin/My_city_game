<script setup lang="ts">
</script>

<template>
  <div class="bg-white rounded-xl py-4 px-6 shadow-sm flex flex-col gap-4 h-full">
    <div class="flex items-center gap-3 flex-none">
      <div class="w-8 h-8 bg-slate-200 rounded"></div>
      <h3 class="font-bold text-lg">Legenda i zasady gry</h3>
    </div>

    <div class="text-sm flex-1 overflow-y-auto pr-2 max-h-[70vh]">
      <h4 class="font-bold mb-2">Instrukcja do gry</h4>
      <p class="mb-4">
        Witaj, burmistrzu! Chwyć kartkę i ołówek – czas stworzyć miasteczko marzeń!
        Podczas gry będziesz stawiać domy, sadzić lasy, tworzyć jeziora oraz budować fabryki.
        Trochę szczęścia, sprytne decyzje i kto wie… może to właśnie Ty zostaniesz Burmistrzem Roku!
      </p>

      <h4 class="font-bold mb-1">Przygotowanie do gry</h4>
      <p class="mb-3">
        Na początku gry jeden z graczy rzuca dwiema kośćmi. Teraz czas na tajne planowanie:
        każdy wybiera dwa projekty, jeden w każdej kolumnie wskazanej przez kości.
        Na tym etapie gry place nie są jeszcze dostępne, każdy projekt możesz narysować tylko raz.
      </p>

      <h4 class="font-bold mb-1">Rozgrywka</h4>
      <p class="mb-3">
        W każdej rundzie jeden z graczy rzuca 2 kośćmi, a następnie wszyscy gracze równocześnie wykonują czynności przewidziane w fazach.
      </p>

      <h5 class="font-bold mb-1">Faza budowy</h5>
      <p class="mb-3">
        Korzystając z wartości na kostkach i katalogu projektów, każdy gracz musi narysować 2 projekty.
        Jeśli kolumna wskazana przez kość jest już zapełniona, rysuj w najbliższej pustej kolumnie.
        Gdy na obu kościach wypadnie ta sama wartość — narysuj projekt i plac.
      </p>

      <h5 class="font-bold mb-1">Faza bonusu</h5>
      <p class="mb-3">
        W rundach 3, 6 i 9 każdy gracz rysuje jeden bonusowy projekt w dowolnym pustym polu miasteczka.
      </p>

      <h5 class="font-bold mb-1">Faza punktacji</h5>
      <p class="mb-3">
        Suma wartości obu kości wskazuje ulicę, od której rozpoczyna się podliczanie punktów.
        Grupa projektów przynosi punkty tylko wtedy, gdy przynajmniej jeden z nich jest na wskazanej ulicy.
      </p>

      <h5 class="font-bold mb-1">Koniec gry</h5>
      <p>
        Po zakończeniu 9 rund każdy gracz sumuje wszystkie punkty i dodaje 10 punktów za każdy plac,
        który sąsiaduje z domem, lasem i stawem. Gracz z największą liczbą punktów wygrywa!
      </p>
    </div>

    <ul class="text-sm text-slate-600 space-y-3 flex-none">
      <li class="flex items-center gap-2">
        <div class="w-4 h-4 border-2 rounded-sm border-rose-500 shadow-[0_0_6px_#f43f5e55]"></div>
        <div><span class="font-semibold"> — 3 punkty</span></div>
      </li>
      <li class="flex items-center gap-2">
        <div class="w-4 h-4 border-2 rounded-sm border-sky-500 shadow-[0_0_6px_#0ea5e955]"></div>
        <div><span class="font-semibold"> — 2 punkty</span></div>
      </li>

      <li class="flex items-center gap-2">
        <div class="w-4 h-4 border-2 rounded-sm border-emerald-500 shadow-[0_0_6px_#10b98155]"></div>
        <div><span class="font-semibold"> — 1 punkt</span></div>
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
