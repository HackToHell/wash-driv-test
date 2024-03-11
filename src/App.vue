<script setup>
import { usePreferredDark } from '@vueuse/core'
import QuizComponents from './components/QuizComponent.vue'
import { ref } from 'vue'

const isDark = ref('light')
isDark.value = usePreferredDark().value ? 'dark' : 'light'

</script>

<template>
  <header class="bg-neutral" data-theme={{isDark}}>
    <div class="text-neutral-content">Sample Washington state driving exam questions</div>
  </header>

  <main class="min-h-screen bg-neutral" data-theme={{isDark}}>
    <QuizComponents />
  </main>
</template>

