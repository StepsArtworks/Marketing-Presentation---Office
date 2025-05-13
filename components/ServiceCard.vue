<script setup lang="ts">
import { ref } from 'vue'
import { useMotion } from '@vueuse/motion'

const props = defineProps({
  title: {
    type: String,
    required: true
  },
  description: {
    type: String,
    required: true
  },
  icon: {
    type: String,
    default: 'star'
  },
  color: {
    type: String,
    default: 'primary'
  }
})

const cardRef = ref(null)
const hovered = ref(false)

const { motion } = useMotion(cardRef, {
  initial: {
    opacity: 0,
    y: 20,
  },
  enter: {
    opacity: 1,
    y: 0,
    transition: {
      duration: 600,
    },
  },
})

const colorMap = {
  primary: 'var(--primary)',
  secondary: 'var(--secondary)',
  accent: 'var(--accent)',
  success: 'var(--success)',
  warning: 'var(--warning)',
  error: 'var(--error)'
}

const getColor = () => {
  return colorMap[props.color] || colorMap.primary
}
</script>

<template>
  <div 
    ref="cardRef"
    class="tbr-service-card"
    @mouseenter="hovered = true"
    @mouseleave="hovered = false">
    <div class="icon-container" :style="{ backgroundColor: getColor() + '20' }">
      <div v-if="icon === 'star'" class="icon">★</div>
      <div v-else-if="icon === 'chart'" class="icon">📊</div>
      <div v-else-if="icon === 'bulb'" class="icon">💡</div>
      <div v-else-if="icon === 'growth'" class="icon">📈</div>
      <div v-else class="icon">⭐</div>
    </div>
    <h3 class="title">{{ title }}</h3>
    <p class="description">{{ description }}</p>
    <div 
      class="learn-more"
      :class="{ 'visible': hovered }">
      Learn more
      <span class="arrow">→</span>
    </div>
  </div>
</template>

<style scoped>
.tbr-service-card {
  background-color: white;
  border-radius: 12px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  padding: 24px;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  height: 100%;
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
  border: 1px solid transparent;
}

.tbr-service-card:hover {
  transform: translateY(-4px) scale(1.02);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  border-color: var(--primary-light);
}

.icon-container {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 16px;
  transition: transform 0.3s ease;
}

.tbr-service-card:hover .icon-container {
  transform: scale(1.1);
}

.icon {
  font-size: 24px;
}

.title {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 8px;
  color: var(--neutral-800);
  transition: color 0.3s ease;
}

.description {
  font-size: 14px;
  line-height: 1.5;
  color: var(--neutral-600);
  flex-grow: 1;
}

.learn-more {
  display: flex;
  align-items: center;
  font-size: 14px;
  font-weight: 500;
  color: var(--primary);
  margin-top: 16px;
  opacity: 0;
  transform: translateY(8px);
  transition: all 0.3s ease;
}

.learn-more.visible {
  opacity: 1;
  transform: translateY(0);
}

.arrow {
  margin-left: 4px;
  transition: transform 0.2s ease;
}

.tbr-service-card:hover .arrow {
  transform: translateX(4px);
}

:global(.dark-mode) .tbr-service-card {
  background-color: var(--neutral-800);
  border-color: var(--neutral-700);
}

:global(.dark-mode) .tbr-service-card:hover {
  border-color: var(--primary);
}

:global(.dark-mode) .title {
  color: var(--neutral-100);
}

:global(.dark-mode) .description {
  color: var(--neutral-300);
}
</style>