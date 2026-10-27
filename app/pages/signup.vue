<script setup lang="ts">
const toast = useToast();

useHead({
  title: "Créer un compte",
});

const form = reactive({
  nom: "",
  prenom: "",
  numero: "",
  email: "",
  password: "",
});

function submitSignup() {
  toast.add({
    title: "Inscription non configurée",
    description: "Le service d’authentification doit être connecté pour créer votre compte.",
    color: "warning",
  });
}

function signupWithGoogle() {
  toast.add({
    title: "Google non configuré",
    description: "La connexion Google nécessite une configuration OAuth.",
    color: "warning",
  });
}
</script>

<template>
  <main class="relative isolate flex min-h-[calc(100dvh-4rem)] items-center justify-center overflow-hidden px-4 py-10 sm:px-6">
    <Background variant="bubble" :speed="1.15" />
    <div aria-hidden="true" class="pointer-events-none absolute inset-0 bg-linear-to-b from-default/10 via-transparent to-default/60" />

    <UContainer class="relative z-10 flex w-full max-w-lg flex-col items-center">
      <div class="mb-7 text-center">
        <div class="mx-auto mb-4 flex size-12 items-center justify-center rounded-2xl border border-primary/20 bg-primary/10 text-primary shadow-lg shadow-primary/10">
          <UIcon name="i-lucide-user-round-plus" class="size-6" />
        </div>
        <p class="text-xs font-semibold uppercase tracking-[0.18em] text-primary">OrssiAuth · Votre espace personnel</p>
        <h1 class="mt-3 text-3xl font-semibold tracking-tight text-highlighted sm:text-4xl">Créez votre compte.</h1>
        <p class="mt-2 text-sm leading-6 text-muted">Quelques informations pour commencer.</p>
      </div>

      <UCard class="w-full border border-default/70 bg-default/85 shadow-2xl shadow-primary/10 backdrop-blur-xl" :ui="{ body: 'p-6 sm:p-8' }">
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

        <form class="space-y-4" @submit.prevent="submitSignup">
          <div class="grid gap-4 sm:grid-cols-2">
            <UFormField label="Prénom" name="prenom">
              <UInput
                v-model="form.prenom"
                name="given-name"
                autocomplete="given-name"
                placeholder="Camille"
                icon="i-lucide-user-round"
                size="lg"
                class="w-full"
                required
              />
            </UFormField>

            <UFormField label="Nom" name="nom">
              <UInput
                v-model="form.nom"
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

          <UFormField label="Numéro de téléphone" name="numero">
            <UInput
              v-model="form.numero"
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

          <UFormField label="Adresse e-mail" name="email">
            <UInput
              v-model="form.email"
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

          <UFormField label="Mot de passe" name="password" hint="8 caractères minimum">
            <UInput
              v-model="form.password"
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

          <UButton type="submit" label="Créer mon compte" trailing-icon="i-lucide-arrow-right" color="primary" size="lg" block class="mt-6! shadow-lg shadow-primary/20" />
        </form>

        <div class="mt-6 border-t border-default/70 pt-5 text-center text-sm text-muted">
          Vous avez déjà un compte ?
          <NuxtLink to="/login" class="font-semibold text-primary hover:underline">Se connecter</NuxtLink>
        </div>
      </UCard>

      <NuxtLink to="/" class="mt-5 inline-flex items-center gap-2 text-sm text-muted transition-colors hover:text-highlighted">
        <UIcon name="i-lucide-arrow-left" class="size-4" />
        Retour à l’accueil
      </NuxtLink>
    </UContainer>
  </main>
</template>