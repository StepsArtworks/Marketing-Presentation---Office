<script setup lang="ts">
import { ref } from 'vue'
import { useMotion } from '@vueuse/motion'
import { KinesisContainer, KinesisElement } from 'vue-kinesis'

const props = defineProps({
  features: {
    type: Array,
    default: () => []
  }
})

const activeIndex = ref(0)
const containerRef = ref(null)

const { motion } = useMotion(containerRef, {
  initial: { opacity: 0, y: 40 },
  enter: {
    opacity: 1,
    y: 0,
    transition: { duration: 800, ease: 'easeOut' }
  }
})

const setActive = (index) => {
  activeIndex.value = index
}
</script>

<template>
  <div class="feature-list-container" ref="containerRef">
    <div class="feature-list">
      <div 
        v-for="(feature, index) in features" 
        :key="index"
        class="feature-item"
        :class="{ 'active': index === activeIndex }"
        @click="setActive(index)"
        @mouseenter="setActive(index)"
      >
        <div class="feature-number">{{ index + 1 }}</div>
        <div class="feature-content">
          <h3 class="feature-title">{{ feature.title }}</h3>
          <p class="feature-description">{{ feature.description }}</p>
        </div>
      </div>
    </div>
    <KinesisContainer class="feature-detail">
      <KinesisElement type="depth" :strength="10">
        <div class="feature-image-container">
          <img 
            :src="features[activeIndex]?.image || 'https://via.placeholder.com/600x400'" 
            alt="Feature illustration" 
            class="feature-image" 
          />
        </div>
      </KinesisElement>
    </KinesisContainer>
  </div>
</template>

<style scoped>
.feature-list-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 40px;
  align-items: center;
}

.feature-list {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.feature-item {
  display: flex;
  padding: 16px;
  border-radius: 8px;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: pointer;
  border-left: 3px solid transparent;
  background: linear-gradient(to right, transparent 50%, rgba(0, 122, 255, 0.1) 50%);
  background-size: 200% 100%;
  background-position: left bottom;
}

.feature-item.active {
  background-position: right bottom;
  border-left-color: var(--primary);
  transform: translateX(10px);
}

.feature-number {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  background: linear-gradient(135deg, var(--primary), var(--secondary));
  color: white;
  border-radius: 50%;
  font-weight: 600;
  margin-right: 16px;
  flex-shrink: 0;
  transition: transform 0.3s ease;
}

.feature-item:hover .feature-number {
  transform: scale(1.1) rotate(10deg);
}

.feature-content {
  flex: 1;
}

.feature-title {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 8px;
  transition: color 0.3s ease;
}

.feature-description {
  font-size: 14px;
  color: var(--neutral-600);
  line-height: 1.5;
  transition: color 0.3s ease;
}

.feature-detail {
  overflow: hidden;
  border-radius: 12px;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
  transition: transform 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.feature-image-container {
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: relative;
}

.feature-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.feature-image:hover {
  transform: scale(1.05);
}

@media (max-width: 768px) {
  .feature-list-container {
    grid-template-columns: 1fr;
  }
  
  .feature-detail {
    order: -1;
    margin-bottom: 24px;
  }
  
  .feature-image-container {
    height: 250px;
  }
}

/* Dark mode styles */
:global(.dark-mode) .feature-item {
  background: linear-gradient(to right, transparent 50%, rgba(10, 132, 255, 0.1) 50%);
  background-size: 200% 100%;
  background-position: left bottom;
}

:global(.dark-mode) .feature-title {
  color: var(--neutral-200);
}

:global(.dark-mode) .feature-description {
  color: var(--neutral-400);
}
</style>