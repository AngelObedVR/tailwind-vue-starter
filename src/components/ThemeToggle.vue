<template>
  <button
    id="theme-toggle"
    type="button"
    :aria-label="isDark ? 'Activar modo claro' : 'Activar modo oscuro'"
    class="text-gray-500 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-700 focus:outline-none focus:ring-4 focus:ring-gray-200 dark:focus:ring-gray-700 rounded-lg text-sm p-2.5"
    @click="toggleTheme"
  >
    <MoonIcon v-if="isDark"/>
    <SunIcon v-else/>
  </button>
</template>

<script setup lang="ts">
import MoonIcon from './icons/MoonIcon.vue'
import SunIcon from './icons/SunIcon.vue'
import { ref, onMounted } from 'vue'

const isDark = ref(false)

function setTheme(dark: boolean) {
  isDark.value = dark
  if (dark) {
    document.documentElement.classList.add('dark')
    localStorage.setItem('color-theme', 'dark')
  } else {
    document.documentElement.classList.remove('dark')
    localStorage.setItem('color-theme', 'light')
  }
}

function toggleTheme() {
  setTheme(!isDark.value)
}

onMounted(() => {
  const savedTheme = localStorage.getItem('color-theme')
  if (savedTheme) {
    setTheme(savedTheme === 'dark')
  } else {
    // Detecta preferencia del sistema
    setTheme(window.matchMedia('(prefers-color-scheme: dark)').matches)
  }
})
</script>
