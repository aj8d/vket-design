<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue';

const blurPx = ref(0);
let rafId = 0;

const updateSceneState = () => {
  const viewport = window.innerHeight || 1;
  const progress = Math.min(window.scrollY / viewport, 1);
  blurPx.value = progress * 14;
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
</style>
