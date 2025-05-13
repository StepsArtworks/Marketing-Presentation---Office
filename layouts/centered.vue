<template>
  <div class="slidev-layout centered">
    <div class="centered-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.slidev-layout.centered {
  @apply h-full flex items-center justify-center;
  background-color: white;
}

.centered-content {
  @apply text-center max-w-3xl mx-auto px-8;
}

/* Dark mode adjustments */
:global(.dark-mode) .slidev-layout.centered {
  background-color: var(--neutral-900);
  color: var(--neutral-100);
}
</style>