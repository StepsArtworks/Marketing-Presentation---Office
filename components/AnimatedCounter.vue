<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import gsap from 'gsap'

const props = defineProps({
  target: {
    type: Number,
    required: true
  },
  duration: {
    type: Number,
    default: 2000
  },
  prefix: {
    type: String,
    default: ''
  },
  suffix: {
    type: String,
    default: ''
  },
  startOnView: {
    type: Boolean,
    default: true
  }
})

const count = ref(0)
const animationStarted = ref(false)
const counterRef = ref(null)

const startAnimation = () => {
  if (animationStarted.value) return
  
  animationStarted.value = true
  
  gsap.to(count, {
    value: props.target,
    duration: props.duration / 1000,
    ease: "power2.out",
    onUpdate: () => {
      count.value = Math.floor(count.value)
    }
  })
}

const observeElement = () => {
  if (!props.startOnView || !counterRef.value) return
  
  const observer = new IntersectionObserver((entries) => {
    if (entries[0].isIntersecting) {
      startAnimation()
      observer.disconnect()
    }
  }, { threshold: 0.1 })
  
  observer.observe(counterRef.value)
}

onMounted(() => {
  if (props.startOnView) {
    observeElement()
  } else {
    startAnimation()
  }
})

watch(() => props.target, () => {
  animationStarted.value = false
  count.value = 0
  if (props.startOnView) {
    observeElement()
  } else {
    startAnimation()
  }
})
</script>

<template>
  <div ref="counterRef" class="animated-counter">
    <span class="prefix">{{ prefix }}</span>
    <span class="count">{{ count }}</span>
    <span class="suffix">{{ suffix }}</span>
  </div>
</template>

<style scoped>
.animated-counter {
  font-size: 3rem;
  font-weight: 700;
  color: var(--primary);
  display: inline-flex;
  align-items: center;
  background: linear-gradient(45deg, var(--primary), var(--secondary));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  transition: transform 0.3s ease;
}

.animated-counter:hover {
  transform: scale(1.05);
}

.prefix, .suffix {
  font-size: 2rem;
  color: var(--neutral-600);
  margin: 0 4px;
  -webkit-text-fill-color: var(--neutral-600);
}

:global(.dark-mode) .prefix, :global(.dark-mode) .suffix {
  color: var(--neutral-400);
  -webkit-text-fill-color: var(--neutral-400);
}
</style>