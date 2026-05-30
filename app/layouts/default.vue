<script setup lang="ts">
withDefaults(defineProps<{
  variant?: 'ink' | 'bone'
}>(), {
  variant: 'ink',
})
</script>

<template>
  <div class="min-h-screen flex flex-col bg-paper">
    <SiteHeader :variant="variant" />
    <main class="flex-1">
      <slot />
    </main>
    <SiteFooter />
  </div>
</template>
