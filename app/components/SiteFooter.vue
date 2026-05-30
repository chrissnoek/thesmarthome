<script setup lang="ts">
const groups = [
  {
    title: 'Verhalen',
    links: [
      { label: 'Recente reviews', to: '/' },
      { label: 'Vergelijkingen', to: '/' },
      { label: 'Koopgidsen', to: '/' },
      { label: 'Beginners', to: '/smart-home-voor-beginners' },
    ],
  },
  {
    title: 'Categorieën',
    links: [
      { label: 'Verlichting', to: '/beste-slimme-verlichting' },
      { label: 'Klimaat', to: '/welke-slimme-thermostaat-is-het-beste' },
      { label: 'Veiligheid', to: '/wat-is-een-slimme-deurbel-welke-de-beste' },
      { label: 'Netwerk', to: '/versterken-wifi-in-huis' },
    ],
  },
  {
    title: 'Onderzoek',
    links: [
      { label: 'Test­methodiek', to: '/' },
      { label: 'Onafhankelijkheid', to: '/' },
      { label: 'Affiliate-beleid', to: '/' },
      { label: 'Privacy & data', to: '/' },
    ],
  },
  {
    title: 'Help',
    links: [
      { label: 'Contact', to: '/' },
      { label: 'Veelgestelde vragen', to: '/' },
      { label: 'Schrijf voor ons', to: '/' },
      { label: 'Pers', to: '/' },
    ],
  },
]
</script>

<template>
  <footer class="bg-paper text-ink">
    <!-- Big editorial wordmark stripe -->
    <div class="border-t border-rule">
      <div class="mx-auto max-w-[1320px] px-6 md:px-10 py-16 md:py-24">
        <!-- Heading + intro -->
        <div class="grid grid-cols-1 md:grid-cols-12 gap-10 md:gap-16">
          <div class="md:col-span-5">
            <BrandMark />
            <p class="mt-6 max-w-md text-[15px] leading-relaxed text-clay">
              Een onafhankelijke Nederlandse publicatie over slim wonen. We schrijven voor de bewoner — niet voor de fabrikant.
            </p>

            <!-- Newsletter -->
            <form class="mt-8 max-w-md" @submit.prevent>
              <label class="eyebrow text-stone block mb-3">De zondagbrief</label>
              <div class="flex items-center gap-2 border-b border-ink pb-2">
                <input
                  type="email"
                  placeholder="je@adres.nl"
                  class="flex-1 bg-transparent text-[15px] placeholder:text-stone outline-none"
                />
                <button
                  type="submit"
                  class="inline-flex items-center gap-1.5 text-[13px] font-medium hover:text-amber transition-colors"
                >
                  Aanmelden
                  <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M5 12h14M13 6l6 6-6 6" stroke-linecap="round" stroke-linejoin="round" />
                  </svg>
                </button>
              </div>
              <p class="mt-2 text-[12px] text-stone">Eén editie per week. Uitschrijven met één klik.</p>
            </form>
          </div>

          <div class="md:col-span-7 grid grid-cols-2 md:grid-cols-4 gap-8">
            <div v-for="g in groups" :key="g.title">
              <h4 class="eyebrow text-stone mb-4">{{ g.title }}</h4>
              <ul class="space-y-2.5">
                <li v-for="l in g.links" :key="l.label">
                  <NuxtLink :to="l.to" class="text-[14px] hover:text-amber transition-colors">
                    {{ l.label }}
                  </NuxtLink>
                </li>
              </ul>
            </div>
          </div>
        </div>

        <!-- Oversized masthead -->
        <div class="mt-20 md:mt-28 select-none pointer-events-none overflow-hidden">
          <div class="font-display text-ink leading-[0.85] tracking-[-0.04em] text-[clamp(80px,18vw,260px)]">
            the smart <span class="italic font-extralight">home</span><span class="text-amber">.</span>
          </div>
        </div>

        <!-- Legal bar -->
        <div class="mt-10 pt-6 border-t border-rule flex flex-col md:flex-row md:items-center justify-between gap-4 text-[12px] text-stone">
          <div>© {{ new Date().getFullYear() }} the smart home — gemaakt in Amsterdam.</div>
          <div class="flex items-center gap-5">
            <a href="#" class="hover:text-ink">Voorwaarden</a>
            <a href="#" class="hover:text-ink">Privacy</a>
            <a href="#" class="hover:text-ink">Cookies</a>
            <span class="inline-flex items-center gap-1.5">
              <span class="inline-block w-1.5 h-1.5 rounded-full bg-moss"></span>
              Klimaatbewust gehost
            </span>
          </div>
        </div>
      </div>
    </div>
  </footer>
</template>
