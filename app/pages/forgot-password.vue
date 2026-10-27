<script setup lang="ts">
const toast = useToast();

useHead({
  title: "Mot de passe oublié",
});

const email = ref("");

function submitReset() {
  email.value = email.value.trim();

  toast.add({
    title: "Réinitialisation non configurée",
    description:
      "Le service d’authentification doit être connecté pour envoyer un lien de réinitialisation.",
    color: "warning",
    icon: "i-lucide-info",
  });
}
</script>

<template>
  <main
    class="relative isolate flex min-h-[calc(100dvh-4rem)] items-center justify-center overflow-hidden px-4 py-12 sm:px-6"
  >
    <Background variant="bubble" :speed="1.2" />
    <div
      aria-hidden="true"
      class="pointer-events-none absolute inset-0 bg-linear-to-b from-default/10 via-transparent to-default/60"
    />

    <UContainer class="relative z-10 w-full max-w-md">
      <div class="mb-8 text-center">
        <div
          class="mx-auto mb-5 flex size-14 items-center justify-center rounded-2xl border border-primary/20 bg-primary/10 text-primary shadow-lg shadow-primary/10"
        >
          <UIcon name="i-lucide-key-round" class="size-7" />
        </div>
        <p
          class="text-xs font-semibold uppercase tracking-[0.18em] text-primary"
        >
          OrssiAuth · Votre espace personnel
        </p>
        <h1
          class="mt-3 text-3xl font-semibold tracking-tight text-highlighted sm:text-4xl"
        >
          Réinitialiser le mot de passe.
        </h1>
        <p class="mt-3 text-sm leading-6 text-muted">
          Saisissez l’adresse e-mail associée à votre compte.
        </p>
      </div>

      <UCard
        class="w-full border border-default/70 bg-default/85 shadow-2xl shadow-primary/10 backdrop-blur-xl"
        :ui="{ body: 'p-6 sm:p-8' }"
      >
        <form class="space-y-5" @submit.prevent="submitReset">
          <UFormField label="Adresse e-mail" name="email">
            <UInput
              v-model="email"
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

          <UButton
            type="submit"
            label="Envoyer le lien"
            trailing-icon="i-lucide-arrow-right"
            color="primary"
            size="xl"
            block
            class="rounded-xl shadow-lg shadow-primary/20"
          />
        </form>

        <div class="mt-6 border-t border-default/70 pt-5 text-center">
          <NuxtLink
            to="/login"
            class="inline-flex items-center gap-2 text-sm font-medium text-primary underline-offset-4 hover:underline"
          >
            <UIcon name="i-lucide-arrow-left" class="size-4" />
            Retour à la connexion
          </NuxtLink>
        </div>
      </UCard>
    </UContainer>
  </main>
</template>