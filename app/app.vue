<script setup lang="ts">
const route = useRoute()

const description
  = 'OrssiAuth simplifie l\'accès à votre espace personnel. Créez votre compte ou connectez-vous grâce à une interface claire et intuitive.'

useHead({
  title: 'Accéder à votre espace',
  titleTemplate: '%s · OrssiAuth',

  meta: [
    {
      name: 'viewport',
      content: 'width=device-width, initial-scale=1'
    },
    {
      name: 'theme-color',
      content: '#6366f1'
    }
  ],

  link: [
    {
      rel: 'icon',
      href: '/favicon.ico'
    }
  ],

  htmlAttrs: {
    lang: 'fr'
  }
})

useSeoMeta({
  description,
  ogTitle: 'OrssiAuth — Votre espace personnel',
  ogDescription: description,
  ogType: 'website',
  twitterCard: 'summary'
})
</script>

<template>
  <UApp>
    <UHeader>
      <!-- Identité de la plateforme -->
      <template #title>
        <span class="inline-flex items-center gap-3">
          <span
            class="flex size-10 items-center justify-center rounded-xl bg-primary text-white shadow-lg shadow-primary/20"
          >
            <UIcon
              name="i-lucide-shield-check"
              class="size-6"
            />
          </span>

          <span class="flex flex-col">
            <span class="text-lg font-bold tracking-tight text-highlighted">
              OrssiAuth
            </span>

            <span class="hidden text-xs text-muted sm:block">
              Votre espace personnel
            </span>
          </span>
        </span>
      </template>

      <!-- Navigation -->
      <template #right>
        <UColorModeButton
          size="lg"
          variant="ghost"
          aria-label="Changer le thème"
        />

        <!-- Sur la page de connexion -->
        <template v-if="route.path === '/login'">
          <UButton
            to="/signup"
            label="Créer un compte"
            icon="i-lucide-user-round-plus"
            color="primary"
            variant="solid"
            size="lg"
            class="hidden sm:inline-flex"
          />

          <UButton
            to="/signup"
            icon="i-lucide-user-round-plus"
            color="primary"
            variant="solid"
            size="lg"
            class="sm:hidden"
            aria-label="Créer un compte"
          />
        </template>

        <!-- Sur la page d'inscription -->
        <template v-else-if="route.path === '/signup'">
          <UButton
            to="/login"
            label="Se connecter"
            icon="i-lucide-log-in"
            color="neutral"
            variant="outline"
            size="lg"
          />
        </template>

        <!-- Sur le tableau de bord -->
        <template v-else-if="route.path === '/dashboard'">
          <UButton
            to="/"
            label="Quitter l’aperçu"
            icon="i-lucide-arrow-up-left"
            color="neutral"
            variant="outline"
            size="lg"
          />
        </template>

        <!-- Sur les autres pages -->
        <template v-else>
          <UButton
            to="/login"
            label="Connexion"
            icon="i-lucide-log-in"
            color="neutral"
            variant="outline"
            size="lg"
            class="hidden sm:inline-flex"
          />

          <UButton
            to="/signup"
            label="Créer un compte"
            icon="i-lucide-user-round-plus"
            color="primary"
            variant="solid"
            size="lg"
          />
        </template>
      </template>
    </UHeader>

    <!-- Pages de l'application -->
    <NuxtPage />
  </UApp>
</template>
