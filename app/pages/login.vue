<script setup lang="ts">
const toast = useToast()
const supabase = useSupabaseClient()

const showPassword = ref(false)
const loading = ref(false)
const errorMessage = ref('')

const form = reactive({
  email: '',
  password: ''
})

const benefits = [
  {
    icon: 'i-lucide-layout-dashboard',
    title: 'Un espace centralisé',
    description: 'Retrouvez votre espace personnel au même endroit.'
  },
  {
    icon: 'i-lucide-zap',
    title: 'Un parcours simplifié',
    description: 'Une interface claire pour accéder à votre compte.'
  },
  {
    icon: 'i-lucide-monitor-smartphone',
    title: 'Sur tous vos écrans',
    description: 'Une expérience adaptée au mobile et à l’ordinateur.'
  }
]

useHead({
  title: 'Se connecter',
  meta: [
    {
      name: 'description',
      content: 'Connectez-vous à votre espace personnel OrssiAuth.'
    }
  ]
})

async function submitLogin() {
  loading.value = true
  errorMessage.value = ''

  try {
    const { error } = await supabase.auth.signInWithPassword({
      email: form.email.trim(),
      password: form.password
    })

    if (error) {
      errorMessage.value = error.message
      toast.add({
        title: 'Connexion impossible',
        description: error.message,
        color: 'error'
      })
      return
    }

    await navigateTo('/dashboard')
  } catch {
    errorMessage.value = 'Une erreur inattendue est survenue.'
    toast.add({
      title: 'Erreur',
      description: errorMessage.value,
      color: 'error'
    })
  } finally {
    loading.value = false
  }
}

function loginWithGoogle() {
  toast.add({
    title: 'Google non configuré',
    description: 'La connexion Google nécessite une configuration OAuth.',
    color: 'warning',
    icon: 'i-lucide-info'
  })
}
</script>

<template>
  <main
    class="relative isolate flex min-h-[calc(100dvh-4rem)] items-center overflow-hidden py-10 sm:py-14"
  >
    <!-- Arrière-plan animé -->
    <Background
      variant="particles"
      :speed="1.3"
    />

    <!-- Améliore le contraste du contenu -->
    <div
      aria-hidden="true"
      class="pointer-events-none absolute inset-0 bg-gradient-to-b from-default/10 via-default/5 to-default/60"
    />

    <UContainer class="relative z-10 w-full">
      <div
        class="mx-auto grid w-full max-w-6xl items-center gap-10 lg:grid-cols-2 lg:gap-20"
      >
        <!-- Présentation sur ordinateur -->
        <section
          aria-labelledby="login-intro"
          class="hidden lg:block"
        >
          <UBadge
            color="primary"
            variant="subtle"
            size="lg"
            class="gap-2 rounded-full px-3 py-1.5"
          >
            <UIcon
              name="i-lucide-sparkles"
              class="size-4"
            />
            Votre espace personnel
          </UBadge>

          <h1
            id="login-intro"
            class="mt-7 max-w-xl text-5xl font-semibold tracking-tight text-highlighted xl:text-6xl"
          >
            Heureux de vous revoir.
            <span
              class="mt-2 block bg-gradient-to-r from-primary via-indigo-400 to-cyan-400 bg-clip-text text-transparent"
            >
              Reprenez là où vous en étiez.
            </span>
          </h1>

          <p class="mt-6 max-w-lg text-base leading-8 text-muted xl:text-lg">
            Connectez-vous à OrssiAuth pour retrouver votre espace personnel,
            avec une interface conçue pour rester simple et agréable à utiliser.
          </p>
        </section>

        <!-- Formulaire de connexion -->
        <section class="mx-auto w-full max-w-md">
          <!-- Présentation sur mobile -->
          <div class="mb-7 text-center lg:hidden">
            <div
              class="mx-auto mb-4 flex size-14 items-center justify-center rounded-2xl border border-primary/20 bg-primary/10 text-primary shadow-lg shadow-primary/10"
            >
              <UIcon
                name="i-lucide-shield-check"
                class="size-7"
              />
            </div>

            <p
              class="text-xs font-semibold uppercase tracking-[0.18em] text-primary"
            >
              OrssiAuth
            </p>

            <h1
              class="mt-2 text-3xl font-semibold tracking-tight text-highlighted"
            >
              Heureux de vous revoir.
            </h1>

            <p class="mt-2 text-sm leading-6 text-muted">
              Retrouvez votre espace personnel.
            </p>
          </div>

          <UCard
            class="login-card w-full border border-default/70 bg-default/85 shadow-2xl shadow-primary/10 backdrop-blur-2xl"
            :ui="{
              body: 'p-6 sm:p-8'
            }"
          >
            <!-- En-tête du formulaire -->
            <div class="mb-8">
              <div class="flex items-center justify-between gap-4">
                <div>
                  <h2
                    class="text-2xl font-semibold tracking-tight text-highlighted"
                  >
                    Se connecter
                  </h2>
                </div>
              </div>
            </div>

            <!-- Connexion avec Google -->
            <UButton
              type="button"
              color="neutral"
              variant="outline"
              size="xl"
              block
              class="rounded-xl border-default/80 bg-default/50 transition-colors hover:bg-elevated"
              @click="loginWithGoogle"
            >
              <template #leading>
                <UIcon
                  name="i-simple-icons-google"
                  class="size-5"
                />
              </template>

              Continuer avec Google
            </UButton>

            <!-- Séparateur -->
            <div class="relative my-6">
              <div class="absolute inset-0 flex items-center">
                <div class="w-full border-t border-default/70" />
              </div>

              <div class="relative flex justify-center">
                <span class="bg-default px-3 text-xs text-muted">
                  OU CONTINUER AVEC VOTRE E-MAIL
                </span>
              </div>
            </div>

            <UForm
              :state="form"
              class="space-y-5"
              @submit.prevent="submitLogin"
            >
              <!-- Adresse e-mail -->
              <UFormField
                label="Adresse e-mail"
                name="email"
                required
              >
                <UInput
                  v-model="form.email"
                  type="email"
                  name="email"
                  autocomplete="email"
                  placeholder="vous@exemple.com"
                  leading-icon="i-lucide-mail"
                  size="xl"
                  class="w-full"
                  required
                />
              </UFormField>

              <!-- Mot de passe -->
              <UFormField
                label="Mot de passe"
                name="password"
                required
              >
                <UInput
                  v-model="form.password"
                  :type="showPassword ? 'text' : 'password'"
                  name="password"
                  autocomplete="current-password"
                  placeholder="Votre mot de passe"
                  leading-icon="i-lucide-lock-keyhole"
                  size="xl"
                  class="w-full"
                  required
                >
                  <template #trailing>
                    <UButton
                      type="button"
                      color="neutral"
                      variant="ghost"
                      size="sm"
                      :icon="showPassword ? 'i-lucide-eye-off' : 'i-lucide-eye'"
                      :aria-label="
                        showPassword
                          ? 'Masquer le mot de passe'
                          : 'Afficher le mot de passe'
                      "
                      :aria-pressed="showPassword"
                      @click="showPassword = !showPassword"
                    />
                  </template>
                </UInput>
              </UFormField>

              <!-- Récupération du mot de passe -->
              <div class="-mt-1 flex justify-end">
                <NuxtLink
                  to="/forgot-password"
                  class="text-sm font-medium text-primary underline-offset-4 transition-colors hover:underline"
                >
                  Mot de passe oublié ?
                </NuxtLink>
              </div>
              <UAlert
                v-if="errorMessage"
                color="error"
                variant="subtle"
                title="Connexion impossible"
                :description="errorMessage"
              />
              <!-- Connexion -->
              <UButton
                type="submit"
                label="Se connecter"
                trailing-icon="i-lucide-arrow-right"
                color="primary"
                size="xl"
                block
                :loading="loading"
                :disabled="loading"
                class="rounded-xl shadow-lg shadow-primary/20 transition-transform duration-200 hover:-translate-y-0.5"
              />
            </UForm>

              <!-- Inscription -->
              <div class="mt-7 border-t border-default/70 pt-6">
                <p class="text-center text-sm text-muted">
                  Vous n’avez pas encore de compte ?
                </p>

                <UButton
                  to="/signup"
                  label="Créer un compte"
                  trailing-icon="i-lucide-user-round-plus"
                  color="neutral"
                  variant="outline"
                  size="xl"
                  block
                  class="mt-4 rounded-xl"
                />
              </div>
          </UCard>
        </section>
      </div>
    </UContainer>
  </main>
</template>

<style scoped>
.login-card {
  animation: login-appear 650ms cubic-bezier(0.2, 0.7, 0.2, 1) both;
}

@keyframes login-appear {
  from {
    opacity: 0;
    transform: translateY(14px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (prefers-reduced-motion: reduce) {
  .login-card {
    animation: none;
  }
}
</style>
