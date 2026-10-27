<template>
  <NuxtLink
    to="/"
    aria-label="OrssiAuth, accueil"
    class="group inline-flex items-center gap-3"
  >
    <span
      class="flex size-10 items-center justify-center rounded-2xl bg-primary text-white shadow-lg shadow-primary/20 transition-transform duration-300 group-hover:rotate-3 group-hover:scale-105"
    >
      <UIcon name="i-lucide-shield-check" class="size-6" />
    </span>

    <span class="flex flex-col">
      <span class="text-lg font-bold tracking-tight text-highlighted">
        OrssiAuth
      </span>
      <span class="text-xs text-muted"> Votre espace, simplement. </span>
    </span>
  </NuxtLink>
</template>
