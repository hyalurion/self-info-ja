<script setup>
import RichText from './RichText.vue'

defineProps({
  data: { type: Object, required: true },
})
</script>

<template>
  <div class="header" style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 100vh;">
    <div class="glass-container-apple">
      <p v-for="(line, i) in data.lines" :key="i" :class="i === 0 ? 'info-text' : ''">
        <RichText :segments="line" />
      </p>
    </div>
  </div>
</template>