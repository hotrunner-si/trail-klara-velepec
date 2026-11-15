<script setup>
import { computed, onMounted, ref } from 'vue'
import moon from './icons/moon.svg'
import sun from './icons/sun.svg'

const props = defineProps({ labels: { type: Object, required: true } })
const key = 'athlete-theme'
const theme = ref('light')
const dark = computed(() => theme.value === 'dark')
const label = computed(() => dark.value ? props.labels.light : props.labels.dark)

function applyTheme(value) {
  theme.value = value
  document.documentElement.dataset.theme = value
}

function toggle() {
  const next = dark.value ? 'light' : 'dark'
  applyTheme(next)
  try { localStorage.setItem(key, next) } catch {}
}

onMounted(() => {
  let saved
  try { saved = localStorage.getItem(key) } catch {}
  applyTheme(saved === 'light' || saved === 'dark' ? saved :
    (window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light'))
})
</script>

<template>
  <button class="theme-toggle" type="button" :aria-label="label" :title="label" @click="toggle">
    <span class="theme-icon" aria-hidden="true" :style="{ '--theme-icon': 'url(&quot;' + (dark ? sun : moon) + '&quot;)' }"></span>
  </button>
</template>
