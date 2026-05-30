<script setup lang="ts">
defineProps<{
  variant?: 'ink' | 'bone'
}>()

const nav = [
  { label: 'Verhalen', to: '/' },
  { label: 'Categorieën', to: '/#categories' },
  { label: 'Over', to: '/about' },
  { label: 'Nieuwsbrief', to: '/#nieuwsbrief' },
]
</script>

<template>
  <header
    class="relative z-30"
    :class="variant === 'bone' ? 'text-bone' : 'text-ink'"
  >
    <div class="mx-auto max-w-[1320px] px-6 md:px-10 pt-7 md:pt-8 pb-5">
      <div class="flex items-center justify-between gap-6">
        <BrandMark :variant="variant" />

        <nav class="hidden md:flex items-center gap-8 text-[14px]">
          <NuxtLink
            v-for="item in nav"
            :key="item.label"
            :to="item.to"
            class="link-rise"
          >
            {{ item.label }}
          </NuxtLink>
        </nav>

        <div class="flex items-center gap-3">
          <button
            type="button"
            class="hidden md:inline-flex items-center gap-2 text-[14px] link-rise"
            aria-label="Zoeken"
          >
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7">
              <circle cx="11" cy="11" r="7" />
              <path d="m20 20-3.5-3.5" stroke-linecap="round" />
            </svg>
            Zoeken
          </button>

          <NuxtLink
            to="/nieuwsbrief"
            class="group inline-flex items-center gap-2 rounded-pill px-4 py-2 text-[13px] font-medium transition-colors"
            :class="variant === 'bone'
              ? 'bg-bone text-ink hover:bg-amber-soft'
              : 'bg-ink text-paper hover:bg-amber'"
          >
            Abonneer
            <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="transition-transform group-hover:translate-x-0.5">
              <path d="M5 12h14M13 6l6 6-6 6" stroke-linecap="round" stroke-linejoin="round" />
            </svg>
          </NuxtLink>
        </div>
      </div>
    </div>
  </header>
</template>
