<script setup lang="ts">
import { computed } from 'vue'
import type { CSSProperties } from 'vue'

type BackgroundVariant = 'particles' | 'bubble' | 'circuits'

interface Props {
  variant?: BackgroundVariant
  speed?: number
}

type AnimatedStyle = CSSProperties & {
  '--animation-duration'?: string
  '--animation-delay'?: string
  '--twinkle-duration'?: string
  '--twinkle-delay'?: string
  '--drift-x'?: string
  '--drift-y'?: string
  '--bubble-size'?: string
  '--particle-opacity'?: number
  '--streak-duration'?: string
  '--streak-delay'?: string
  '--streak-angle'?: string
  '--streak-length'?: string
}

const props = withDefaults(defineProps<Props>(), {
  variant: 'particles',
  speed: 1
})

const safeSpeed = computed(() => {
  const value = Number(props.speed)

  return Number.isFinite(value) && value > 0 ? Math.min(value, 8) : 1
})

/**
 * Génération pseudo-aléatoire déterministe :
 * le rendu reste stable entre le serveur et le navigateur.
 */
function random(seed: number, offset = 0): number {
  const value = Math.sin(seed * 127.1 + offset * 311.7) * 43758.5453

  return value - Math.floor(value)
}

/**
 * Particules avec positions, tailles, trajectoires
 * et rythmes d'animation variés.
 */
const particles = Array.from({ length: 64 }, (_, index) => {
  const seed = index + 1

  return {
    id: index,
    left: `${(random(seed, 1) * 100).toFixed(2)}%`,
    top: `${(random(seed, 2) * 100).toFixed(2)}%`,
    size: `${(1.5 + random(seed, 3) * 3.5).toFixed(1)}px`,
    opacity: Number((0.4 + random(seed, 4) * 0.6).toFixed(2)),
    delay: -(random(seed, 5) * 32),
    duration: 20 + random(seed, 6) * 26,
    twinkleDuration: 3 + random(seed, 7) * 6,
    twinkleDelay: -(random(seed, 8) * 8),
    driftX: `${((random(seed, 9) - 0.5) * 100).toFixed(1)}px`,
    driftY: `${((random(seed, 10) - 0.5) * 120).toFixed(1)}px`,
    tone: random(seed, 11) > 0.55 ? 'cyan' : 'primary'
  }
})

/**
 * Traînées lumineuses discrètes qui traversent la scène.
 */
const streaks = Array.from({ length: 5 }, (_, index) => ({
  id: index,
  left: `${(index * 23 + 13) % 100}%`,
  top: `${(index * 31 + 12) % 85}%`,
  delay: -(index * 4 + 2),
  duration: 18 + index * 3,
  angle: `${-35 + (index % 3) * 12}deg`,
  length: `${80 + (index % 3) * 40}px`
}))

const bubbles = [
  {
    id: 0,
    left: '8%',
    top: '12%',
    size: '18rem',
    delay: -8,
    driftX: '-24px',
    driftY: '18px'
  },
  {
    id: 1,
    left: '68%',
    top: '4%',
    size: '26rem',
    delay: -16,
    driftX: '30px',
    driftY: '-20px'
  },
  {
    id: 2,
    left: '35%',
    top: '58%',
    size: '22rem',
    delay: -3,
    driftX: '-18px',
    driftY: '28px'
  },
  {
    id: 3,
    left: '84%',
    top: '68%',
    size: '15rem',
    delay: -12,
    driftX: '20px',
    driftY: '-16px'
  }
]

function animationStyle(
  delay: number,
  duration: number,
  driftX: string,
  driftY: string,
  opacity?: number,
  size?: string,
  twinkleDuration = 6,
  twinkleDelay = 0
): AnimatedStyle {
  return {
    '--animation-duration': `${(duration / safeSpeed.value).toFixed(3)}s`,
    '--animation-delay': `${delay.toFixed(3)}s`,
    '--twinkle-duration': `${(twinkleDuration / safeSpeed.value).toFixed(3)}s`,
    '--twinkle-delay': `${twinkleDelay.toFixed(3)}s`,
    '--drift-x': driftX,
    '--drift-y': driftY,
    ...(opacity !== undefined ? { '--particle-opacity': opacity } : {}),
    ...(size ? { '--bubble-size': size } : {})
  }
}

function streakStyle(
  duration: number,
  delay: number,
  angle: string,
  length: string
): AnimatedStyle {
  return {
    '--streak-duration': `${(duration / safeSpeed.value).toFixed(3)}s`,
    '--streak-delay': `${delay.toFixed(3)}s`,
    '--streak-angle': angle,
    '--streak-length': length
  }
}
</script>

<template>
  <div
    aria-hidden="true"
    class="pointer-events-none absolute inset-0 -z-10 overflow-hidden bg-default"
  >
    <!-- Lumière d'ambiance -->
    <div class="background-aurora" />

    <!-- Halos diffus -->
    <div class="background-orb background-orb--one" />
    <div class="background-orb background-orb--two" />
    <div class="background-orb background-orb--three" />

    <!-- Grille subtile -->
    <div class="background-grid" />

    <!-- Voile de profondeur -->
    <div class="background-vignette" />

    <!-- Variante particules -->
    <template v-if="variant === 'particles'">
      <span
        v-for="particle in particles"
        :key="particle.id"
        class="background-particle absolute rounded-full"
        :class="
          particle.tone === 'cyan'
            ? 'background-particle--cyan'
            : 'background-particle--primary'
        "
        :style="{
          ...animationStyle(
            particle.delay,
            particle.duration,
            particle.driftX,
            particle.driftY,
            particle.opacity,
            undefined,
            particle.twinkleDuration,
            particle.twinkleDelay
          ),
          left: particle.left,
          top: particle.top,
          width: particle.size,
          height: particle.size
        }"
      />

      <!-- Traînées lumineuses -->
      <span
        v-for="streak in streaks"
        :key="`streak-${streak.id}`"
        class="background-streak absolute"
        :style="{
          ...streakStyle(
            streak.duration,
            streak.delay,
            streak.angle,
            streak.length
          ),
          left: streak.left,
          top: streak.top
        }"
      />
    </template>

    <!-- Variante bulles -->
    <template v-else-if="variant === 'bubble'">
      <span
        v-for="bubble in bubbles"
        :key="bubble.id"
        class="background-bubble absolute rounded-full"
        :style="{
          ...animationStyle(
            bubble.delay,
            28,
            bubble.driftX,
            bubble.driftY,
            undefined,
            bubble.size
          ),
          left: bubble.left,
          top: bubble.top
        }"
      />
    </template>

    <!-- Variante circuits -->
    <svg
      v-else
      class="absolute inset-0 h-full w-full text-primary"
      viewBox="0 0 1200 800"
      preserveAspectRatio="xMidYMid slice"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
    >
      <g
        stroke="currentColor"
        stroke-width="1"
        stroke-opacity=".18"
        stroke-linecap="round"
        stroke-linejoin="round"
      >
        <path
          d="M-40 100 H100 L150 150 H280 L330 100 H440 L480 140 H610 L660 90 H810"
        />
        <path d="M1240 160 H1080 L1030 210 H920 L870 160 H760 L710 210 H590" />
        <path d="M-30 430 H120 L170 380 H300 L350 430 H470 L520 380 H650" />
        <path d="M1230 500 H1080 L1030 450 H900 L850 500 H730 L680 450 H560" />
        <path d="M180 -30 V100 L230 150 V270 L180 320 V460 L230 510 V650" />
        <path d="M920 -30 V90 L870 140 V260 L920 310 V440 L870 490 V650" />
        <path d="M430 830 V700 L480 650 V570 L430 520 V400" />
        <path d="M750 830 V690 L700 640 V560 L750 510 V390" />
      </g>

      <g
        class="circuit-flow"
        stroke="currentColor"
        stroke-width="1.5"
        stroke-linecap="round"
        stroke-linejoin="round"
      >
        <path
          d="M-40 100 H100 L150 150 H280 L330 100 H440 L480 140 H610 L660 90 H810"
        />
        <path d="M1240 160 H1080 L1030 210 H920 L870 160 H760 L710 210 H590" />
        <path d="M-30 430 H120 L170 380 H300 L350 430 H470 L520 380 H650" />
        <path d="M1230 500 H1080 L1030 450 H900 L850 500 H730 L680 450 H560" />
        <path d="M180 -30 V100 L230 150 V270 L180 320 V460 L230 510 V650" />
        <path d="M920 -30 V90 L870 140 V260 L920 310 V440 L870 490 V650" />
        <path d="M430 830 V700 L480 650 V570 L430 520 V400" />
        <path d="M750 830 V690 L700 640 V560 L750 510 V390" />
      </g>

      <g
        class="circuit-nodes"
        fill="currentColor"
      >
        <circle
          cx="150"
          cy="150"
          r="2.5"
        />
        <circle
          cx="330"
          cy="100"
          r="2.5"
        />
        <circle
          cx="480"
          cy="140"
          r="2.5"
        />
        <circle
          cx="1030"
          cy="210"
          r="2.5"
        />
        <circle
          cx="870"
          cy="160"
          r="2.5"
        />
        <circle
          cx="170"
          cy="380"
          r="2.5"
        />
        <circle
          cx="520"
          cy="380"
          r="2.5"
        />
        <circle
          cx="850"
          cy="500"
          r="2.5"
        />
        <circle
          cx="230"
          cy="150"
          r="2.5"
        />
        <circle
          cx="870"
          cy="490"
          r="2.5"
        />
        <circle
          cx="480"
          cy="650"
          r="2.5"
        />
        <circle
          cx="700"
          cy="640"
          r="2.5"
        />
      </g>
    </svg>
  </div>
</template>

<style scoped>
.background-aurora {
  position: absolute;
  inset: 0;
  background:
    radial-gradient(
      ellipse at 18% 8%,
      rgba(99, 102, 241, 0.17),
      transparent 42%
    ),
    radial-gradient(
      ellipse at 85% 65%,
      rgba(14, 165, 233, 0.13),
      transparent 44%
    ),
    radial-gradient(
      ellipse at 45% 100%,
      rgba(139, 92, 246, 0.09),
      transparent 40%
    );
}

.background-orb {
  position: absolute;
  width: 28rem;
  height: 28rem;
  border-radius: 9999px;
  filter: blur(70px);
  opacity: 0.15;
  animation: orb-float 26s ease-in-out infinite alternate;
  will-change: transform;
}

.background-orb--one {
  top: -13rem;
  left: -9rem;
  background: #6366f1;
}

.background-orb--two {
  top: 25%;
  right: -15rem;
  background: #06b6d4;
  animation-delay: -9s;
  animation-duration: 32s;
}

.background-orb--three {
  bottom: -20rem;
  left: 32%;
  background: #8b5cf6;
  animation-delay: -17s;
  animation-duration: 35s;
}

.background-grid {
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(to right, rgba(148, 163, 184, 0.08) 1px, transparent 1px),
    linear-gradient(to bottom, rgba(148, 163, 184, 0.08) 1px, transparent 1px);
  background-size: 42px 42px;
  -webkit-mask-image: radial-gradient(
    ellipse at center,
    black 15%,
    transparent 78%
  );
  mask-image: radial-gradient(ellipse at center, black 15%, transparent 78%);
}

.background-vignette {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    transparent 45%,
    color-mix(in srgb, var(--ui-bg) 35%, transparent) 100%
  );
}

.background-particle {
  z-index: 1;
  opacity: var(--particle-opacity, 0.6);
  animation:
    particle-drift var(--animation-duration, 24s) ease-in-out
      var(--animation-delay, 0s) infinite alternate,
    particle-twinkle var(--twinkle-duration, 5s) ease-in-out
      var(--twinkle-delay, 0s) infinite;
  will-change: translate, opacity, transform;
}

.background-particle--primary {
  color: var(--ui-primary);
  background: currentColor;
  box-shadow:
    0 0 8px currentColor,
    0 0 20px color-mix(in srgb, currentColor 45%, transparent);
}

.background-particle--cyan {
  color: #67e8f9;
  background: currentColor;
  box-shadow:
    0 0 8px currentColor,
    0 0 20px rgba(34, 211, 238, 0.35);
}

.background-streak {
  z-index: 1;
  width: var(--streak-length, 100px);
  height: 1px;
  border-radius: 9999px;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(103, 232, 249, 0.9),
    rgba(129, 140, 248, 0.45),
    transparent
  );
  transform-origin: center;
  rotate: var(--streak-angle, -30deg);
  opacity: 0;
  animation: streak-sweep var(--streak-duration, 20s) linear
    var(--streak-delay, 0s) infinite;
  box-shadow: 0 0 8px rgba(103, 232, 249, 0.35);
  will-change: translate, opacity;
}

.background-bubble {
  width: var(--bubble-size, 20rem);
  height: var(--bubble-size, 20rem);
  border: 1px solid color-mix(in srgb, var(--ui-primary) 16%, transparent);
  background: radial-gradient(
    circle at 35% 35%,
    color-mix(in srgb, var(--ui-primary) 15%, transparent),
    transparent 70%
  );
  box-shadow: inset 0 0 60px
    color-mix(in srgb, var(--ui-primary) 12%, transparent);
  filter: blur(30px);
  opacity: 0.7;
  animation: particle-drift var(--animation-duration, 28s) ease-in-out
    var(--animation-delay, 0s) infinite alternate;
  will-change: translate;
}

.circuit-flow {
  stroke-dasharray: 3 18;
  animation: circuit-travel 5s linear infinite;
  filter: drop-shadow(0 0 3px currentColor);
  opacity: 0.65;
}

.circuit-nodes {
  opacity: 0.65;
  animation: circuit-pulse 3s ease-in-out infinite alternate;
}

@keyframes particle-drift {
  from {
    translate: 0 0;
  }

  to {
    translate: var(--drift-x, 20px) var(--drift-y, -20px);
  }
}

@keyframes particle-twinkle {
  0%,
  100% {
    opacity: 0.15;
    transform: scale(0.7);
  }

  50% {
    opacity: var(--particle-opacity, 0.8);
    transform: scale(1.25);
  }
}

@keyframes orb-float {
  from {
    translate: -20px -10px;
    scale: 0.95;
  }

  to {
    translate: 40px 30px;
    scale: 1.1;
  }
}

@keyframes streak-sweep {
  0% {
    opacity: 0;
    translate: -100px 70px;
  }

  8% {
    opacity: 0.85;
  }

  35% {
    opacity: 0;
    translate: 220px -150px;
  }

  100% {
    opacity: 0;
    translate: 220px -150px;
  }
}

@keyframes circuit-travel {
  to {
    stroke-dashoffset: -84;
  }
}

@keyframes circuit-pulse {
  from {
    opacity: 0.25;
  }

  to {
    opacity: 0.95;
  }
}

@media (prefers-reduced-motion: reduce) {
  .background-particle,
  .background-bubble,
  .background-streak,
  .background-orb,
  .circuit-flow,
  .circuit-nodes {
    animation: none;
    will-change: auto;
  }

  .background-particle,
  .background-streak {
    opacity: 0.65;
  }
}
</style>
