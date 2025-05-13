<script setup lang="ts">
import { ref } from 'vue'
import { useMotion } from '@vueuse/motion'
import { useIntersectionObserver } from '@vueuse/core'

const testimonialRef = ref(null)
const isVisible = ref(false)

const { motion } = useMotion(testimonialRef, {
  initial: {
    opacity: 0,
    scale: 0.9,
    y: 20,
  },
  enter: {
    opacity: 1,
    scale: 1,
    y: 0,
    transition: {
      duration: 800,
      ease: 'cubic-bezier(0.4, 0, 0.2, 1)',
    },
  },
})

useIntersectionObserver(testimonialRef, ([{ isIntersecting }]) => {
  if (isIntersecting) {
    isVisible.value = true
  }
})

defineProps({
  quote: {
    type: String,
    required: true
  },
  author: {
    type: String,
    required: true
  },
  position: {
    type: String,
    default: ''
  },
  company: {
    type: String,
    default: ''
  },
  avatar: {
    type: String,
    default: 'https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg?auto=compress&cs=tinysrgb&w=300'
  }
})
</script>

<template>
  <div ref="testimonialRef" class="testimonial" :class="{ 'visible': isVisible }">
    <div class="quote-mark">"</div>
    <p class="quote">{{ quote }}</p>
    <div class="author-info">
      <div class="avatar">
        <img :src="avatar" alt="Author avatar" />
      </div>
      <div class="author-details">
        <p class="author-name">{{ author }}</p>
        <p class="author-position" v-if="position || company">
          {{ position }}
          <span v-if="position && company">, </span>
          {{ company }}
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.testimonial {
  position: relative;
  background-color: white;
  border-radius: 12px;
  padding: 32px;
  padding-top: 48px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
  border: 1px solid transparent;
}

.testimonial:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  border-color: var(--primary-light);
}

.quote-mark {
  position: absolute;
  top: 12px;
  left: 20px;
  font-size: 80px;
  line-height: 1;
  font-family: Georgia, serif;
  background: linear-gradient(135deg, var(--primary), var(--secondary));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  opacity: 0.8;
  user-select: none;
  transition: transform 0.3s ease;
}

.testimonial:hover .quote-mark {
  transform: scale(1.1) rotate(-5deg);
}

.quote {
  font-size: 16px;
  line-height: 1.6;
  color: var(--neutral-700);
  font-style: italic;
  margin-bottom: 24px;
  position: relative;
  z-index: 1;
  transition: color 0.3s ease;
}

.author-info {
  display: flex;
  align-items: center;
  transition: transform 0.3s ease;
}

.testimonial:hover .author-info {
  transform: scale(1.02);
}

.avatar {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  overflow: hidden;
  margin-right: 16px;
  flex-shrink: 0;
  border: 2px solid transparent;
  transition: all 0.3s ease;
}

.testimonial:hover .avatar {
  border-color: var(--primary);
  transform: rotate(5deg);
}

.avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.testimonial:hover .avatar img {
  transform: scale(1.1);
}

.author-name {
  font-weight: 600;
  font-size: 16px;
  color: var(--neutral-800);
  margin: 0;
  transition: color 0.3s ease;
}

.author-position {
  font-size: 14px;
  color: var(--neutral-500);
  margin: 4px 0 0 0;
  transition: color 0.3s ease;
}

/* Dark mode styles */
:global(.dark-mode) .testimonial {
  background-color: var(--neutral-800);
  border-color: var(--neutral-700);
}

:global(.dark-mode) .testimonial:hover {
  border-color: var(--primary);
}

:global(.dark-mode) .quote {
  color: var(--neutral-200);
}

:global(.dark-mode) .author-name {
  color: var(--neutral-100);
}

:global(.dark-mode) .author-position {
  color: var(--neutral-400);
}
</style>