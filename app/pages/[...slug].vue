<script setup lang="ts">
definePageMeta({ layout: false })

const route = useRoute()

const { data: page } = await useAsyncData('page-' + route.path, () => {
  return queryCollection('content').path(route.path).first()
})

if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page not found', fatal: true })
}

useSeoMeta({
  title: () => page.value?.title ? `${page.value.title} — the smart home` : 'the smart home',
  description: () => page.value?.description,
})

function formatDate(input: unknown): string {
  if (!input) return ''
  const d = new Date(input as string)
  if (Number.isNaN(d.getTime())) return ''
  return d.toLocaleDateString('nl-NL', { day: 'numeric', month: 'long', year: 'numeric' })
}
</script>

<template>
  <div v-if="page" class="min-h-screen flex flex-col bg-paper paper-grain">

    <!-- Hero band with soft glow -->
    <section class="relative hero-glow">
      <SiteHeader />

      <article class="mx-auto max-w-[1320px] px-6 md:px-10 pt-10 md:pt-16 pb-16 md:pb-24">
        <!-- Breadcrumb / category eyebrow -->
        <div class="flex items-center gap-3 mb-10">
          <div class="w-8 h-px bg-ink/60" />
          <span class="eyebrow text-clay">
            <NuxtLink to="/" class="hover:text-ink">Verhalen</NuxtLink>
            <span v-if="page.category"> — {{ page.category }}</span>
          </span>
        </div>

        <!-- Title block -->
        <div class="grid grid-cols-1 md:grid-cols-12 gap-8 md:gap-16">
          <div class="md:col-span-9">
            <h1 class="font-display text-[clamp(40px,6.5vw,96px)] leading-[0.95] tracking-[-0.03em] text-ink">
              {{ page.title }}
            </h1>
            <p
              v-if="page.description"
              class="mt-8 font-display italic text-[clamp(20px,2.2vw,28px)] leading-[1.3] text-clay max-w-[34ch]"
            >
              {{ page.description }}
            </p>
          </div>

          <div class="md:col-span-3 md:pt-6">
            <div class="hairline mb-5 w-12" />
            <dl class="space-y-4 text-[13px]">
              <div v-if="page.date">
                <dt class="eyebrow text-stone mb-1">Gepubliceerd</dt>
                <dd class="text-ink">{{ formatDate(page.date) }}</dd>
              </div>
              <div v-if="page.category">
                <dt class="eyebrow text-stone mb-1">Sectie</dt>
                <dd class="text-ink">{{ page.category }}</dd>
              </div>
              <div>
                <dt class="eyebrow text-stone mb-1">Leestijd</dt>
                <dd class="text-ink">± 8 min</dd>
              </div>
            </dl>
          </div>
        </div>
      </article>
    </section>

    <!-- Article body -->
    <section class="relative bg-paper pb-24 md:pb-32">
      <div class="mx-auto max-w-[1320px] px-6 md:px-10">
        <div class="grid grid-cols-1 md:grid-cols-12 gap-8 md:gap-16">
          <!-- Side meta -->
          <aside class="md:col-span-3 md:order-2">
            <div class="md:sticky md:top-10 space-y-8">
              <div>
                <div class="eyebrow text-stone mb-3">Deel dit verhaal</div>
                <div class="flex items-center gap-2">
                  <a href="#" class="w-9 h-9 rounded-full border border-ink/15 hover:border-ink hover:bg-ink hover:text-paper transition-colors flex items-center justify-center" aria-label="Kopieer link">
                    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="M10 13a5 5 0 0 0 7.07 0l3-3a5 5 0 0 0-7.07-7.07l-1 1M14 11a5 5 0 0 0-7.07 0l-3 3a5 5 0 0 0 7.07 7.07l1-1" stroke-linecap="round" stroke-linejoin="round"/></svg>
                  </a>
                  <a href="#" class="w-9 h-9 rounded-full border border-ink/15 hover:border-ink hover:bg-ink hover:text-paper transition-colors flex items-center justify-center" aria-label="Deel via mail">
                    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><rect x="3" y="5" width="18" height="14" rx="2"/><path d="m3 7 9 6 9-6"/></svg>
                  </a>
                </div>
              </div>

              <div>
                <div class="eyebrow text-stone mb-3">In dit nummer</div>
                <ul class="space-y-2.5 text-[13px]">
                  <li><NuxtLink to="/" class="link-rise">Lampen &amp; Logica</NuxtLink></li>
                  <li><NuxtLink to="/" class="link-rise">De zondagbrief</NuxtLink></li>
                  <li><NuxtLink to="/" class="link-rise">Archief</NuxtLink></li>
                </ul>
              </div>
            </div>
          </aside>

          <!-- Body -->
          <div class="md:col-span-9 md:order-1">
            <div class="hairline mb-12 max-w-[200px]" />
            <div class="prose-editorial max-w-[68ch]">
              <ContentRenderer :value="page" />
            </div>

            <!-- End of article flourish -->
            <div class="mt-16 max-w-[68ch] flex items-center gap-4">
              <div class="flex-1 hairline" />
              <span class="font-display italic text-stone text-[14px]">— einde —</span>
              <div class="flex-1 hairline" />
            </div>

            <!-- Author byline -->
            <div class="mt-12 max-w-[68ch] flex items-start gap-5 p-6 bg-paper-2 rounded-2xl">
              <div class="w-12 h-12 rounded-full bg-ink text-paper flex items-center justify-center font-display text-[20px] italic shrink-0">
                e.
              </div>
              <div>
                <div class="text-[13px] uppercase tracking-wider text-stone">Geschreven door</div>
                <div class="font-display text-[20px] text-ink mt-1">Eva Bakker</div>
                <p class="mt-2 text-[14px] text-clay max-w-[44ch]">
                  Schrijft over technologie thuis sinds 2018. Woont in een rijtjeshuis dat te slim is om uit te leggen.
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Related / next read -->
    <section class="relative bg-ink-2 text-bone">
      <div class="mx-auto max-w-[1320px] px-6 md:px-10 py-24 md:py-32">
        <div class="grid grid-cols-1 md:grid-cols-12 gap-10 items-end">
          <div class="md:col-span-7">
            <div class="eyebrow text-bone-3 mb-4">Verder lezen</div>
            <h2 class="font-display text-[clamp(40px,6vw,76px)] leading-[0.95] tracking-[-0.03em]">
              Eén verhaal is <span class="italic font-light text-amber-soft">zelden genoeg.</span>
            </h2>
            <p class="mt-6 text-[14px] text-bone-2 max-w-[44ch]">
              Terug naar de redactie, voor het volgende verhaal in deze editie.
            </p>
          </div>

          <div class="md:col-span-5 flex md:justify-end">
            <NuxtLink
              to="/"
              class="group inline-flex items-center gap-3 rounded-pill bg-bone text-ink px-6 py-3 text-[14px] font-medium hover:bg-amber-soft transition-colors"
            >
              Naar alle verhalen
              <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="transition-transform group-hover:translate-x-1">
                <path d="M5 12h14M13 6l6 6-6 6" stroke-linecap="round" stroke-linejoin="round" />
              </svg>
            </NuxtLink>
          </div>
        </div>
      </div>
    </section>

    <SiteFooter />
  </div>
</template>
