<script setup>
import { ref } from 'vue'
import NavBar from './components/NavBar.vue'

const darkMode = ref(false)

const toggleDarkMode = () => {
  darkMode.value = !darkMode.value
  if (darkMode.value) {
    document.documentElement.classList.add('dark-mode')
    localStorage.setItem('theme', 'dark')
  } else {
    document.documentElement.classList.remove('dark-mode')
    localStorage.setItem('theme', 'light')
  }
}
</script>

<template>
  <div class="app-container">
    <NavBar :dark-mode="darkMode" @toggle-dark-mode="toggleDarkMode" />
    
    <main>
      <router-view></router-view>
    </main>
  </div>
</template>

<style>
@import './styles/main.css';

body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Text', 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

.app-container {
  min-height: 100vh;
}

main {
  padding-top: 70px; /* Space for fixed navbar */
}
</style>