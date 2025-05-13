<template>
  <div class="slidev-layout cover">
    <div class="cover-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.slidev-layout.cover {
  @apply h-full grid;
  background-image: linear-gradient(to right bottom, rgba(0, 122, 255, 0.8), rgba(88, 86, 214, 0.9)), url('https://images.pexels.com/photos/3183150/pexels-photo-3183150.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1');
  background-size: cover;
  background-position: center;
  color: white;
}

.cover-content {
  @apply flex flex-col items-center justify-center text-center;
}

/* Dark mode adjustments */
:global(.dark-mode) .slidev-layout.cover {
  background-image: linear-gradient(to right bottom, rgba(10, 132, 255, 0.8), rgba(94, 92, 230, 0.9)), url('https://images.pexels.com/photos/3183150/pexels-photo-3183150.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1');
}
</style>