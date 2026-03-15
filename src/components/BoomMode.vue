<script setup>
import { computed, onBeforeUnmount, onMounted, ref, watch } from 'vue'

const props = defineProps({
  words: {
    type: Array,
    default: () => [],
  },
})

const emit = defineEmits(['onRestart', 'onNextStage'])

const timeLeft = ref(15.0)
const score = ref(0)
const pendingWords = ref([])
const targetWord = ref(null)
const isGameOver = ref(false)
const isVictory = ref(false)
const clickedWord = ref('')
const clickedState = ref('')
let timerId = null

const timeText = computed(() => timeLeft.value.toFixed(2))
const levelText = computed(() => `LEVEL: ${6 - pendingWords.value.length} - ${targetWord.value?.word || '...'}`)

const stopTimer = () => {
  if (timerId) {
    clearInterval(timerId)
    timerId = null
  }
}

const pickNextTarget = () => {
  if (!pendingWords.value.length) {
    targetWord.value = null
    return
  }

  const index = Math.floor(Math.random() * pendingWords.value.length)
  targetWord.value = pendingWords.value[index]
}

const startTimer = () => {
  stopTimer()
  timerId = setInterval(() => {
    if (isGameOver.value || isVictory.value) return

    const next = Number((timeLeft.value - 0.01).toFixed(2))
    timeLeft.value = next > 0 ? next : 0

    if (timeLeft.value <= 0) {
      stopTimer()
      isGameOver.value = true
    }
  }, 10)
}

const initializeGame = () => {
  stopTimer()
  timeLeft.value = 15.0
  score.value = 0
  isGameOver.value = false
  isVictory.value = false
  clickedWord.value = ''
  clickedState.value = ''
  pendingWords.value = Array.isArray(props.words) ? [...props.words] : []
  pickNextTarget()

  if (pendingWords.value.length) {
    startTimer()
  }
}

const handleWordClick = (wordItem) => {
  if (isGameOver.value || isVictory.value || !targetWord.value) return

  const isCorrect = wordItem.word === targetWord.value.word
  clickedWord.value = wordItem.word

  if (!isCorrect) {
    clickedState.value = 'wrong'
    stopTimer()
    isGameOver.value = true
    return
  }

  clickedState.value = 'correct'
  stopTimer()

  setTimeout(() => {
    score.value += 25
    timeLeft.value = Number((timeLeft.value + 3).toFixed(2))
    pendingWords.value = pendingWords.value.filter((item) => item.word !== wordItem.word)
    clickedWord.value = ''
    clickedState.value = ''

    if (!pendingWords.value.length) {
      isVictory.value = true
      stopTimer()
      return
    }

    pickNextTarget()
    startTimer()
  }, 1000)
}

watch(
  () => props.words,
  (words) => {
    if (!Array.isArray(words) || !words.length) return
    initializeGame()
  },
  { immediate: true },
)

onMounted(() => {
  if (props.words?.length) initializeGame()
})

onBeforeUnmount(() => {
  stopTimer()
})
</script>

<template>
  <section
    class="relative min-h-screen w-full overflow-x-hidden rounded-3xl border border-red-500/30 bg-[radial-gradient(ellipse_at_center,_var(--tw-gradient-stops))] from-red-950 via-slate-900 to-black px-4 py-8"
    :class="
      clickedWord && targetWord && clickedWord === targetWord.word
        ? 'cursor-mira-green'
        : 'cursor-mira-red'
    "
  >
    <div class="pointer-events-none absolute -left-20 top-24 h-64 w-64 rounded-full bg-red-600/30 blur-2xl sm:h-72 sm:w-72" />
    <div class="pointer-events-none absolute -right-8 top-24 h-64 w-64 rounded-full bg-pink-600/25 blur-2xl sm:h-80 sm:w-80" />
    <div class="pointer-events-none absolute bottom-10 left-1/2 h-36 w-72 -translate-x-1/2 rounded-full bg-rose-500/20 blur-2xl sm:h-44 sm:w-80" />

    <div class="relative mx-auto flex min-h-screen w-full max-w-6xl flex-col items-center justify-center">
      <div class="rounded-2xl border-4 border-neutral-800 bg-neutral-950 px-4 py-3 sm:border-8 sm:px-8 sm:py-4 md:px-10 md:py-5">
        <p class="font-mono text-5xl font-bold text-red-500 drop-shadow-[0_0_25px_rgba(239,68,68,1)] sm:text-7xl md:text-8xl">
          {{ timeText }}
        </p>
      </div>

      <div
        class="relative z-10 mt-6 w-full max-w-lg rounded-2xl border-4 border-slate-600 bg-gradient-to-b from-slate-400 to-slate-500 px-5 py-5 shadow-inner sm:px-8 sm:py-6"
      >
        <div class="absolute left-3 top-3 h-3 w-3 rounded-full bg-slate-700 shadow-inner" />
        <div class="absolute right-3 top-3 h-3 w-3 rounded-full bg-slate-700 shadow-inner" />
        <div class="absolute bottom-3 left-3 h-3 w-3 rounded-full bg-slate-700 shadow-inner" />
        <div class="absolute bottom-3 right-3 h-3 w-3 rounded-full bg-slate-700 shadow-inner" />

        <p class="text-center text-base font-extrabold text-slate-900 sm:text-xl">
          DESARME A BOMBA!
        </p>
        <p class="mt-2 text-center text-sm font-extrabold text-slate-900 sm:text-base">
          Descrição: {{ targetWord?.description || '...' }}
        </p>
      </div>

      <div class="mt-8 flex w-full max-w-2xl flex-wrap justify-center gap-3">
        <button
          v-for="(wordItem, index) in words"
          :key="`${wordItem.word}-${index}`"
          type="button"
          class="w-full -skew-x-12 border-2 border-red-500 bg-red-800 px-3 py-2 text-center text-xl font-bold text-white shadow-[0_0_12px_rgba(239,68,68,0.55)] transition-all duration-300 hover:scale-105 sm:w-auto sm:px-5 sm:py-3 sm:text-3xl"
          :class="[
            clickedWord === wordItem.word && clickedState === 'correct'
              ? 'border-green-400 bg-green-600 drop-shadow-[0_0_20px_rgba(34,197,94,1)]'
              : '',
            clickedWord === wordItem.word && clickedState === 'wrong'
              ? 'border-red-300 bg-red-600 animate-pulse'
              : '',
          ]"
          @click="handleWordClick(wordItem)"
        >
          <span class="inline-block skew-x-12">{{ wordItem.word }}</span>
        </button>
      </div>

      <div class="mt-auto flex w-full max-w-2xl gap-4 pt-8">
        <div class="flex-1 rounded-xl border border-amber-300/40 bg-black/50 px-4 py-3 text-center text-lg font-bold text-amber-300 drop-shadow-[0_0_10px_rgba(252,211,77,0.7)] sm:text-2xl">
          SCORE: {{ score }}
        </div>
        <div class="flex-1 rounded-xl border border-red-300/40 bg-black/50 px-4 py-3 text-center text-sm font-bold text-red-300 sm:text-xl">
          {{ levelText }}
        </div>
      </div>
    </div>

    <div
      v-if="isGameOver"
      class="absolute inset-0 z-30 flex flex-col items-center justify-center gap-6 bg-red-950/90 text-center animate-pulse"
    >
      <p class="text-5xl font-black text-red-300">BOOM! GAME OVER</p>
      <button
        type="button"
        class="rounded-xl border border-red-300/70 bg-red-700 px-6 py-3 text-lg font-bold text-white"
        @click="emit('onRestart')"
      >
        Tentar Novamente
      </button>
    </div>

    <div
      v-if="isVictory"
      class="absolute inset-0 z-30 flex flex-col items-center justify-center gap-6 bg-gradient-to-br from-emerald-900/90 via-cyan-900/80 to-blue-900/90 text-center"
    >
      <p class="text-5xl font-black text-emerald-300 drop-shadow-[0_0_18px_rgba(52,211,153,0.95)]">
        BOMBA DESARMADA! 🏆
      </p>
      <button
        type="button"
        class="rounded-xl border border-emerald-300/70 bg-emerald-700 px-6 py-3 text-lg font-bold text-white"
        @click="emit('onNextStage')"
      >
        Próxima Fase
      </button>
    </div>
  </section>
</template>

<style scoped>
.cursor-mira-red, .cursor-mira-red * {
  cursor: url('data:image/svg+xml;utf8,<svg width="40" height="40" viewBox="0 0 40 40" xmlns="http://www.w3.org/2000/svg"><circle cx="20" cy="20" r="16" fill="none" stroke="%23EF4444" stroke-width="3"/><path d="M20 0v10M20 40v-10M0 20h10M40 20H30" stroke="%23EF4444" stroke-width="3"/><circle cx="20" cy="20" r="3" fill="%23EF4444"/></svg>') 20 20, crosshair !important;
}

.cursor-mira-green, .cursor-mira-green * {
  cursor: url('data:image/svg+xml;utf8,<svg width="40" height="40" viewBox="0 0 40 40" xmlns="http://www.w3.org/2000/svg"><circle cx="20" cy="20" r="16" fill="none" stroke="%2322C55E" stroke-width="3"/><path d="M20 0v10M20 40v-10M0 20h10M40 20H30" stroke="%2322C55E" stroke-width="3"/><circle cx="20" cy="20" r="3" fill="%2322C55E"/></svg>') 20 20, crosshair !important;
}
</style>
