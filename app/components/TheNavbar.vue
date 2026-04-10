<template>
  <nav class="fixed left-1/2 top-4 z-50 w-[calc(100vw-24px)] max-w-[22rem] -translate-x-1/2">
    <div
      class="flex w-full items-center justify-between rounded-full border border-white/20 bg-white/10 px-5 py-3 text-white shadow-[0_18px_40px_rgba(7,12,24,0.22)] backdrop-blur-xl"
    >
      <NuxtLink to="/" class="select-none text-lg font-semibold tracking-[0.32em] text-white"> vris </NuxtLink>

      <button
        type="button"
        aria-label="メニュー"
        class="flex h-11 w-11 items-center justify-center rounded-full border border-white/20 bg-white/10 text-white transition-colors duration-200 hover:bg-white/18"
      >
        <svg
          viewBox="0 0 24 24"
          class="h-[18px] w-[18px]"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
        >
          <path d="M4 7h16" />
          <path d="M4 12h16" />
          <path d="M4 17h16" />
        </svg>
      </button>
    </div>
  </nav>
</template>
