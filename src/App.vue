<script setup>
import { onMounted, ref } from 'vue'
import BoomMode from './components/BoomMode.vue'
import FlashCard from './components/FlashCard.vue'

const isLoading = ref(true)
const hasError = ref(false)
const words = ref([])
const bloomSeed = ref(0)
const isChallengeMode = ref(false)

const normalizeWords = (payload) => {
  if (Array.isArray(payload)) return payload
  if (Array.isArray(payload?.words)) return payload.words
  if (Array.isArray(payload?.data)) return payload.data
  return []
}

const fetchWords = async () => {
  isLoading.value = true
  hasError.value = false
  words.value = [] // Limpa os cards antigos para a animação rodar de novo

  try {
    const response = await fetch('https://fiap-bff-2502.onrender.com/ask')
    if (!response.ok) throw new Error('Falha ao buscar palavras')

    const data = await response.json()
    words.value = normalizeWords(data).slice(0, 5)
    bloomSeed.value += 1
  } catch (error) {
    console.error(error)
    hasError.value = true
  } finally {
    isLoading.value = false
  }
}

onMounted(fetchWords)
</script>

<template>
  <main class="relative min-h-screen overflow-hidden bg-slate-900 text-slate-100">
    <div class="pointer-events-none absolute -left-28 -top-28 h-[30rem] w-[30rem] rounded-full bg-purple-600 opacity-30 blur-[100px]" />
    <div class="pointer-events-none absolute -bottom-28 -right-24 h-[30rem] w-[30rem] rounded-full bg-pink-600 opacity-30 blur-[100px]" />

    <section class="relative z-10 mx-auto flex w-full max-w-[82rem] flex-col items-center px-3 py-14 sm:px-6">
      <div class="absolute right-4 top-6 flex items-center gap-2 text-base text-slate-200/90 sm:right-8">
        <span>🔥 Challenge Mode</span>
        <button
          type="button"
          aria-label="Alternar challenge mode"
          class="h-7 w-14 rounded-full border border-white/10 bg-slate-700/60 p-1 transition-all"
          :disabled="isLoading"
          :class="[
            isChallengeMode ? 'bg-red-700/80' : 'bg-slate-700/60',
            isLoading ? 'opacity-50 cursor-not-allowed' : 'cursor-pointer',
          ]"
          @click="isChallengeMode = !isChallengeMode"
        >
          <span
            class="block h-5 w-5 rounded-full bg-white transition-transform"
            :class="isChallengeMode ? 'translate-x-7' : 'translate-x-0'"
          />
        </button>
      </div>

      <header class="mb-14 text-center">
        <span class="mb-3 inline-block text-3xl drop-shadow-lg">🌸</span>
        <h1 class="bg-gradient-to-r from-purple-400 via-pink-500 to-purple-400 bg-clip-text pb-2 text-5xl font-extrabold tracking-tight text-transparent sm:text-7xl">
          Bloom Words
        </h1>
        <p class="mt-3 text-lg text-slate-300 sm:text-xl">
          Suas 5 palavras de hoje para destravar o inglês.
        </p>
      </header>

      <div class="flex min-h-[22rem] w-full flex-col items-center justify-center">
        <div v-if="isLoading" class="flex flex-col items-center gap-4">
          <div class="text-5xl animate-pulse drop-shadow-[0_0_18px_rgba(236,72,153,0.5)]">🌸</div>
          <span class="bg-gradient-to-r from-purple-400 to-pink-400 bg-clip-text text-xl font-medium text-transparent">
            Desabrochando palavras...
          </span>
        </div>

        <div
          v-else-if="hasError"
          class="rounded-2xl border border-pink-500/30 bg-slate-800/70 px-8 py-6 text-center text-slate-300 shadow-xl"
        >
          <p class="mb-2 text-xl">🥀 Oops!</p>
          <p>A raiz das palavras secou. Tente novamente mais tarde.</p>
        </div>

        <div v-else-if="isChallengeMode && words.length" class="w-full">
          <BoomMode
            :words="words"
            @onRestart="fetchWords"
            @onNextStage="fetchWords"
          />
        </div>

        <div v-else class="w-full">
          <div class="grid w-full grid-cols-1 place-items-center gap-3 sm:grid-cols-2 lg:hidden">
            <FlashCard
              v-for="(card, index) in words"
              :key="`${card.word}-${index}-${bloomSeed}`"
              :card="card"
              :index="index"
            />
          </div>

          <div class="hidden w-full lg:flex lg:flex-col lg:items-center lg:gap-3">
            <div class="flex items-stretch justify-center gap-3">
              <FlashCard
                v-for="(card, index) in words.slice(0, 3)"
                :key="`${card.word}-${index}-${bloomSeed}`"
                :card="card"
                :index="index"
              />
            </div>
            <div class="flex items-stretch justify-center gap-3">
              <FlashCard
                v-for="(card, index) in words.slice(3)"
                :key="`${card.word}-${index + 3}-${bloomSeed}`"
                :card="card"
                :index="index + 3"
              />
            </div>
          </div>
        </div>
      </div>

      <button
        type="button"
        class="mt-14 rounded-full border border-purple-500/50 bg-slate-800 px-8 py-3 text-sm font-semibold tracking-wide text-white shadow-[0_0_15px_rgba(168,85,247,0.3)] transition-all duration-500 hover:scale-105 hover:border-pink-500 hover:shadow-[0_0_25px_rgba(236,72,153,0.5)] focus:outline-none"
        :class="
          !isLoading && words.length > 0 && !isChallengeMode
            ? 'opacity-100 scale-100'
            : 'pointer-events-none scale-95 opacity-0'
        "
        @click="fetchWords"
      >
        ✨ Florescer Novas Palavras
      </button>
    </section>
  </main>
</template>
