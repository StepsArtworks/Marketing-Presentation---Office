<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps({
  darkMode: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['toggle-dark-mode'])

const isMenuOpen = ref(false)

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
}

const closeMenu = () => {
  isMenuOpen.value = false
}

const toggleDarkMode = () => {
  emit('toggle-dark-mode')
}
</script>

<template>
  <nav class="navbar" :class="{ 'scrolled': true }">
    <div class="navbar-container">
      <div class="logo">
        <span class="logo-text">TBR</span>
      </div>
      
      <div class="navbar-right">
        <ul class="nav-links desktop-only">
          <li><a href="#home" @click="closeMenu">Home</a></li>
          <li><a href="#services" @click="closeMenu">Services</a></li>
          <li><a href="#about" @click="closeMenu">About</a></li>
          <li><a href="#contact" @click="closeMenu">Contact</a></li>
        </ul>
        
        <button @click="toggleDarkMode" class="dark-mode-toggle" :title="darkMode ? 'Switch to light mode' : 'Switch to dark mode'">
          <span v-if="darkMode" class="toggle-icon">☀️</span>
          <span v-else class="toggle-icon">🌙</span>
        </button>
        
        <button @click="toggleMenu" class="menu-toggle mobile-only">
          <span class="menu-bar"></span>
          <span class="menu-bar"></span>
          <span class="menu-bar"></span>
        </button>
      </div>
    </div>
    
    <div class="mobile-menu" :class="{ 'open': isMenuOpen }">
      <ul class="mobile-nav-links">
        <li><a href="#home" @click="closeMenu">Home</a></li>
        <li><a href="#services" @click="closeMenu">Services</a></li>
        <li><a href="#about" @click="closeMenu">About</a></li>
        <li><a href="#contact" @click="closeMenu">Contact</a></li>
      </ul>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
  transition: all 0.3s ease;
  padding: 16px 0;
  background-color: transparent;
}

.navbar.scrolled {
  background-color: rgba(255, 255, 255, 0.95);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
}

.navbar-container {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 24px;
}

.logo {
  display: flex;
  align-items: center;
}

.logo-text {
  font-size: 24px;
  font-weight: 700;
  color: var(--primary);
}

.navbar-right {
  display: flex;
  align-items: center;
}

.nav-links {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav-links li {
  margin-left: 32px;
}

.nav-links a {
  color: var(--neutral-700);
  text-decoration: none;
  font-size: 16px;
  font-weight: 500;
  transition: all 0.2s ease;
}

.nav-links a:hover {
  color: var(--primary);
}

.dark-mode-toggle {
  background: none;
  border: none;
  cursor: pointer;
  margin-left: 24px;
  font-size: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  transition: all 0.2s ease;
}

.dark-mode-toggle:hover {
  background-color: var(--neutral-200);
}

.menu-toggle {
  display: none;
  flex-direction: column;
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
  margin-left: 16px;
}

.menu-bar {
  width: 24px;
  height: 2px;
  background-color: var(--neutral-700);
  margin: 4px 0;
  transition: all 0.3s ease;
}

.mobile-menu {
  position: fixed;
  top: 70px;
  left: 0;
  width: 100%;
  background-color: white;
  padding: 16px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  transform: translateY(-100%);
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  z-index: 999;
}

.mobile-menu.open {
  transform: translateY(0);
  opacity: 1;
  visibility: visible;
}

.mobile-nav-links {
  list-style: none;
  margin: 0;
  padding: 0;
}

.mobile-nav-links li {
  margin: 16px 0;
}

.mobile-nav-links a {
  color: var(--neutral-700);
  text-decoration: none;
  font-size: 18px;
  font-weight: 500;
  display: block;
  padding: 8px 0;
}

@media (max-width: 768px) {
  .menu-toggle {
    display: flex;
  }
}

/* Dark mode styles */
:global(.dark-mode) .navbar.scrolled {
  background-color: rgba(31, 41, 55, 0.95);
}

:global(.dark-mode) .nav-links a {
  color: var(--neutral-300);
}

:global(.dark-mode) .nav-links a:hover {
  color: var(--primary-light);
}

:global(.dark-mode) .menu-bar {
  background-color: var(--neutral-300);
}

:global(.dark-mode) .mobile-menu {
  background-color: var(--neutral-800);
}

:global(.dark-mode) .mobile-nav-links a {
  color: var(--neutral-300);
}

:global(.dark-mode) .dark-mode-toggle:hover {
  background-color: var(--neutral-700);
}
</style>