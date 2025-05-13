<template>
  <div class="slidev-layout section">
    <div class="section-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.slidev-layout.section {
  @apply h-full flex items-center justify-center;
  background-color: var(--primary);
  color: white;
}

.section-content {
  @apply text-center;
}

/* Dark mode adjustments */
:global(.dark-mode) .slidev-layout.section {
  background-color: var(--primary-dark);
}
</style>