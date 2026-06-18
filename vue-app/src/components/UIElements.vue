<script setup>
import { ref, onMounted } from 'vue'
import RichText from './RichText.vue'

defineProps({
  data: { type: Object, required: true },
  isKanaPage: { type: Boolean, default: false },
})

const bgmPaused = ref(false)

function toggleBGM() {
  const bgm = document.getElementById('bgm-audio')
  if (!bgm) return
  if (bgm.paused) {
    bgm.play()
    bgmPaused.value = false
  } else {
    bgm.pause()
    bgmPaused.value = true
  }
}

onMounted(() => {
  const bgm = document.getElementById('bgm-audio')
  if (bgm) {
    bgm.play().catch(() => {})
    bgm.addEventListener('play', () => { bgmPaused.value = false })
    bgm.addEventListener('pause', () => { bgmPaused.value = true })
  }
})
</script>

<template>
  <!-- BGM Audio -->
  <audio v-if="data.bgm.src" id="bgm-audio" :src="data.bgm.src" loop autoplay></audio>

  <!-- BGM Toggle Button -->
  <div v-if="data.bgm.src" style="position: fixed; bottom: 10px; left: 20px; z-index: 1000;">
    <button id="bgm-toggle" class="bgm-button" :class="{ paused: bgmPaused }" title="BGM ON/OFF" @click="toggleBGM">♬</button>
  </div>

  <!-- Feedback -->
  <div v-if="data.feedback.href" style="position: fixed; bottom: 10px; right: 10px; z-index: 1000;">
    <a class="feedback-container" :href="data.feedback.href" target="_blank" style="display: inline-block; padding: 8px 16px; background-color: rgba(255, 255, 255, 0.2); backdrop-filter: blur(3px); border-radius: 20px; box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1); text-align: center; font-size: 14px; color: white; font-weight: 500; text-decoration: none; border: 1px solid rgba(255, 255, 255, 0.3); transition: all 0.3s ease;">
      <img :src="data.feedback.img" alt="feedback" class="icon" style="width: 16px; height: 16px; vertical-align: middle;">
    </a>
  </div>

  <!-- Changelog -->
  <div v-if="data.changelog.href" style="position: fixed; bottom: 10px; left: 50%; transform: translateX(-50%); padding: 8px 16px; background-color: rgba(255, 255, 255, 0.15); backdrop-filter: blur(3px); border-radius: 20px; box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1); text-align: center; font-size: 14px; color: #ffcc80; font-weight: 500; z-index: 1000; border: 1px solid rgba(255, 255, 255, 0.2);">
    <a :href="data.changelog.href" target="_blank" style="color: #ffcc80; text-decoration: none;">
      <RichText :segments="data.changelog.dateRich" />
      <img :src="data.changelog.img" alt="changelog" class="icon" style="width: 16px; height: 16px; vertical-align: middle; margin-left: 4px;" />
    </a>
  </div>

  <!-- Kana Button -->
  <a
    v-if="data.kanaButton.href"
    :href="isKanaPage ? '?' : '?kana=1'"
    id="kana-button"
    class="kana-button"
  >
    <RichText :segments="data.kanaButton.textRich" />
  </a>
</template>