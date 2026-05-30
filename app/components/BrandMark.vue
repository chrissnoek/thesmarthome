<script setup lang="ts">
defineProps<{
  variant?: 'ink' | 'bone'
}>()
</script>

<template>
  <NuxtLink
    to="/"
    class="group inline-flex items-center gap-[10px] select-none"
    :class="variant === 'bone' ? 'text-bone' : 'text-ink'"
    aria-label="the smart home — home"
  >
    <!-- Mark: a small house glyph made from a square and a circle, suggesting a window with a sensor -->
    <svg
      width="28"
      height="28"
      viewBox="0 0 28 28"
      fill="none"
      class="transition-transform duration-500 group-hover:rotate-[-8deg]"
    >
      <rect
        x="2.5" y="2.5" width="23" height="23" rx="6"
        :stroke="variant === 'bone' ? '#ece5d2' : '#0f0f0e'"
        stroke-width="1.4"
      />
      <circle
        cx="14" cy="14" r="4.5"
        :fill="variant === 'bone' ? '#ece5d2' : '#0f0f0e'"
      />
      <circle
        cx="14" cy="14" r="1.6"
        :fill="variant === 'bone' ? '#0f0f0e' : '#f1ecdf'"
      />
    </svg>

    <span class="font-display text-[22px] leading-none tracking-[-0.02em]">
      the smart
      <span class="italic font-light">home</span>
      <span :class="variant === 'bone' ? 'text-amber-soft' : 'text-amber'">.</span>
    </span>
  </NuxtLink>
</template>
