<script setup lang="ts">
definePageMeta({
  layout: false,
})

useHead({
  title: 'the smart home — slim wonen, eerlijk advies',
  meta: [
    {
      name: 'description',
      content: 'De beste apparaten, technologie en uitrusting voor jouw smart home. Reviews, vergelijkingen en koopgidsen voor wie zijn woning slim wil maken.',
    },
  ],
})

/* -------------------------------------------------------------
 *  Articles are loaded directly from the @nuxt/content collection.
 *  Anything with a `category` and `date` in frontmatter appears here
 *  automatically — no hand-maintained list.
 * -----------------------------------------------------------*/

type Article = {
  path: string
  title: string
  description?: string
  category?: string
  date?: string
}

const { data: allPages } = await useAsyncData('home-articles', () =>
  queryCollection('content').all(),
)

const articles = computed<Article[]>(() => {
  return ((allPages.value ?? []) as Article[])
    .filter(p => p.category && p.path && p.path !== '/' && !p.path.startsWith('/about'))
    .sort((a, b) => {
      const da = a.date ? new Date(a.date).getTime() : 0
      const db = b.date ? new Date(b.date).getTime() : 0
      return db - da
    })
})

const totalCount = computed(() => articles.value.length)
const latest = computed(() => articles.value.slice(0, 8))

const CATEGORY_ORDER = [
  'Beginners',
  'Veiligheid en toegang',
  'Keuken en huishouden',
  'Klimaatbeheersing',
  'Entertainment',
  'Tuin en klussen',
  'Netwerk',
  'Energie',
  'Persoonlijke verzorging',
  'Starterspakketten',
]

const grouped = computed(() => {
  const map = new Map<string, Article[]>()
  for (const article of articles.value) {
    const cat = article.category || 'Overig'
    if (!map.has(cat)) map.set(cat, [])
    map.get(cat)!.push(article)
  }
  const ordered: Array<{ category: string, items: Article[] }> = []
  for (const cat of CATEGORY_ORDER) {
    if (map.has(cat)) ordered.push({ category: cat, items: map.get(cat)! })
  }
  for (const [cat, items] of map.entries()) {
    if (!CATEGORY_ORDER.includes(cat)) ordered.push({ category: cat, items })
  }
  return ordered
})

const categories = computed(() => [
  { id: 'all', label: 'Alles' },
  ...grouped.value.map(g => ({ id: g.category, label: g.category })),
])

const activeFilter = ref<string>('all')

const visibleSections = computed(() => {
  if (activeFilter.value === 'all') return grouped.value
  return grouped.value.filter(g => g.category === activeFilter.value)
})

const categoryStyle: Record<string, { accent: string, bg: string, num: string }> = {
  'Beginners': { accent: '#5f7457', bg: '#d8e6d2', num: '01' },
  'Veiligheid en toegang': { accent: '#c97a2e', bg: '#f5d5b3', num: '02' },
  'Keuken en huishouden': { accent: '#e8b458', bg: '#f5e5c3', num: '03' },
  'Klimaatbeheersing': { accent: '#c97a2e', bg: '#f5d5b3', num: '04' },
  'Entertainment': { accent: '#0f0f0e', bg: '#ebe5d2', num: '05' },
  'Tuin en klussen': { accent: '#5f7457', bg: '#d8e6d2', num: '06' },
  'Netwerk': { accent: '#c97a2e', bg: '#ebe5d2', num: '07' },
  'Energie': { accent: '#5f7457', bg: '#d8e6d2', num: '08' },
  'Persoonlijke verzorging': { accent: '#c97a2e', bg: '#f5d5b3', num: '09' },
  'Starterspakketten': { accent: '#e8b458', bg: '#f5e5c3', num: '10' },
  'Overig': { accent: '#847e72', bg: '#ebe5d2', num: '00' },
}
function styleFor(cat?: string) {
  return categoryStyle[cat || 'Overig'] ?? categoryStyle['Overig']!
}

function formatDate(input: unknown): string {
  if (!input) return ''
  const d = new Date(input as string)
  if (Number.isNaN(d.getTime())) return ''
  return d.toLocaleDateString('nl-NL', { day: 'numeric', month: 'long', year: 'numeric' })
}

const railEl = ref<HTMLElement | null>(null)
function scrollRail(dir: -1 | 1) {
  const el = railEl.value
  if (!el) return
  el.scrollBy({ left: dir * (el.clientWidth * 0.8), behavior: 'smooth' })
}
</script>

<template>
  <div class="min-h-screen flex flex-col bg-paper paper-grain">

    <!-- ============================================================
         HERO
    ============================================================ -->
    <section class="relative hero-glow overflow-hidden">
      <SiteHeader />

      <div class="relative mx-auto max-w-[1320px] px-6 md:px-10 pt-10 md:pt-16 pb-20 md:pb-28">
        <!-- archive counter -->
        <div class="absolute top-8 right-6 md:right-10 hidden sm:flex flex-col items-end gap-1 text-ink/70">
          <div class="eyebrow">In het archief</div>
          <div class="font-display text-[44px] leading-none italic font-light">{{ totalCount }}</div>
          <div class="text-[12px] text-clay mt-1">artikelen &amp; gidsen</div>
        </div>

        <div class="flex items-center gap-4 mb-12 md:mb-20">
          <div class="w-10 h-px bg-ink" />
          <span class="eyebrow">Slim wonen — eerlijk advies, sinds 2019</span>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-12 gap-8 md:gap-16 items-end">
          <h1 class="md:col-span-8 font-display text-[clamp(56px,9vw,150px)] leading-[0.88] tracking-[-0.035em] text-ink">
            the smart <span class="italic font-light">home</span><span class="text-amber">.</span>
          </h1>

          <div class="md:col-span-4 md:pb-4">
            <div class="hairline mb-5 w-12" />
            <p class="font-display italic text-[22px] leading-[1.3] text-clay max-w-[28ch]">
              De beste apparaten, technologie en uitrusting voor jouw smart home.
            </p>
            <p class="mt-4 text-[13px] text-stone">— Vergelijkingen, reviews en koopgidsen</p>
          </div>
        </div>

        <!-- category filter -->
        <div id="categories" class="mt-16 md:mt-24 flex flex-wrap items-center gap-x-7 gap-y-3 border-b border-rule pb-3">
          <button
            v-for="f in categories"
            :key="f.id"
            type="button"
            class="relative pb-2 text-[14px] transition-colors"
            :class="activeFilter === f.id ? 'text-ink' : 'text-clay hover:text-ink'"
            @click="activeFilter = f.id"
          >
            {{ f.label }}
            <span
              v-if="activeFilter === f.id"
              class="absolute -bottom-[1px] left-0 right-0 h-px bg-ink"
            />
          </button>

          <div class="ml-auto hidden md:flex items-center gap-2 text-[13px] text-stone">
            <span>{{ totalCount }} artikelen</span>
            <span class="inline-block w-1 h-1 rounded-full bg-stone" />
            <span>Sorteer op datum</span>
          </div>
        </div>
      </div>
    </section>

    <!-- ============================================================
         LATEST — horizontal rail, only when no filter applied
    ============================================================ -->
    <section v-if="activeFilter === 'all' && latest.length" class="relative bg-paper py-20 md:py-28">
      <div class="mx-auto max-w-[1320px] px-6 md:px-10">

        <div class="flex items-end justify-between gap-8 mb-10 md:mb-14">
          <div class="max-w-[52rem]">
            <div class="eyebrow text-stone mb-3">Laatste artikelen</div>
            <h2 class="font-display text-[clamp(40px,6vw,80px)] leading-[0.95] tracking-[-0.03em] text-ink">
              Net <span class="italic font-light">verschenen</span><span class="text-amber">.</span>
            </h2>
          </div>

          <div class="hidden md:flex items-center gap-2 shrink-0">
            <button
              type="button"
              class="w-12 h-12 rounded-full border border-ink hover:bg-ink hover:text-paper transition-colors flex items-center justify-center"
              aria-label="Vorige"
              @click="scrollRail(-1)"
            >
              <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                <path d="M19 12H5M11 6l-6 6 6 6" stroke-linecap="round" stroke-linejoin="round" />
              </svg>
            </button>
            <button
              type="button"
              class="w-12 h-12 rounded-full bg-ink text-paper hover:bg-amber transition-colors flex items-center justify-center"
              aria-label="Volgende"
              @click="scrollRail(1)"
            >
              <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                <path d="M5 12h14M13 6l6 6-6 6" stroke-linecap="round" stroke-linejoin="round" />
              </svg>
            </button>
          </div>
        </div>

        <div
          ref="railEl"
          class="flex gap-6 overflow-x-auto no-scrollbar snap-x snap-mandatory -mx-6 md:-mx-10 px-6 md:px-10 pb-2"
        >
          <article
            v-for="item in latest"
            :key="item.path"
            class="group shrink-0 w-[82vw] sm:w-[60vw] md:w-[400px] lg:w-[440px] snap-start"
          >
            <NuxtLink :to="item.path" class="block">
              <div
                class="aspect-[4/5] p-8 flex flex-col justify-between overflow-hidden transition-transform duration-[800ms] group-hover:scale-[1.02]"
                :style="{ background: styleFor(item.category).bg }"
              >
                <div class="flex items-start justify-between">
                  <span class="eyebrow text-ink/70">{{ item.category }}</span>
                  <span class="font-display text-[14px] text-ink/40 italic">№ {{ styleFor(item.category).num }}</span>
                </div>
                <div class="font-display text-[clamp(26px,3vw,38px)] leading-[1.05] tracking-[-0.02em] text-ink">
                  {{ item.title }}
                </div>
              </div>

              <div class="mt-5 flex items-center gap-2 text-[11px] tracking-wider uppercase text-stone">
                <span>{{ item.category }}</span>
                <span class="inline-block w-[3px] h-[3px] rounded-full bg-stone" />
                <span>{{ formatDate(item.date) }}</span>
              </div>

              <h3 class="mt-3 font-display text-[22px] leading-[1.15] tracking-[-0.015em] text-ink">
                {{ item.title }}
              </h3>
              <p v-if="item.description" class="mt-3 text-[14px] leading-relaxed text-clay max-w-[36ch]">
                {{ item.description }}
              </p>

              <div class="mt-4">
                <span class="inline-flex items-center gap-2 rounded-pill border border-ink px-4 py-2 text-[12px] font-medium text-ink hover:bg-ink hover:text-paper transition-colors">
                  Lees verder
                  <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M5 12h14M13 6l6 6-6 6" stroke-linecap="round" stroke-linejoin="round" />
                  </svg>
                </span>
              </div>
            </NuxtLink>
          </article>
        </div>
      </div>
    </section>

    <!-- ============================================================
         CATEGORY SECTIONS
         Alternates ink/paper bands for visual rhythm.
    ============================================================ -->
    <section
      v-for="(section, idx) in visibleSections"
      :key="section.category"
      class="relative"
      :class="idx % 2 === 1 ? 'bg-ink-2 text-bone' : 'bg-paper'"
    >
      <div class="mx-auto max-w-[1320px] px-6 md:px-10 py-20 md:py-28">
        <div class="grid grid-cols-1 md:grid-cols-12 gap-8 mb-12 md:mb-16">
          <div class="md:col-span-3">
            <div class="flex items-center gap-3">
              <span
                class="w-2.5 h-2.5 rounded-full"
                :style="{ background: idx % 2 === 1 ? '#e8b458' : styleFor(section.category).accent }"
              />
              <span
                class="eyebrow"
                :class="idx % 2 === 1 ? 'text-bone-2' : 'text-stone'"
              >
                Sectie · {{ String(idx + 1).padStart(2, '0') }}
              </span>
            </div>
          </div>
          <div class="md:col-span-7">
            <h2
              class="font-display text-[clamp(40px,6vw,76px)] leading-[1] tracking-[-0.025em]"
              :class="idx % 2 === 1 ? 'text-bone' : 'text-ink'"
            >
              {{ section.category }}<span
                class="italic font-light"
                :class="idx % 2 === 1 ? 'text-amber-soft' : 'text-amber'"
              >.</span>
            </h2>
          </div>
          <div class="md:col-span-2 md:pt-4">
            <div class="text-[13px]" :class="idx % 2 === 1 ? 'text-bone-3' : 'text-stone'">
              {{ section.items.length }} artikel{{ section.items.length === 1 ? '' : 'en' }}
            </div>
          </div>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-x-8 gap-y-14">
          <article
            v-for="item in section.items"
            :key="item.path"
            class="group"
          >
            <NuxtLink :to="item.path" class="block">
              <div
                class="aspect-[4/5] p-8 flex flex-col justify-between overflow-hidden transition-transform duration-[800ms] group-hover:scale-[1.02]"
                :style="{ background: styleFor(section.category).bg }"
              >
                <div class="flex items-start justify-between">
                  <span class="eyebrow text-ink/70">{{ section.category }}</span>
                  <span class="font-display text-[14px] text-ink/40 italic">№ {{ styleFor(section.category).num }}</span>
                </div>
                <div class="font-display text-[clamp(22px,2.6vw,30px)] leading-[1.05] tracking-[-0.02em] text-ink">
                  {{ item.title }}
                </div>
              </div>

              <div
                class="mt-5 flex items-center gap-2 text-[11px] tracking-wider uppercase"
                :class="idx % 2 === 1 ? 'text-bone-3' : 'text-stone'"
              >
                <span>{{ formatDate(item.date) }}</span>
              </div>
              <h3
                class="mt-3 font-display text-[22px] leading-[1.1] tracking-[-0.015em]"
                :class="idx % 2 === 1 ? 'text-bone' : 'text-ink'"
              >
                {{ item.title }}
              </h3>
              <p
                v-if="item.description"
                class="mt-3 text-[14px] leading-relaxed max-w-[42ch]"
                :class="idx % 2 === 1 ? 'text-bone-2' : 'text-clay'"
              >
                {{ item.description }}
              </p>

              <div
                class="mt-4 inline-flex items-center gap-2 text-[13px] font-medium"
                :class="idx % 2 === 1 ? 'text-bone group-hover:text-amber-soft' : 'text-ink'"
              >
                <span class="link-rise">Lees verder</span>
                <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="transition-transform group-hover:translate-x-1">
                  <path d="M5 12h14M13 6l6 6-6 6" stroke-linecap="round" stroke-linejoin="round" />
                </svg>
              </div>
            </NuxtLink>
          </article>
        </div>
      </div>
    </section>

    <!-- ============================================================
         CTA — newsletter
    ============================================================ -->
    <section class="relative bg-paper">
      <div class="mx-auto max-w-[1320px] px-6 md:px-10 pb-24 md:pb-32 pt-12">
        <div class="relative overflow-hidden rounded-[28px] bg-ink-2 text-bone px-8 md:px-16 py-16 md:py-24">
          <div
            class="absolute -bottom-40 -left-40 w-[600px] h-[600px] rounded-full pointer-events-none opacity-25"
            style="background: radial-gradient(circle, #e8b458 0%, transparent 60%)"
          />

          <div class="relative grid grid-cols-1 md:grid-cols-12 gap-10 items-end">
            <div class="md:col-span-7">
              <div id="nieuwsbrief" class="eyebrow text-bone-2 mb-5">Nieuwsbrief</div>
              <h2 class="font-display text-[clamp(40px,6vw,76px)] leading-[0.95] tracking-[-0.03em] text-bone">
                Eén goed verhaal,<br />
                <span class="italic font-light text-amber-soft">af en toe</span>, in je inbox.
              </h2>
              <p class="mt-6 text-[15px] leading-relaxed text-bone-2 max-w-[44ch]">
                Aanbiedingen, vergelijkingen en nieuwe artikelen — alleen wanneer we iets te zeggen hebben.
              </p>
            </div>

            <div class="md:col-span-5">
              <form class="flex flex-col gap-3" @submit.prevent>
                <div class="flex items-center gap-2 bg-paper text-ink rounded-pill pl-5 pr-1.5 py-1.5">
                  <input
                    type="email"
                    placeholder="je@adres.nl"
                    class="flex-1 bg-transparent text-[15px] placeholder:text-stone outline-none py-2"
                  />
                  <button
                    type="submit"
                    class="inline-flex items-center gap-1.5 rounded-pill bg-ink text-paper px-5 py-2.5 text-[13px] font-medium hover:bg-amber hover:text-ink transition-colors"
                  >
                    Abonneer
                    <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                      <path d="M5 12h14M13 6l6 6-6 6" stroke-linecap="round" stroke-linejoin="round" />
                    </svg>
                  </button>
                </div>
                <p class="text-[12px] text-bone-3 pl-5">
                  Gratis. Uitschrijven met één klik. Geen tracking.
                </p>
              </form>
            </div>
          </div>
        </div>
      </div>
    </section>

    <SiteFooter />
  </div>
</template>
