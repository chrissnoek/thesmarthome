<script setup lang="ts">
definePageMeta({
  layout: false,
})

useHead({
  title: 'the smart home — Lampen & Logica',
  meta: [
    {
      name: 'description',
      content: 'Een onafhankelijke Nederlandse publicatie over slim wonen. Reviews, vergelijkingen en koopgidsen voor wie z\'n woning slim wil maken.',
    },
  ],
})

/* -------------------------------------------------------------
 *  Editorial content model — curated overlay on top of @nuxt/content
 * -----------------------------------------------------------*/

type Feature = {
  slug: string
  category: string
  date: string
  author?: string
  title: string
  excerpt: string
  image: string
  imageAlt: string
}

const issueNo = 42
const issueDate = '30 mei 2026'

const featured: Feature[] = [
  {
    slug: '/smart-home-voor-beginners',
    category: 'Beginners',
    date: '12 mei 2026',
    author: 'Eva Bakker',
    title: 'Hoe je begint zonder spijt — een gids voor de eerste vijf apparaten',
    excerpt: 'Tegen elke nieuwe smart home staan minstens drie spijtpunten. We schreven de gids die we onszelf gunden in 2019.',
    image: 'https://images.unsplash.com/photo-1616486338812-3dadae4b4ace?w=1400&q=80&auto=format&fit=crop',
    imageAlt: 'Lichte woonkamer met warme lampen',
  },
  {
    slug: '/welke-slimme-thermostaat-is-het-beste',
    category: 'Klimaat',
    date: '08 mei 2026',
    author: 'Sven Hartman',
    title: 'Nest, Tado en Honeywell — welke thermostaat past bij een Nederlandse CV?',
    excerpt: 'Drie thermostaten, drie weken in hetzelfde rijtjeshuis, één meter die niet liegt. Resultaten in kWh, niet in marketing.',
    image: 'https://images.unsplash.com/photo-1545558014-8692077e9b5c?w=1400&q=80&auto=format&fit=crop',
    imageAlt: 'Slimme thermostaat aan een witte muur',
  },
  {
    slug: '/beste-slimme-verlichting',
    category: 'Verlichting',
    date: '02 mei 2026',
    author: 'Maud de Groot',
    title: 'Voorbij Philips Hue — vier alternatieven die je niet wegjagen bij de prijs',
    excerpt: 'Een avond met Ikea, een ochtend met Hue, en een week met de stille buitenstaander. Welke kleur licht is een gewoonte waard?',
    image: 'https://images.unsplash.com/photo-1513506003901-1e6a229e2d15?w=1400&q=80&auto=format&fit=crop',
    imageAlt: 'Warme pendelverlichting in een eetkamer',
  },
]

const filters = [
  { id: 'all', label: 'Alles' },
  { id: 'reviews', label: 'Reviews' },
  { id: 'vergelijking', label: 'Vergelijkingen' },
  { id: 'beginners', label: 'Beginners' },
  { id: 'klimaat', label: 'Klimaat' },
  { id: 'veiligheid', label: 'Veiligheid' },
]
const activeFilter = ref('all')

const deepDive: Feature[] = [
  {
    slug: '/ring-3-vergelijking-eufy-deurbel',
    category: 'Vergelijking',
    date: '24 april 2026',
    title: 'Hoe we 80 uur deurbel-beelden bekeken zonder gek te worden',
    excerpt: 'De Ring 3 en de Eufy zien hetzelfde landje voor de voordeur. Wat ze ermee doen — en wat ze met je data doen — verschilt sterker dan je denkt.',
    image: 'https://images.unsplash.com/photo-1558618666-fcd25c85cd64?w=1400&q=80&auto=format&fit=crop',
    imageAlt: 'Architecturale houten voordeur',
  },
  {
    slug: '/welke-slimme-beveiligingscamera-is-het-beste',
    category: 'Diepgaand — Veiligheid',
    date: '18 april 2026',
    title: 'Hoe vraag je een camera om te kijken zonder te kijken?',
    excerpt: 'Zes camera\'s op één balkon, één maand lang. Wat een Nederlandse rechter ervan vindt, en wat de buren.',
    image: 'https://images.unsplash.com/photo-1593784991095-a205069470b6?w=1400&q=80&auto=format&fit=crop',
    imageAlt: 'Minimalistische beveiligingscamera tegen een betonnen muur',
  },
  {
    slug: '/versterken-wifi-in-huis',
    category: 'Lab — Netwerk',
    date: '11 april 2026',
    title: 'Waarom je mesh-netwerk traag is — en hoe je dat in een middag oplost',
    excerpt: 'Een Amsterdams trapgat, drie etages en één router. We meten waar de signaalverliezen zitten en welke vier ingrepen écht helpen.',
    image: '',
    imageAlt: '',
  },
  {
    slug: '/beste-slimme-deurslot',
    category: 'Review',
    date: '04 april 2026',
    title: 'Drie sloten die je sleutel echt overbodig maken',
    excerpt: 'Een week zonder huissleutel, drie pakketbezorgers en één schoonmoeder die niet binnenkwam. Een eerlijke proef.',
    image: 'https://images.unsplash.com/photo-1582719188393-bb71ca45dbb9?w=1400&q=80&auto=format&fit=crop',
    imageAlt: 'Modern voordeurslot in matzwart',
  },
]

const articles: Feature[] = [
  {
    slug: '/beste-robotmaaier',
    category: 'Tuin',
    date: '28 maart 2026',
    title: 'De gemaaide vrijdagavond — robotmaaiers die je weekend teruggeven',
    excerpt: 'Vier maaiers, drie tuinen, één opmerkelijk traag exemplaar dat tóch onze favoriet werd.',
    image: 'https://images.unsplash.com/photo-1416879595882-3373a0480b5b?w=1400&q=80&auto=format&fit=crop',
    imageAlt: 'Zonnige tuin met vers gemaaid gras',
  },
  {
    slug: '/beste-slimme-speakers',
    category: 'Geluid',
    date: '21 maart 2026',
    title: 'Vijf luidsprekers die niet vergeten dat een huis een huis is',
    excerpt: 'Esthetiek, geluid en hoe vaak je per dag "Hé Google" wilt zeggen — een ranglijst.',
    image: 'https://images.unsplash.com/photo-1545454675-3531b543be5d?w=1400&q=80&auto=format&fit=crop',
    imageAlt: 'Houten luidspreker op een marmeren tafel',
  },
  {
    slug: '/beste-slimme-stekker',
    category: 'Klein spul',
    date: '14 maart 2026',
    title: 'De stekker is de eenvoudigste opstap — maar welke?',
    excerpt: 'Acht stekkers, een meter die niet liegt en één Action-aanbieding die ons verraste.',
    image: 'https://images.unsplash.com/photo-1558002038-1055907df827?w=1400&q=80&auto=format&fit=crop',
    imageAlt: 'Slimme stekker in een stopcontact bij avondlicht',
  },
]

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
         HERO — light cream surface with warm peach/mint gradient
    ============================================================ -->
    <section class="relative hero-glow overflow-hidden">
      <SiteHeader />

      <div class="relative mx-auto max-w-[1320px] px-6 md:px-10 pt-10 md:pt-16 pb-20 md:pb-28">
        <!-- floating issue stamp -->
        <div class="absolute top-8 right-6 md:right-10 hidden sm:flex flex-col items-end gap-1 text-ink/70">
          <div class="eyebrow">Editie</div>
          <div class="font-display text-[44px] leading-none italic font-light">№ {{ issueNo }}</div>
          <div class="text-[12px] text-clay mt-1">{{ issueDate }}</div>
        </div>

        <!-- eyebrow line -->
        <div class="flex items-center gap-4 mb-12 md:mb-20">
          <div class="w-10 h-px bg-ink" />
          <span class="eyebrow">Een publicatie over slim wonen — sinds 2019</span>
        </div>

        <!-- Masthead title -->
        <div class="grid grid-cols-1 md:grid-cols-12 gap-8 md:gap-16 items-end">
          <h1 class="md:col-span-8 font-display text-[clamp(64px,11vw,170px)] leading-[0.88] tracking-[-0.035em] text-ink">
            Lampen <span class="italic font-light">&amp;</span><br />
            Logica<span class="text-amber">.</span>
          </h1>

          <div class="md:col-span-4 md:pb-4">
            <div class="hairline mb-5 w-12" />
            <p class="font-display italic text-[22px] leading-[1.3] text-clay max-w-[28ch]">
              "Onze grootste uitdaging is schrijven over een huis dat denkt — zonder onze lezer kwijt te raken aan jargon."
            </p>
            <p class="mt-4 text-[13px] text-stone">— de redactie</p>
          </div>
        </div>

        <!-- Filter tabs -->
        <div class="mt-16 md:mt-24 flex flex-wrap items-center gap-x-7 gap-y-3 border-b border-rule pb-3">
          <button
            v-for="f in filters"
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
            <span>112 verhalen</span>
            <span class="inline-block w-1 h-1 rounded-full bg-stone" />
            <span>Sorteer op datum</span>
          </div>
        </div>
      </div>
    </section>

    <!-- ============================================================
         FEATURED — 3-up horizontal rail with editorial cards
    ============================================================ -->
    <section class="relative pb-24 md:pb-32 -mt-6">
      <div class="mx-auto max-w-[1320px] px-6 md:px-10">
        <div class="grid grid-cols-1 md:grid-cols-3 gap-x-8 gap-y-14">
          <article
            v-for="(item, i) in featured"
            :key="item.slug"
            class="group"
            :style="{ animationDelay: `${i * 80}ms` }"
          >
            <NuxtLink :to="item.slug" class="block">
              <div class="relative aspect-[4/5] overflow-hidden bg-paper-2">
                <img
                  :src="item.image"
                  :alt="item.imageAlt"
                  loading="lazy"
                  class="absolute inset-0 w-full h-full object-cover transition-transform duration-[1200ms] ease-out group-hover:scale-[1.04]"
                />
                <!-- middle floating compare badge on center card -->
                <div
                  v-if="i === 1"
                  class="absolute inset-0 flex items-center justify-center pointer-events-none"
                >
                  <div class="w-14 h-14 rounded-full bg-paper/95 backdrop-blur-sm flex items-center justify-center shadow-lg">
                    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#0f0f0e" stroke-width="1.8">
                      <path d="m9 6-6 6 6 6M15 6l6 6-6 6" stroke-linecap="round" stroke-linejoin="round" />
                    </svg>
                  </div>
                </div>
              </div>

              <div class="mt-5 flex items-center gap-2 text-[11px] tracking-wider uppercase text-stone">
                <span>Gepubliceerd in {{ item.category }}</span>
                <span class="inline-block w-[3px] h-[3px] rounded-full bg-stone" />
                <span>{{ item.date }}</span>
                <template v-if="item.author">
                  <span class="ml-auto italic font-display text-[13px] normal-case tracking-normal text-clay">
                    door {{ item.author }}
                  </span>
                </template>
              </div>

              <h3 class="mt-3 font-display text-[26px] leading-[1.1] tracking-[-0.015em] text-ink">
                {{ item.title }}
              </h3>
              <p class="mt-3 text-[15px] leading-relaxed text-clay max-w-[42ch]">
                {{ item.excerpt }}
              </p>

              <div class="mt-5 inline-flex items-center gap-2 text-[13px] font-medium text-ink">
                <span class="link-rise">Lees het verhaal</span>
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
         DARK DEEP-DIVE — full-bleed ink section with mosaic grid
    ============================================================ -->
    <section class="relative bg-ink-2 text-bone overflow-hidden">
      <!-- subtle warm radial in upper-right of dark section -->
      <div
        class="absolute -top-32 -right-32 w-[600px] h-[600px] rounded-full pointer-events-none opacity-20"
        style="background: radial-gradient(circle, #c97a2e 0%, transparent 60%)"
      />

      <div class="relative mx-auto max-w-[1320px] px-6 md:px-10 py-24 md:py-32">
        <!-- Eyebrow + intro -->
        <div class="grid grid-cols-1 md:grid-cols-12 gap-8 mb-16 md:mb-24">
          <div class="md:col-span-3">
            <div class="flex items-center gap-3">
              <span class="w-2.5 h-2.5 rounded-full bg-amber-soft" />
              <span class="eyebrow text-bone-2">Diepgaand</span>
            </div>
          </div>
          <div class="md:col-span-6">
            <h2 class="font-display text-[clamp(40px,5vw,68px)] leading-[1] tracking-[-0.025em]">
              Onderzoek dat <span class="italic font-light text-amber-soft">tijd kost</span> — omdat het tijd nodig heeft.
            </h2>
          </div>
          <div class="md:col-span-3 md:pt-3">
            <p class="text-[14px] leading-relaxed text-bone-2 max-w-[28ch]">
              Lange tests, eerlijke conclusies. We schrijven pas wanneer we genoeg gemeten hebben.
            </p>
          </div>
        </div>

        <!-- Mosaic 2×2 -->
        <div class="grid grid-cols-1 md:grid-cols-12 gap-x-6 gap-y-12 md:gap-y-20">

          <!-- Card 1 — large image left, title beneath -->
          <article class="md:col-span-7 group">
            <NuxtLink :to="deepDive[0].slug">
              <div class="aspect-[16/10] overflow-hidden">
                <img
                  :src="deepDive[0].image"
                  :alt="deepDive[0].imageAlt"
                  loading="lazy"
                  class="w-full h-full object-cover transition-transform duration-[1200ms] group-hover:scale-[1.03]"
                />
              </div>
              <div class="mt-6 max-w-[36rem]">
                <div class="eyebrow text-bone-3 mb-2">{{ deepDive[0].category }} — {{ deepDive[0].date }}</div>
                <h3 class="font-display text-[clamp(28px,3vw,42px)] leading-[1.05] tracking-[-0.02em] text-bone">
                  {{ deepDive[0].title }}
                </h3>
                <p class="mt-4 text-[15px] leading-relaxed text-bone-2">
                  {{ deepDive[0].excerpt }}
                </p>
              </div>
            </NuxtLink>
          </article>

          <!-- Card 2 — image with overlaid title (top right) -->
          <article class="md:col-span-5 group md:mt-12">
            <NuxtLink :to="deepDive[1].slug">
              <div class="relative aspect-[4/5] overflow-hidden">
                <img
                  :src="deepDive[1].image"
                  :alt="deepDive[1].imageAlt"
                  loading="lazy"
                  class="absolute inset-0 w-full h-full object-cover transition-transform duration-[1200ms] group-hover:scale-[1.03]"
                />
                <div class="absolute inset-0 bg-gradient-to-t from-black/70 via-black/15 to-transparent" />
                <div class="absolute bottom-0 left-0 right-0 p-6 md:p-8">
                  <div class="eyebrow text-bone/70 mb-2">{{ deepDive[1].category }}</div>
                  <h3 class="font-display text-[clamp(22px,2.5vw,30px)] leading-[1.1] tracking-[-0.015em] text-bone underline decoration-1 underline-offset-[6px] decoration-bone/40">
                    {{ deepDive[1].title }}
                  </h3>
                  <p class="mt-3 text-[14px] leading-relaxed text-bone/85 max-w-[34ch]">
                    {{ deepDive[1].excerpt }}
                  </p>
                </div>
              </div>
            </NuxtLink>
          </article>

          <!-- Card 3 — text-only block, no image -->
          <article class="md:col-span-5 group flex flex-col justify-end pr-6">
            <NuxtLink :to="deepDive[2].slug">
              <div class="flex items-center gap-2 mb-5">
                <span class="text-amber-soft">●</span>
                <span class="eyebrow text-bone-3">Lab notitie · {{ deepDive[2].date }}</span>
              </div>
              <h3 class="font-display text-[clamp(28px,3.5vw,46px)] leading-[1.05] tracking-[-0.02em] text-bone">
                {{ deepDive[2].title }}
              </h3>
              <p class="mt-5 text-[15px] leading-relaxed text-bone-2 max-w-[40ch]">
                {{ deepDive[2].excerpt }}
              </p>
              <div class="mt-7 hairline-dark w-24" />
              <div class="mt-5 inline-flex items-center gap-2 text-[13px] text-bone group-hover:text-amber-soft transition-colors">
                Lees het onderzoek
                <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="transition-transform group-hover:translate-x-1">
                  <path d="M5 12h14M13 6l6 6-6 6" stroke-linecap="round" stroke-linejoin="round" />
                </svg>
              </div>
            </NuxtLink>
          </article>

          <!-- Card 4 — image with title beneath -->
          <article class="md:col-span-7 group">
            <NuxtLink :to="deepDive[3].slug">
              <div class="aspect-[16/9] overflow-hidden">
                <img
                  :src="deepDive[3].image"
                  :alt="deepDive[3].imageAlt"
                  loading="lazy"
                  class="w-full h-full object-cover transition-transform duration-[1200ms] group-hover:scale-[1.03]"
                />
              </div>
              <div class="mt-5 max-w-[36rem]">
                <div class="eyebrow text-bone-3 mb-2">{{ deepDive[3].category }} — {{ deepDive[3].date }}</div>
                <h3 class="font-display text-[clamp(24px,2.6vw,34px)] leading-[1.1] tracking-[-0.015em] text-bone">
                  {{ deepDive[3].title }}
                </h3>
                <p class="mt-3 text-[14px] leading-relaxed text-bone-2 max-w-[44ch]">
                  {{ deepDive[3].excerpt }}
                </p>
              </div>
            </NuxtLink>
          </article>
        </div>

        <!-- Marquee strip — branding flourish -->
        <div class="mt-24 md:mt-32 -mx-6 md:-mx-10 overflow-hidden border-t border-b border-white/10 py-5">
          <div class="flex whitespace-nowrap animate-marquee gap-12 text-[14px] text-bone-3 italic font-display">
            <span class="flex items-center gap-12">
              <span>Onafhankelijk sinds 2019</span><span>●</span>
              <span>Geen sponsored content</span><span>●</span>
              <span>112 verhalen gepubliceerd</span><span>●</span>
              <span>2.300 kWh getest</span><span>●</span>
              <span>1 buurthuis geadviseerd</span><span>●</span>
            </span>
            <span class="flex items-center gap-12">
              <span>Onafhankelijk sinds 2019</span><span>●</span>
              <span>Geen sponsored content</span><span>●</span>
              <span>112 verhalen gepubliceerd</span><span>●</span>
              <span>2.300 kWh getest</span><span>●</span>
              <span>1 buurthuis geadviseerd</span><span>●</span>
            </span>
          </div>
        </div>
      </div>
    </section>

    <!-- ============================================================
         ARTICLES RAIL — horizontal scroll cards with arrows
    ============================================================ -->
    <section class="relative bg-paper py-24 md:py-32">
      <div class="mx-auto max-w-[1320px] px-6 md:px-10">

        <div class="flex items-end justify-between gap-8 mb-12 md:mb-16">
          <div class="max-w-[52rem]">
            <div class="eyebrow text-stone mb-3">Verhalen</div>
            <h2 class="font-display text-[clamp(48px,7vw,96px)] leading-[0.95] tracking-[-0.03em] text-ink">
              Geschreven door bewoners,<br />
              <span class="italic font-light">niet door</span> fabrikanten.
            </h2>
            <p class="mt-5 text-[15px] leading-relaxed text-clay max-w-[40ch]">
              Reviews door mensen die het apparaat een seizoen in huis hadden, niet een middag op de redactie.
            </p>
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
            v-for="item in articles"
            :key="item.slug"
            class="group shrink-0 w-[82vw] sm:w-[60vw] md:w-[400px] lg:w-[440px] snap-start"
          >
            <NuxtLink :to="item.slug" class="block">
              <div class="aspect-[4/5] overflow-hidden bg-paper-2">
                <img
                  :src="item.image"
                  :alt="item.imageAlt"
                  loading="lazy"
                  class="w-full h-full object-cover transition-transform duration-[1200ms] group-hover:scale-[1.04]"
                />
              </div>

              <div class="mt-5 flex items-center gap-2 text-[11px] tracking-wider uppercase text-stone">
                <span>{{ item.category }}</span>
                <span class="inline-block w-[3px] h-[3px] rounded-full bg-stone" />
                <span>{{ item.date }}</span>
              </div>

              <h3 class="mt-3 font-display text-[24px] leading-[1.1] tracking-[-0.015em] text-ink">
                {{ item.title }}
              </h3>
              <p class="mt-3 text-[14px] leading-relaxed text-clay max-w-[36ch]">
                {{ item.excerpt }}
              </p>

              <div class="mt-5">
                <span class="inline-flex items-center gap-2 rounded-pill border border-ink px-4 py-2 text-[12px] font-medium text-ink hover:bg-ink hover:text-paper transition-colors">
                  Lees verder
                  <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M5 12h14M13 6l6 6-6 6" stroke-linecap="round" stroke-linejoin="round" />
                  </svg>
                </span>
              </div>
            </NuxtLink>
          </article>

          <!-- Final card: archive teaser -->
          <article class="shrink-0 w-[82vw] sm:w-[60vw] md:w-[400px] lg:w-[440px] snap-start">
            <NuxtLink to="/" class="block h-full">
              <div class="aspect-[4/5] bg-ink text-bone flex flex-col items-center justify-center text-center p-10 hover:bg-amber hover:text-ink transition-colors">
                <div class="eyebrow text-bone-2 mb-6">Archief</div>
                <div class="font-display text-[72px] leading-none italic font-light">112</div>
                <div class="mt-3 text-[14px] text-bone-2">verhalen sinds 2019</div>
                <div class="mt-10 inline-flex items-center gap-2 text-[13px] font-medium">
                  Bekijk het archief
                  <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M5 12h14M13 6l6 6-6 6" stroke-linecap="round" stroke-linejoin="round" />
                  </svg>
                </div>
              </div>
            </NuxtLink>
          </article>
        </div>
      </div>
    </section>

    <!-- ============================================================
         CTA — newsletter / "Think beyond the wave" equivalent
    ============================================================ -->
    <section class="relative bg-paper">
      <div class="mx-auto max-w-[1320px] px-6 md:px-10 pb-24 md:pb-32">
        <div class="relative overflow-hidden rounded-[28px] bg-ink-2 text-bone px-8 md:px-16 py-16 md:py-24">
          <!-- soft warm glow -->
          <div
            class="absolute -bottom-40 -left-40 w-[600px] h-[600px] rounded-full pointer-events-none opacity-25"
            style="background: radial-gradient(circle, #e8b458 0%, transparent 60%)"
          />

          <div class="relative grid grid-cols-1 md:grid-cols-12 gap-10 items-end">
            <div class="md:col-span-7">
              <div class="eyebrow text-bone-2 mb-5">De zondagbrief — sinds Editie № 01</div>
              <h2 class="font-display text-[clamp(44px,6.5vw,84px)] leading-[0.95] tracking-[-0.03em] text-bone">
                Eén traag verhaal,<br />
                <span class="italic font-light text-amber-soft">elke zondag</span>, in je inbox.
              </h2>
              <p class="mt-6 text-[15px] leading-relaxed text-bone-2 max-w-[44ch]">
                Geen pushmeldingen. Geen affiliate-druk. Eén goed onderzocht stuk over slim wonen, om bij de koffie te lezen.
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

              <!-- Editorial endorsements row -->
              <div class="mt-10 grid grid-cols-3 gap-6">
                <div>
                  <div class="font-display text-[28px] leading-none">2.341</div>
                  <div class="text-[11px] uppercase tracking-wider text-bone-3 mt-1">abonnees</div>
                </div>
                <div>
                  <div class="font-display text-[28px] leading-none">94<span class="text-amber-soft">%</span></div>
                  <div class="text-[11px] uppercase tracking-wider text-bone-3 mt-1">openrate</div>
                </div>
                <div>
                  <div class="font-display text-[28px] leading-none italic font-light">0</div>
                  <div class="text-[11px] uppercase tracking-wider text-bone-3 mt-1">sponsors</div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <SiteFooter />
  </div>
</template>
