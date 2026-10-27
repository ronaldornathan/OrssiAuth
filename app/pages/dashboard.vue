<script setup lang="ts">
type DashboardSection = 'overview' | 'activity' | 'security'

const toast = useToast()
const activeSection = ref<DashboardSection>('overview')

const sections: Array<{
  id: DashboardSection
  label: string
  icon: string
}> = [
  { id: 'overview', label: 'Vue d’ensemble', icon: 'i-lucide-layout-dashboard' },
  { id: 'activity', label: 'Activité', icon: 'i-lucide-activity' },
  { id: 'security', label: 'Sécurité', icon: 'i-lucide-shield-check' }
]

const activityDays = [
  { label: 'Lun', value: 38 },
  { label: 'Mar', value: 62 },
  { label: 'Mer', value: 46 },
  { label: 'Jeu', value: 82 },
  { label: 'Ven', value: 56 },
  { label: 'Sam', value: 30 },
  { label: 'Dim', value: 68 }
]

const sessions = [
  {
    device: 'Chrome sur Windows',
    location: 'Paris, France',
    date: 'Aujourd’hui, 09:42',
    icon: 'i-lucide-monitor',
    current: true
  },
  {
    device: 'Safari sur iPhone',
    location: 'Paris, France',
    date: 'Hier, 18:16',
    icon: 'i-lucide-smartphone',
    current: false
  },
  {
    device: 'Firefox sur macOS',
    location: 'Lyon, France',
    date: '20 octobre, 11:08',
    icon: 'i-lucide-monitor',
    current: false
  }
]

useHead({
  title: 'Tableau de bord',
  meta: [
    {
      name: 'description',
      content: 'Vue d’ensemble de votre espace personnel OrssiAuth.'
    }
  ]
})

function showDemoNotice() {
  toast.add({
    title: 'Aperçu de démonstration',
    description: 'Cette action sera disponible après la connexion du service d’authentification.',
    color: 'warning',
    icon: 'i-lucide-info'
  })
}
</script>

<template>
  <main class="relative isolate min-h-[calc(100dvh-4rem)] overflow-hidden">
    <Background variant="circuits" :speed="1" />
    <div
      aria-hidden="true"
      class="pointer-events-none absolute inset-0 bg-linear-to-b from-default/10 via-default/30 to-default/70"
    />

    <UContainer class="relative z-10 py-8 sm:py-10">
      <header class="mb-8 flex flex-col justify-between gap-5 border-b border-default/60 pb-7 sm:flex-row sm:items-end">
        <div>
          <div class="mb-3 flex flex-wrap items-center gap-3">
            <p class="text-xs font-semibold uppercase tracking-[0.16em] text-primary">Espace personnel</p>
            <UBadge color="neutral" variant="subtle" size="sm">Aperçu démo</UBadge>
          </div>
          <h1 class="text-3xl font-semibold tracking-tight text-highlighted sm:text-4xl">Bonjour, Camille.</h1>
          <p class="mt-2 text-sm leading-6 text-muted">Voici ce qui se passe dans votre espace aujourd’hui.</p>
        </div>

        <UButton
          label="Compléter mon profil"
          icon="i-lucide-user-round-pen"
          trailing-icon="i-lucide-arrow-right"
          color="primary"
          size="lg"
          class="w-full shadow-lg shadow-primary/15 sm:w-auto"
          @click="showDemoNotice"
        />
      </header>

      <div class="grid gap-8 lg:grid-cols-[220px_minmax(0,1fr)] lg:gap-10">
        <aside>
          <nav aria-label="Navigation du tableau de bord" class="flex gap-2 overflow-x-auto pb-2 lg:flex-col lg:overflow-visible lg:pb-0">
            <UButton
              v-for="section in sections"
              :key="section.id"
              :label="section.label"
              :icon="section.icon"
              :variant="activeSection === section.id ? 'subtle' : 'ghost'"
              :color="activeSection === section.id ? 'primary' : 'neutral'"
              size="lg"
              class="shrink-0 justify-start lg:w-full"
              :aria-current="activeSection === section.id ? 'page' : undefined"
              @click="activeSection = section.id"
            />
          </nav>

          <div class="mt-8 hidden border-t border-default/60 pt-6 lg:block">
            <p class="text-xs font-semibold uppercase tracking-[0.14em] text-muted">Connecté en tant que</p>
            <div class="mt-4 flex items-center gap-3">
              <UAvatar text="CM" size="md" class="bg-primary/15 text-primary" />
              <div class="min-w-0">
                <p class="truncate text-sm font-semibold text-highlighted">Camille Martin</p>
                <p class="truncate text-xs text-muted">camille@exemple.com</p>
              </div>
            </div>
          </div>
        </aside>

        <section class="min-w-0" aria-live="polite">
          <template v-if="activeSection === 'overview'">
            <div class="grid border-y border-default/60 sm:grid-cols-3">
              <div class="py-5 sm:pr-6">
                <p class="text-sm text-muted">Sessions actives</p>
                <p class="mt-2 text-3xl font-semibold text-highlighted">2</p>
                <p class="mt-1 flex items-center gap-1.5 text-xs text-primary">
                  <UIcon name="i-lucide-circle-check" class="size-3.5" />
                  Vos appareils
                </p>
              </div>
              <div class="border-default/60 py-5 sm:border-l sm:px-6">
                <p class="text-sm text-muted">Dernière connexion</p>
                <p class="mt-2 text-xl font-semibold text-highlighted">Aujourd’hui</p>
                <p class="mt-1 text-xs text-muted">09:42 · Paris, France</p>
              </div>
              <div class="border-default/60 py-5 sm:border-l sm:pl-6">
                <p class="text-sm text-muted">Sécurité du compte</p>
                <p class="mt-2 text-xl font-semibold text-highlighted">À renforcer</p>
                <button
                  type="button"
                  class="mt-1 text-xs font-medium text-primary underline-offset-4 hover:underline"
                  @click="activeSection = 'security'"
                >
                  Consulter les conseils
                </button>
              </div>
            </div>

            <div class="mt-8 grid gap-8 xl:grid-cols-[minmax(0,1.4fr)_minmax(260px,0.8fr)]">
              <section class="rounded-xl border border-default/70 bg-default/75 p-5 backdrop-blur-xl sm:p-6" aria-labelledby="activity-title">
                <div class="flex items-start justify-between gap-4">
                  <div>
                    <h2 id="activity-title" class="font-semibold text-highlighted">Activité de la semaine</h2>
                    <p class="mt-1 text-sm text-muted">Connexions à votre espace</p>
                  </div>
                  <UButton
                    label="Historique"
                    trailing-icon="i-lucide-arrow-right"
                    color="neutral"
                    variant="ghost"
                    size="sm"
                    @click="activeSection = 'activity'"
                  />
                </div>

                <div class="mt-7 flex h-36 items-end justify-between gap-3" role="img" aria-label="Activité de connexion sur les sept derniers jours">
                  <div v-for="day in activityDays" :key="day.label" class="flex h-full flex-1 flex-col items-center justify-end gap-2">
                    <div class="flex h-full w-full items-end rounded-md bg-elevated/70 p-1">
                      <div class="w-full rounded-sm bg-primary/75 transition-[height] duration-500" :style="{ height: `${day.value}%` }" />
                    </div>
                    <span class="text-[11px] text-muted">{{ day.label }}</span>
                  </div>
                </div>
              </section>

              <section class="rounded-xl border border-default/70 bg-default/75 p-5 backdrop-blur-xl sm:p-6" aria-labelledby="account-title">
                <div class="flex items-center justify-between gap-3">
                  <h2 id="account-title" class="font-semibold text-highlighted">Votre compte</h2>
                  <UIcon name="i-lucide-id-card" class="size-5 text-primary" />
                </div>
                <dl class="mt-5 divide-y divide-default/70">
                  <div class="py-3 first:pt-0">
                    <dt class="text-xs text-muted">Nom</dt>
                    <dd class="mt-1 text-sm font-medium text-highlighted">Camille Martin</dd>
                  </div>
                  <div class="py-3">
                    <dt class="text-xs text-muted">Adresse e-mail</dt>
                    <dd class="mt-1 break-all text-sm font-medium text-highlighted">camille@exemple.com</dd>
                  </div>
                  <div class="py-3 last:pb-0">
                    <dt class="text-xs text-muted">Méthode de connexion</dt>
                    <dd class="mt-1 flex items-center gap-2 text-sm font-medium text-highlighted">
                      <UIcon name="i-lucide-mail" class="size-4 text-muted" /> E-mail et mot de passe
                    </dd>
                  </div>
                </dl>
              </section>
            </div>

            <section class="mt-8" aria-labelledby="sessions-title">
              <div class="mb-4 flex items-center justify-between gap-4">
                <div>
                  <h2 id="sessions-title" class="font-semibold text-highlighted">Connexions récentes</h2>
                  <p class="mt-1 text-sm text-muted">Appareils utilisés pour accéder au compte</p>
                </div>
                <UButton
                  label="Tout voir"
                  trailing-icon="i-lucide-arrow-right"
                  color="neutral"
                  variant="ghost"
                  size="sm"
                  @click="activeSection = 'activity'"
                />
              </div>
              <div class="divide-y divide-default/70 border-y border-default/60">
                <div v-for="session in sessions.slice(0, 2)" :key="session.device" class="flex items-center gap-4 py-4">
                  <div class="flex size-10 shrink-0 items-center justify-center rounded-lg border border-default/70 bg-default/70 text-muted">
                    <UIcon :name="session.icon" class="size-5" />
                  </div>
                  <div class="min-w-0 flex-1">
                    <p class="truncate text-sm font-medium text-highlighted">{{ session.device }}</p>
                    <p class="mt-1 text-xs text-muted">{{ session.location }} · {{ session.date }}</p>
                  </div>
                  <UBadge v-if="session.current" color="primary" variant="subtle" size="sm">Cet appareil</UBadge>
                </div>
              </div>
            </section>
          </template>

          <template v-else-if="activeSection === 'activity'">
            <div class="mb-6">
              <h2 class="text-2xl font-semibold tracking-tight text-highlighted">Historique des connexions</h2>
              <p class="mt-2 text-sm text-muted">Les appareils ayant récemment accédé à votre compte.</p>
            </div>
            <div class="divide-y divide-default/70 border-y border-default/60">
              <div v-for="session in sessions" :key="session.device" class="flex items-center gap-4 py-5">
                <div class="flex size-11 shrink-0 items-center justify-center rounded-xl border border-default/70 bg-default/70 text-primary">
                  <UIcon :name="session.icon" class="size-5" />
                </div>
                <div class="min-w-0 flex-1">
                  <p class="truncate font-medium text-highlighted">{{ session.device }}</p>
                  <p class="mt-1 text-sm text-muted">{{ session.location }} · {{ session.date }}</p>
                </div>
                <UBadge v-if="session.current" color="primary" variant="subtle">Session actuelle</UBadge>
                <UButton v-else icon="i-lucide-ellipsis" color="neutral" variant="ghost" aria-label="Options de session" @click="showDemoNotice" />
              </div>
            </div>
          </template>

          <template v-else>
            <div class="mb-6">
              <h2 class="text-2xl font-semibold tracking-tight text-highlighted">Sécurité du compte</h2>
              <p class="mt-2 text-sm text-muted">Quelques vérifications pour mieux protéger votre espace.</p>
            </div>
            <div class="divide-y divide-default/70 border-y border-default/60">
              <div class="flex items-center gap-4 py-5">
                <div class="flex size-10 shrink-0 items-center justify-center rounded-lg bg-primary/10 text-primary"><UIcon name="i-lucide-lock-keyhole" class="size-5" /></div>
                <div class="min-w-0 flex-1">
                  <p class="text-sm font-medium text-highlighted">Mot de passe</p>
                  <p class="mt-1 text-xs text-muted">Modifiez-le régulièrement et choisissez-en un unique.</p>
                </div>
                <UButton label="Modifier" color="neutral" variant="outline" size="sm" @click="showDemoNotice" />
              </div>
              <div class="flex items-center gap-4 py-5">
                <div class="flex size-10 shrink-0 items-center justify-center rounded-lg bg-amber-500/10 text-amber-600 dark:text-amber-400"><UIcon name="i-lucide-shield-alert" class="size-5" /></div>
                <div class="min-w-0 flex-1">
                  <p class="text-sm font-medium text-highlighted">Double authentification</p>
                  <p class="mt-1 text-xs text-muted">Ajoutez une étape de vérification à la connexion.</p>
                </div>
                <UButton label="Configurer" color="primary" variant="subtle" size="sm" @click="showDemoNotice" />
              </div>
              <div class="flex items-center gap-4 py-5">
                <div class="flex size-10 shrink-0 items-center justify-center rounded-lg bg-primary/10 text-primary"><UIcon name="i-lucide-mail-check" class="size-5" /></div>
                <div class="min-w-0 flex-1">
                  <p class="text-sm font-medium text-highlighted">Adresse e-mail</p>
                  <p class="mt-1 text-xs text-muted">camille@exemple.com</p>
                </div>
                <UBadge color="primary" variant="subtle">Vérifiée</UBadge>
              </div>
            </div>
          </template>

          <p class="mt-8 text-xs leading-5 text-muted">
            Les informations affichées sont des données de démonstration et ne correspondent pas à un compte réel.
          </p>
        </section>
      </div>
    </UContainer>
  </main>
</template>