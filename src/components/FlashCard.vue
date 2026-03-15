<script setup>
import { computed, onMounted, ref } from 'vue'

const props = defineProps({
  card: { type: Object, required: true },
  index: { type: Number, default: 0 },
})

const isVisible = ref(false)

// Dá um delay em cascata para cada card nascer
const transitionDelay = computed(() => `${props.index * 150}ms`)

const speakWord = () => {
  if (!window?.speechSynthesis || !props.card?.word) return
  window.speechSynthesis.cancel()
  const utterance = new SpeechSynthesisUtterance(props.card.word)
  utterance.lang = 'en-US'
  utterance.rate = 0.85
  window.speechSynthesis.speak(utterance)
}

onMounted(() => {
  requestAnimationFrame(() => {
    isVisible.value = true
  })
})
</script>

<template>
  <article
    class="group flex h-[21.5rem] w-full max-w-[18.75rem] flex-col rounded-2xl border border-white/10 bg-slate-800/60 p-4 backdrop-blur-md shadow-[0_0_20px_rgba(168,85,247,0.18)] transition-all duration-700 ease-out hover:-translate-y-2 hover:border-pink-500/50 hover:shadow-[0_0_30px_rgba(236,72,153,0.3)]"
    :class="isVisible ? 'scale-100 opacity-100' : 'scale-0 opacity-0'"
    :style="{ transitionDelay }"
  >
    <header class="grid grid-cols-[minmax(0,1fr)_auto] items-start gap-3">
      <div class="min-w-0">
        <p class="mb-1 text-xs font-medium uppercase tracking-wider text-slate-400">Word:</p>
        <h2
          class="break-normal bg-gradient-to-r from-purple-400 to-pink-400 bg-clip-text text-[1.75rem] font-extrabold leading-tight text-transparent"
        >
          {{ card.word }}
        </h2>
      </div>

      <button
        type="button"
        class="shrink-0 text-2xl opacity-70 transition-all hover:scale-110 hover:opacity-100 focus:outline-none"
        aria-label="Ouvir pronúncia"
        @click="speakWord"
      >
        🔊
      </button>
    </header>

    <div class="my-3 flex flex-grow flex-col justify-center">
      <p class="description-clamp text-[0.98rem] leading-relaxed text-slate-300">
        {{ card.description }}
      </p>
    </div>

    <div class="mb-4 h-px w-full bg-gradient-to-r from-transparent via-purple-400/40 to-transparent" />

    <p class="text-[0.95rem] italic leading-relaxed text-pink-200/80">
      "{{ card.useCase }}"
    </p>
  </article>
</template>

<style scoped>
.description-clamp {
  display: -webkit-box;
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>
