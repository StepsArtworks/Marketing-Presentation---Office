<template>
  <div class="slidev-layout image-left">
    <div class="content-container">
      <div class="image">
        <slot name="image">
          <img src="https://images.pexels.com/photos/3182812/pexels-photo-3182812.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1" alt="Default image" />
        </slot>
      </div>
      <div class="content">
        <slot />
      </div>
    </div>
  </div>
</template>

<style scoped>
.slidev-layout.image-left {
  @apply h-full;
  background-color: white;
}

.content-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  height: 100%;
}

.content {
  @apply p-8 flex flex-col justify-center;
}

.image {
  @apply overflow-hidden;
}

.image img {
  @apply h-full w-full object-cover;
}

/* Dark mode adjustments */
:global(.dark-mode) .slidev-layout.image-left {
  background-color: var(--neutral-900);
  color: var(--neutral-100);
}
</style>