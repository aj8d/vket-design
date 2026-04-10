<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'
import { gsap } from 'gsap'
import { CalendarDays, ExternalLink, MapPin, Ticket } from 'lucide-vue-next';
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Autoplay } from 'swiper/modules'

const orbitRing = ref<HTMLElement | null>(null)
const orbitItems = ref<HTMLElement[]>([])
let orbitTimeline: gsap.core.Timeline | null = null

onMounted(() => {
  if (!orbitRing.value || orbitItems.value.length === 0) {
    return
  }

  orbitTimeline = gsap.timeline({ repeat: -1, defaults: { ease: 'none' } })
  orbitTimeline
    .to(orbitRing.value, { rotate: 360, duration: 16 }, 0)
    .to(orbitItems.value, { rotate: -360, duration: 16 }, 0)
})

onBeforeUnmount(() => {
  orbitTimeline?.kill()
})

const cards = [
  {
    key: 'date',
    icon: CalendarDays,
    iconClass: 'bg-[#ffb300]/20 text-[#ffb300]',
    title: '開催日',
    value: '2026年 秋',
    note: '詳細は続報をお待ちください',
    noteClass: 'text-[#00d67a]',
    tint: 'from-[#2a1800]/78 via-[#1d1202]/74 to-[#0f0b07]/78',
    glow: 'shadow-[0_0_28px_rgba(255,171,0,0.20)]',
  },
  {
    key: 'place',
    icon: MapPin,
    iconClass: 'bg-[#00c8e7]/20 text-[#00c8e7]',
    title: '会場',
    value: '札幌市内',
    note: '会場調整中',
    noteClass: 'text-[#00d67a]',
    tint: 'from-[#002637]/78 via-[#001c2d]/74 to-[#001521]/78',
    glow: 'shadow-[0_0_28px_rgba(0,200,231,0.20)]',
  },
  {
    key: 'ticket',
    icon: Ticket,
    iconClass: 'bg-[#ff007a]/20 text-[#ff007a]',
    title: 'チケット',
    value: '準備中',
    note: '詳細は続報をお待ちください',
    noteClass: 'text-[#00d67a]',
    tint: 'from-[#320022]/78 via-[#26001a]/74 to-[#180012]/78',
    glow: 'shadow-[0_0_28px_rgba(255,0,122,0.22)]',
  },
];
</script>

<template>
  <main class="relative min-h-screen overflow-hidden bg-[#0f172a] text-white">
    <div
      class="absolute inset-0 bg-[radial-gradient(circle_at_top,rgba(125,211,252,0.24),transparent_34%),radial-gradient(circle_at_80%_20%,rgba(168,85,247,0.22),transparent_26%),linear-gradient(180deg,#0f172a_0%,#111827_48%,#0b1020_100%)]"
    />
    <div
      class="absolute inset-x-0 top-0 h-64 bg-[radial-gradient(circle_at_center,rgba(255,255,255,0.18),transparent_58%)] opacity-60"
    />

    <div class="relative mx-auto w-full max-w-sm px-4 pb-14 pt-28">
      <section>
        <h2 class="text-center text-[45px] font-semibold leading-[1.08] tracking-[-0.02em] text-[#ff4d00]">
          リアルとバーチャルの
          <br />
          境界を、
        </h2>
        <h1
          class="mb-8 mt-3 text-center text-[68px] font-semibold leading-[0.95] tracking-[-0.04em] text-white drop-shadow-[0_0_16px_#FFA500]"
        >
          開拓せよ。
        </h1>
      </section>

      <section class="space-y-4">
        <div
          v-for="card in cards"
          :key="card.key"
          class="liquid-card rounded-[22px] border border-white/35 px-5 py-5 backdrop-blur-2xl"
          :class="[card.glow, `bg-gradient-to-br ${card.tint}`]"
        >
          <div :class="['mx-auto flex h-11 w-11 items-center justify-center rounded-full', card.iconClass]">
            <component :is="card.icon" :size="22" :stroke-width="2.2" />
          </div>
          <p class="mt-3 text-center text-[20px] font-medium leading-none text-white/92">{{ card.title }}</p>
          <p class="mt-2 text-center text-[36px] font-semibold leading-none text-white">{{ card.value }}</p>
          <p class="mt-2 text-center text-[16px] leading-none" :class="card.noteClass">{{ card.note }}</p>
        </div>
      </section>

      <div class="mt-10 flex justify-center">
        <button
          type="button"
          class="inline-flex items-center gap-2 rounded-full bg-[#ff8a00] px-6 py-3 text-[15px] font-semibold text-[#1f1403] shadow-[0_10px_26px_rgba(255,138,0,0.35)] transition hover:brightness-105"
        >
          イベント詳細を見る
          <ExternalLink :size="15" :stroke-width="2.2" />
        </button>
      </div>

      <section class="mt-14">
        <p class="mb-3 text-center text-xs uppercase tracking-[0.24em] text-white/65">Swiper Rotation</p>
        <Swiper
          :modules="[Autoplay]"
          :space-between="14"
          :slides-per-view="1.08"
          :centered-slides="true"
          :loop="true"
          :speed="900"
          :autoplay="{ delay: 1800, disableOnInteraction: false }"
          class="!overflow-visible"
        >
          <SwiperSlide v-for="card in cards" :key="`swiper-${card.key}`">
            <article class="liquid-card rounded-[20px] border border-white/35 px-4 py-4 backdrop-blur-2xl" :class="[card.glow, `bg-gradient-to-br ${card.tint}`]">
              <div :class="['mx-auto flex h-10 w-10 items-center justify-center rounded-full', card.iconClass]">
                <component :is="card.icon" :size="20" :stroke-width="2.2" />
              </div>
              <p class="mt-2 text-center text-[18px] font-medium text-white/92">{{ card.title }}</p>
              <p class="mt-1 text-center text-[30px] font-semibold leading-none text-white">{{ card.value }}</p>
              <p class="mt-2 text-center text-sm" :class="card.noteClass">{{ card.note }}</p>
            </article>
          </SwiperSlide>
        </Swiper>
      </section>

      <section class="mt-16">
        <p class="mb-5 text-center text-xs uppercase tracking-[0.24em] text-white/65">GSAP Rotation</p>
        <div class="orbit-wrapper">
          <div ref="orbitRing" class="orbit-ring">
            <div
              v-for="(card, index) in cards"
              :key="`gsap-${card.key}`"
              ref="orbitItems"
              class="orbit-item"
              :style="{ '--angle': `${index * 120}deg` }"
            >
              <article class="liquid-card orbit-card rounded-[18px] border border-white/35 px-4 py-4 backdrop-blur-2xl" :class="[card.glow, `bg-gradient-to-br ${card.tint}`]">
                <div :class="['mx-auto flex h-9 w-9 items-center justify-center rounded-full', card.iconClass]">
                  <component :is="card.icon" :size="18" :stroke-width="2.2" />
                </div>
                <p class="mt-2 text-center text-[15px] font-medium text-white/92">{{ card.title }}</p>
                <p class="mt-1 text-center text-[22px] font-semibold leading-none text-white">{{ card.value }}</p>
              </article>
            </div>
          </div>
        </div>
      </section>
    </div>
  </main>
</template>

<style scoped>
.liquid-card {
  position: relative;
  overflow: hidden;
}

.liquid-card::before {
  content: '';
  position: absolute;
  inset: 0;
  pointer-events: none;
  background:
    radial-gradient(circle at 18% -12%, rgba(255, 255, 255, 0.36), rgba(255, 255, 255, 0) 40%),
    radial-gradient(circle at 82% 120%, rgba(255, 255, 255, 0.16), rgba(255, 255, 255, 0) 46%);
}

.liquid-card::after {
  content: '';
  position: absolute;
  inset: 0;
  pointer-events: none;
  border-radius: inherit;
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.45),
    inset 0 -12px 20px rgba(0, 0, 0, 0.26);
}

.orbit-wrapper {
  position: relative;
  height: 320px;
}

.orbit-ring {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 100%;
  height: 100%;
  transform: translate(-50%, -50%);
  transform-origin: center;
}

.orbit-item {
  position: absolute;
  left: 50%;
  top: 50%;
  transform:
    translate(-50%, -50%)
    rotate(var(--angle))
    translateY(-118px)
    rotate(calc(var(--angle) * -1));
  transform-origin: center;
}

.orbit-card {
  width: 154px;
}
</style>
