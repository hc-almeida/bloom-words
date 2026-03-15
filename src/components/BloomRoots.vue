<template>
  <div class="pointer-events-none absolute inset-0 z-0 flex items-end justify-center overflow-hidden opacity-60 mix-blend-screen sm:opacity-100">
    <svg
      viewBox="0 0 1000 600"
      class="h-[120%] w-full max-w-6xl origin-bottom animate-pulse-slow"
      preserveAspectRatio="xMidYMax slice"
      xmlns="http://www.w3.org/2000/svg"
    >
      <defs>
        <linearGradient id="neonGradient" x1="0%" y1="100%" x2="100%" y2="0%">
          <stop offset="0%" stop-color="#a855f7" /> <stop offset="100%" stop-color="#ec4899" /> </linearGradient>

        <filter id="glow" x="-50%" y="-50%" width="200%" height="200%">
          <feGaussianBlur stdDeviation="10" result="blur" />
          <feComposite in="SourceGraphic" in2="blur" operator="over" />
        </filter>
      </defs>

      <g filter="url(#glow)" fill="none" stroke="url(#neonGradient)" stroke-linecap="round">
        
        <path d="M 500 580 C 350 500, 150 450, 50 200" stroke-width="4" opacity="0.6" />
        <path d="M 500 580 C 450 450, 300 350, 250 120" stroke-width="7" opacity="0.8" />
        <path d="M 500 580 C 500 400, 480 250, 500 80" stroke-width="9" opacity="0.9" />
        <path d="M 500 580 C 550 450, 700 350, 750 120" stroke-width="7" opacity="0.8" />
        <path d="M 500 580 C 650 500, 850 450, 950 200" stroke-width="4" opacity="0.6" />

        <path d="M 450 520 Q 380 480, 350 380" stroke-width="3" opacity="0.5" />
        <path d="M 550 520 Q 620 480, 650 380" stroke-width="3" opacity="0.5" />
        <path d="M 470 420 C 420 380, 400 300, 380 220" stroke-width="4" opacity="0.7" />
        <path d="M 530 420 C 580 380, 600 300, 620 220" stroke-width="4" opacity="0.7" />
        <path d="M 420 300 Q 350 250, 300 150" stroke-width="2" opacity="0.4" />
        <path d="M 580 300 Q 650 250, 700 150" stroke-width="2" opacity="0.4" />

        <circle cx="500" cy="580" r="12" fill="#f0abfc" stroke="none" />
        <circle cx="500" cy="580" r="22" fill="none" stroke="#f0abfc" stroke-width="3" opacity="0.8" />
      </g>
    </svg>
  </div>
</template>

<style scoped>
.animate-pulse-slow {
  animation: pulse-slow 4s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}
@keyframes pulse-slow {
  0%, 100% { opacity: 1; transform: scale(1); }
  50% { opacity: 0.85; transform: scale(0.98); }
}
</style>
