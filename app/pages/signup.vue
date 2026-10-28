<script setup lang="ts">
import * as z from 'zod'
import type { FormSubmitEvent } from '@nuxt/ui'

const toast = useToast()
const supabase = useSupabaseClient()

useHead({
  title: 'Créer un compte'
})

const loading = ref(false)
const errorMessage = ref('')

const schema = z.object({
  nom: z.string().trim().min(1, 'Le nom est requis'),
  prenom: z.string().trim().min(1, 'Le prénom est requis'),
  numero: z.string().trim().min(1, 'Le numéro de téléphone est requis'),
  email: z.email('Entre une adresse e-mail valide'),
  password: z
    .string()
    .min(8, 'Le mot de passe doit contenir au moins 8 caractères')
})

type Schema = z.output<typeof schema>

const state = reactive<Schema>({
  nom: '',
  prenom: '',
  numero: '',
  email: '',
  password: ''
})

async function submitSignup(event: FormSubmitEvent<Schema>) {
  loading.value = true
  errorMessage.value = ''

  try {
    const { data, error } = await supabase.auth.signUp({
      email: event.data.email,
      password: event.data.password,
      options: {
        data: {
          nom: event.data.nom,
          prenom: event.data.prenom,
          numero: event.data.numero
        }
      }
    })

    if (error) {
      errorMessage.value = error.message

      toast.add({
        title: 'Inscription impossible',
        description: error.message,
        color: 'error'
      })

      return
    }

    if (data.user && !data.session) {
      toast.add({
        title: 'Vérifie ta boîte mail',
        description:
          'Ton compte a été créé. Vérifie ta boîte mail pour confirmer ton adresse e-mail et continuer.',
        color: 'success'
      })

      return
    }

    if (data.session) {
      toast.add({
        title: 'Compte créé',
        description: 'Tu es maintenant connecté.',
        color: 'success'
      })
      await navigateTo('/dashboard')
    }
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

function signupWithGoogle() {
  toast.add({
    title: 'Google non configuré',
    description:
      'La connexion Google nécessite une configuration OAuth dans Supabase.',
    color: 'warning'
  })
}
</script>

<template>
  <main
    class="relative isolate flex min-h-[calc(100dvh-4rem)] items-center justify-center overflow-hidden px-4 py-10 sm:px-6"
  >
    <Background
      variant="bubble"
      :speed="1.15"
    />
    <div
      aria-hidden="true"
      class="pointer-events-none absolute inset-0 bg-linear-to-b from-default/10 via-transparent to-default/60"
    />

    <UContainer
      class="relative z-10 flex w-full max-w-lg flex-col items-center"
    >
      <div class="mb-7 text-center">
        <div
          class="mx-auto mb-4 flex size-12 items-center justify-center rounded-2xl border border-primary/20 bg-primary/10 text-primary shadow-lg shadow-primary/10"
        >
          <UIcon
            name="i-lucide-user-round-plus"
            class="size-6"
          />
        </div>
        <p
          class="text-xs font-semibold uppercase tracking-[0.18em] text-primary"
        >
          OrssiAuth · Votre espace personnel
        </p>
        <h1
          class="mt-3 text-3xl font-semibold tracking-tight text-highlighted sm:text-4xl"
        >
          Créez votre compte.
        </h1>
        <p class="mt-2 text-sm leading-6 text-muted">
          Quelques informations pour commencer.
        </p>
      </div>

      <UCard
        class="w-full border border-default/70 bg-default/85 shadow-2xl shadow-primary/10 backdrop-blur-xl"
        :ui="{ body: 'p-6 sm:p-8' }"
      >
        <UButton
          type="button"
          label="Continuer avec Google"
          icon="i-simple-icons-google"
          color="neutral"
          variant="outline"
          size="lg"
          block
          @click="signupWithGoogle"
        />

        <div class="my-6 flex items-center gap-4 text-xs text-muted">
          <span class="h-px flex-1 bg-default" />
          <span>ou avec votre e-mail</span>
          <span class="h-px flex-1 bg-default" />
        </div>

        <UForm
          :state="state"
          :schema="schema"
          class="space-y-4"
          @submit.prevent="submitSignup"
        >
          <div class="grid gap-4 sm:grid-cols-2">
            <UFormField
              label="Prénom"
              name="prenom"
            >
              <UInput
                v-model="state.prenom"
                name="given-name"
                autocomplete="given-name"
                placeholder="Camille"
                icon="i-lucide-user-round"
                size="lg"
                class="w-full"
                required
              />
            </UFormField>

            <UFormField
              label="Nom"
              name="nom"
            >
              <UInput
                v-model="state.nom"
                name="family-name"
                autocomplete="family-name"
                placeholder="Martin"
                icon="i-lucide-user-round"
                size="lg"
                class="w-full"
                required
              />
            </UFormField>
          </div>

          <UFormField
            label="Numéro de téléphone"
            name="numero"
          >
            <UInput
              v-model="state.numero"
              type="tel"
              name="tel"
              autocomplete="tel"
              placeholder="06 12 34 56 78"
              icon="i-lucide-phone"
              size="lg"
              class="w-full"
              required
            />
          </UFormField>

          <UFormField
            label="Adresse e-mail"
            name="email"
          >
            <UInput
              v-model="state.email"
              type="email"
              name="email"
              autocomplete="email"
              placeholder="vous@exemple.com"
              icon="i-lucide-mail"
              size="lg"
              class="w-full"
              required
            />
          </UFormField>

          <UFormField
            label="Mot de passe"
            name="password"
            hint="8 caractères minimum"
          >
            <UInput
              v-model="state.password"
              type="password"
              name="new-password"
              autocomplete="new-password"
              placeholder="Choisissez un mot de passe"
              icon="i-lucide-lock-keyhole"
              size="lg"
              class="w-full"
              minlength="8"
              required
            />
          </UFormField>

          <UAlert
            v-if="errorMessage"
            color="error"
            variant="subtle"
            title="Inscription impossible"
            :description="errorMessage"
            :loading="loading"
            :disabled="loading"
          />

          <UButton
            type="submit"
            label="Créer mon compte"
            trailing-icon="i-lucide-arrow-right"
            color="primary"
            size="lg"
            block
            :loading="loading"
            :disabled="loading"
            class="mt-6! shadow-lg shadow-primary/20"
          />
        </UForm>

        <div
          class="mt-6 border-t border-default/70 pt-5 text-center text-sm text-muted"
        >
          Vous avez déjà un compte ?
          <NuxtLink
            to="/login"
            class="font-semibold text-primary hover:underline"
          >Se connecter</NuxtLink>
        </div>
      </UCard>

      <NuxtLink
        to="/"
        class="mt-5 inline-flex items-center gap-2 text-sm text-muted transition-colors hover:text-highlighted"
      >
        <UIcon
          name="i-lucide-arrow-left"
          class="size-4"
        />
        Retour à l’accueil
      </NuxtLink>
    </UContainer>
  </main>
</template>
