<script setup lang="ts">
import type { ButtonProps } from '@nuxt/ui'

const heroLinks: ButtonProps[] = [
  {
    label: 'Créer mon compte',
    to: '/signup',
    icon: 'i-lucide-user-round-plus',
    trailingIcon: 'i-lucide-arrow-up-right',
    size: 'xl',
    color: 'primary',
    variant: 'solid',
    class: 'w-full justify-center shadow-lg shadow-primary/20 sm:w-auto'
  },
  {
    label: 'Se connecter',
    to: '/login',
    icon: 'i-lucide-log-in',
    size: 'xl',
    color: 'neutral',
    variant: 'outline',
    class: 'w-full justify-center sm:w-auto'
  }
]

const features = [
  {
    icon: 'i-lucide-panels-top-left',
    title: 'Un espace centralisé',
    description: 'Un point d’entrée clair vers votre compte.'
  },
  {
    icon: 'i-lucide-mouse-pointer-click',
    title: 'Simple à utiliser',
    description: 'Un parcours pensé pour aller à l’essentiel.'
  },
  {
    icon: 'i-lucide-monitor-smartphone',
    title: 'Sur tous vos écrans',
    description: 'Une interface adaptée au mobile et au bureau.'
  }
]
</script>

<template>
  <div class="relative isolate flex min-h-dvh flex-col overflow-hidden">
    <!-- Arrière-plan animé -->
    <Background
      variant="particles"
      :speed="1.4"
    />

    <!-- Voile de contraste pour conserver une bonne lisibilité -->
    <div
      aria-hidden="true"
      class="pointer-events-none absolute inset-0 bg-gradient-to-b from-default/10 via-transparent to-default/60"
    />

    <!-- En-tête -->
    <header class="relative z-10">
      <UContainer
        class="flex h-20 items-center justify-between gap-4 border-b border-default/50"
      >
        <div class="hidden items-center gap-3 sm:flex">
          <span class="size-2 animate-pulse rounded-full bg-primary" />
          <span class="text-sm text-muted"> Bienvenue sur OrssiAuth </span>
        </div>

        <UButton
          to="/login"
          color="neutral"
          variant="ghost"
          size="sm"
          trailing-icon="i-lucide-arrow-up-right"
          class="sm:hidden"
        >
          Connexion
        </UButton>
      </UContainer>
    </header>

    <!-- Contenu principal -->
    <main class="relative z-10 flex flex-1 flex-col justify-center">
      <UPageHero
        headline="ORSSIAUTH · VOTRE ESPACE PERSONNEL"
        description="Créez votre compte ou connectez-vous à votre espace avec une expérience claire, fluide et pensée pour vous."
        :links="heroLinks"
        orientation="horizontal"
        :ui="{
          root: 'w-full',
          container:
            'grid-cols-1 gap-12 py-14 sm:py-16 lg:grid-cols-2 lg:items-center lg:gap-16 lg:py-20',
          wrapper: 'max-w-2xl',
          headline:
            'mb-5 text-xs font-semibold uppercase tracking-[0.18em] text-primary',
          title:
            'text-5xl font-semibold tracking-tight text-highlighted text-balance sm:text-6xl lg:text-7xl',
          description:
            'mt-6 max-w-xl text-base leading-8 text-muted sm:text-lg',
          footer: 'mt-8',
          links: 'flex flex-col gap-3 sm:flex-row sm:items-center'
        }"
      >
        <!-- Aperçu illustratif de l'interface -->
        <div class="relative mx-auto w-full max-w-md">
          <div
            aria-hidden="true"
            class="absolute -inset-8 rounded-[3rem] bg-gradient-to-br from-primary/20 via-indigo-500/10 to-cyan-400/15 blur-3xl"
          />

          <div
            class="preview-card relative overflow-hidden rounded-[2rem] border border-default/70 bg-default/80 p-6 shadow-2xl shadow-primary/10 backdrop-blur-2xl sm:p-8"
          >
            <!-- Décoration supérieure -->
            <div
              aria-hidden="true"
              class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-primary/70 to-transparent"
            />

            <div class="flex items-center justify-between gap-3">
              <div class="flex items-center gap-3">
                <div
                  class="flex size-11 items-center justify-center rounded-2xl bg-primary/10 text-primary ring-1 ring-primary/15"
                >
                  <UIcon
                    name="i-lucide-shield-check"
                    class="size-6"
                  />
                </div>

                <div>
                  <p class="font-semibold text-highlighted">
                    OrssiAuth
                  </p>
                  <p class="text-xs text-muted">
                    Espace de connexion
                  </p>
                </div>
              </div>

              <UBadge
                color="neutral"
                variant="subtle"
              >
                Aperçu
              </UBadge>
            </div>

            <div class="mt-8">
              <p class="text-2xl font-semibold tracking-tight text-highlighted">
                Heureux de vous revoir.
              </p>
              <p class="mt-2 text-sm leading-6 text-muted">
                Retrouvez votre espace en quelques étapes.
              </p>
            </div>

            <!-- Champ d'e-mail illustratif -->
            <div class="mt-7 space-y-2">
              <p class="text-sm font-medium text-highlighted">
                Adresse e-mail
              </p>

              <div
                class="flex h-12 items-center gap-3 rounded-xl border border-default bg-elevated/40 px-4"
              >
                <UIcon
                  name="i-lucide-mail"
                  class="size-4 shrink-0 text-muted"
                />
                <span class="text-sm text-muted"> vous@exemple.com </span>
              </div>
            </div>

            <!-- Champ de mot de passe illustratif -->
            <div class="mt-5 space-y-2">
              <div class="flex items-center justify-between gap-3">
                <p class="text-sm font-medium text-highlighted">
                  Mot de passe
                </p>
                <UIcon
                  name="i-lucide-lock-keyhole"
                  class="size-4 text-muted"
                />
              </div>

              <div
                class="flex h-12 items-center rounded-xl border border-default bg-elevated/40 px-4"
              >
                <span class="text-sm tracking-[0.3em] text-muted">
                  ••••••••••
                </span>
              </div>
            </div>

            <!-- Bouton purement décoratif de l'aperçu -->
            <div
              aria-hidden="true"
              class="mt-6 flex h-12 items-center justify-center gap-2 rounded-xl bg-primary px-4 font-medium text-white shadow-lg shadow-primary/20"
            >
              <span>Continuer</span>
              <UIcon
                name="i-lucide-arrow-right"
                class="size-4"
              />
            </div>

            <div
              class="mt-5 flex items-center justify-center gap-2 border-t border-default/60 pt-5 text-xs text-muted"
            >
              <UIcon
                name="i-lucide-sparkles"
                class="size-4 text-primary"
              />
              <span>Une expérience claire et intuitive</span>
            </div>
          </div>

          <!-- Petit élément flottant -->
          <div
            class="preview-float absolute -bottom-5 -left-3 hidden items-center gap-3 rounded-2xl border border-default/70 bg-default/90 px-4 py-3 shadow-xl backdrop-blur-xl sm:flex"
          >
            <div
              class="flex size-9 items-center justify-center rounded-xl bg-primary/10 text-primary"
            >
              <UIcon
                name="i-lucide-sparkles"
                class="size-5"
              />
            </div>

            <div>
              <p class="text-sm font-semibold text-highlighted">
                Tout commence ici
              </p>
              <p class="text-xs text-muted">
                Votre prochain accès
              </p>
            </div>
          </div>
        </div>

        <!-- Titre personnalisé -->
        <template #title>
          L'accès à votre espace,
          <span
            class="bg-gradient-to-r from-primary via-indigo-400 to-cyan-400 bg-clip-text text-transparent"
          >
            en toute simplicité.
          </span>
        </template>
      </UPageHero>

      <!-- Points forts -->
      <UContainer class="w-full pb-12 sm:pb-16">
        <div
          class="grid gap-6 border-y border-default/60 py-7 sm:grid-cols-3 sm:gap-8 sm:py-8"
        >
          <div
            v-for="feature in features"
            :key="feature.title"
            class="flex items-start gap-4"
          >
            <div
              class="flex size-10 shrink-0 items-center justify-center rounded-xl border border-default/70 bg-default/60 text-primary"
            >
              <UIcon
                :name="feature.icon"
                class="size-5"
              />
            </div>

            <div>
              <h2 class="font-semibold text-highlighted">
                {{ feature.title }}
              </h2>
              <p class="mt-1 text-sm leading-6 text-muted">
                {{ feature.description }}
              </p>
            </div>
          </div>
        </div>
      </UContainer>
    </main>

    <!-- Pied de page -->
    <footer class="relative z-10">
      <UContainer
        class="flex flex-col items-center justify-between gap-3 py-5 text-center text-xs text-muted sm:flex-row sm:text-left"
      >
        <p>© OrssiAuth. Tous droits réservés.</p>

        <p class="flex items-center gap-2">
          <UIcon
            name="i-lucide-sparkles"
            class="size-3.5"
          />
          Une expérience conçue pour aller à l’essentiel.
        </p>
      </UContainer>
    </footer>
  </div>
</template>

<style scoped>
.preview-card {
  animation: card-appear 800ms cubic-bezier(0.2, 0.7, 0.2, 1) both;
}

.preview-float {
  animation: floating 6s ease-in-out infinite;
}

@keyframes card-appear {
  from {
    opacity: 0;
    transform: translateY(18px) scale(0.985);
  }

  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

@keyframes floating {
  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-7px);
  }
}

@media (prefers-reduced-motion: reduce) {
  .preview-card,
  .preview-float {
    animation: none;
  }
}
</style>
