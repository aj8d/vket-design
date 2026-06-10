<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue';

const blurPx = ref(0);
const scrollIndicatorOpacity = ref(0.45);
let rafId = 0;

const updateSceneState = () => {
  const viewport = window.innerHeight || 1;
  const progress = Math.min(window.scrollY / viewport, 1);
  blurPx.value = progress * 14;
  scrollIndicatorOpacity.value = 1 - progress * 0.9;
};

const onScroll = () => {
  if (rafId) {
    cancelAnimationFrame(rafId);
  }

  rafId = requestAnimationFrame(() => {
    updateSceneState();
    rafId = 0;
  });
};

onMounted(() => {
  updateSceneState();
  window.addEventListener('scroll', onScroll, { passive: true });
  window.addEventListener('resize', updateSceneState);
});

onBeforeUnmount(() => {
  if (rafId) {
    cancelAnimationFrame(rafId);
  }

  window.removeEventListener('scroll', onScroll);
  window.removeEventListener('resize', updateSceneState);
});
</script>

<template>
  <main class="landing-stack">
    <section class="first-view">
      <div class="first-surface" :style="{ '--scroll-blur': blurPx + 'px' }" />
      <div class="scroll-indicator" :style="{ opacity: scrollIndicatorOpacity }">
        <span class="scroll-text">scroll</span>
        <div class="line-outer">
          <div class="line-inner"></div>
        </div>
      </div>
    </section>

    <section class="overlay-view" />
  </main>
</template>

<style scoped>
.landing-stack {
  position: relative;
  min-height: 220vh;
}

.first-view {
  position: sticky;
  top: 0;
  z-index: 1;
  height: 100dvh;
  overflow: hidden;
  background: #000;
}

.scroll-indicator {
  position: absolute;
  bottom: 40px;
  left: 50%;
  z-index: 2;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  transform: translateX(-50%);
  pointer-events: none;
  transition: opacity 0.12s linear;
}

.scroll-text {
  font-size: 10px;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.45);
}

.line-outer {
  position: relative;
  width: 1px;
  height: 48px;
  overflow: hidden;
  background: rgba(255, 255, 255, 0.15);
}

.line-inner {
  position: absolute;
  top: -50%;
  left: 0;
  width: 100%;
  height: 50%;
  background: #fff;
  animation: lineRun 1.8s cubic-bezier(0.76, 0, 0.24, 1) infinite;
}

.first-surface {
  position: relative;
  width: 100%;
  height: 100%;
  overflow: hidden;
  --hero-image: url('/38C3A83F-CB1E-4632-A527-0385D752D1F3.png');
  --scroll-blur: 0px;
}

.first-surface::before {
  content: '';
  position: absolute;
  inset: -8%;
  background-image: var(--hero-image);
  background-size: cover;
  background-position: center center;
  background-repeat: no-repeat;
  transform: scale(1.12);
  filter: blur(16px);
  opacity: 0.95;
}

.first-surface::after {
  content: '';
  position: absolute;
  inset: 0;
  background-image: var(--hero-image);
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  filter: blur(var(--scroll-blur));
  will-change: filter;
}

.overlay-view {
  position: relative;
  z-index: 2;
  min-height: 120vh;
  background: #0b1e4f;
  opacity: 90%;
  border-radius: 36px 36px 0 0;
}

@keyframes lineRun {
  0% {
    top: -50%;
  }

  100% {
    top: 100%;
  }
}
</style>
