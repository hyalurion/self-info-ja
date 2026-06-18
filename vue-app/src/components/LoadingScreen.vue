<script setup>
import { ref, onMounted } from 'vue'
import RichText from './RichText.vue'

const props = defineProps({
  isKanaPage: { type: Boolean, default: false },
})

const emit = defineEmits(['complete'])

const visible = ref(true)

const loadingText = [
  { type: 'text', content: '少女がお願いを祈ってるよ…' }
]
const loadingTextKana = [
  { type: 'ruby', kanji: '少女', reading: 'しょうじょ' },
  { type: 'text', content: 'がお' },
  { type: 'ruby', kanji: '願', reading: 'ねが' },
  { type: 'text', content: 'いを' },
  { type: 'ruby', kanji: '祈', reading: 'いの' },
  { type: 'text', content: 'ってるよ…' }
]

onMounted(() => {
  setTimeout(() => {
    visible.value = false
    emit('complete')
  }, 2000)
})
</script>

<template>
  <div v-if="visible" id="loading-screen" class="splash-screen">
    <div class="splash-content-girl">
      <div class="loader"></div>
      <p class="splash-text">
        <RichText :segments="isKanaPage ? loadingTextKana : loadingText" />
      </p>
    </div>
  </div>
</template>