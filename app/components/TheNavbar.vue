<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue';

const isVisible = ref(false);
const logoSrc = new URL('../../public/vketreal_in_sapporo_logo_dark.png', import.meta.url).href;
let rafId = 0;

const updateVisibility = () => {
  const overlayView = document.querySelector('.overlay-view');

  if (!overlayView) {
    isVisible.value = true;
    return;
  }

  const { top } = overlayView.getBoundingClientRect();
  isVisible.value = top <= 600;
};

const onScroll = () => {
  if (rafId) {
    cancelAnimationFrame(rafId);
  }

  rafId = requestAnimationFrame(() => {
    updateVisibility();
    rafId = 0;
  });
};

onMounted(() => {
  updateVisibility();
  window.addEventListener('scroll', onScroll, { passive: true });
  window.addEventListener('resize', updateVisibility);
});

onBeforeUnmount(() => {
  if (rafId) {
    cancelAnimationFrame(rafId);
  }

  window.removeEventListener('scroll', onScroll);
  window.removeEventListener('resize', updateVisibility);
});
</script>

<template>
  <header class="site-header" :class="{ 'is-visible': isVisible }">
    <NuxtLink to="/" class="brand">
      <img :src="logoSrc" alt="VKET REAL in SAPPORO" />
    </NuxtLink>
    <button type="button" class="nav-button" aria-label="Menu">…</button>
  </header>
</template>

<style scoped>
.site-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 30;
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
  color: #ffffff;
  mix-blend-mode: difference;
  opacity: 0;
  transform: translateY(-18px) scale(0.96);
  pointer-events: none;
  transition:
    opacity 0.35s ease,
    transform 0.35s cubic-bezier(0.2, 0.9, 0.2, 1.16);
}

.site-header.is-visible {
  opacity: 1;
  transform: translateY(0) scale(1);
  pointer-events: auto;
}

.brand {
  border: 1px solid currentColor;
  background: transparent;
  color: inherit;
  height: 38px;
  padding: 0 12px;
  border-radius: 999px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  text-decoration: none;
}

.brand img {
  display: block;
  height: 40px;
  width: auto;
  max-width: 180px;
  mix-blend-mode: normal;
}

.nav-button {
  border: 1px solid currentColor;
  background: transparent;
  color: inherit;
  height: 38px;
  padding: 0 14px;
  border-radius: 999px;
  font-size: 0.72rem;
  letter-spacing: 0.14em;
  cursor: pointer;
}
</style>
