<script setup>
import { ref } from 'vue'
import RichText from './RichText.vue'

defineProps({
  isKanaPage: { type: Boolean, default: false },
})

const popupVisible = ref(false)

const languages = [
  {
    code: 'en',
    name: [{ type: 'ruby', kanji: '英語', reading: 'えいご' }],
    namePlain: [{ type: 'text', content: '英語' }],
    localName: 'English',
    link: 'https://hyalurion.github.io/self-info-en',
  },
  {
    code: 'zh-Hans',
    name: [
      { type: 'ruby', kanji: '中国語', reading: 'ちゅうごくご' },
      { type: 'text', content: '（' },
      { type: 'ruby', kanji: '簡体字', reading: 'かんたいじ' },
      { type: 'text', content: '）' },
    ],
    namePlain: [{ type: 'text', content: '中国語（簡体字）' }],
    localName: '华文（马来西亚/新加坡）',
    link: 'https://self-info-zh-hans.netlify.app/',
  },
  {
    code: 'zh-TW',
    name: [
      { type: 'ruby', kanji: '中国語', reading: 'ちゅうごくご' },
      { type: 'text', content: '（' },
      { type: 'ruby', kanji: '繁体字', reading: 'はんたいじ' },
      { type: 'text', content: '）' },
    ],
    namePlain: [{ type: 'text', content: '中国語（繁体字）' }],
    localName: '繁體中文（台灣）',
    link: 'https://hyalurion.github.io/self-info-zh-tw/',
  },
]

function togglePopup() {
  popupVisible.value = !popupVisible.value
}

function hidePopup() {
  popupVisible.value = false
}

function onPopupClick(e) {
  e.stopPropagation()
}
</script>

<template>
  <!-- Language Selector Button -->
  <button
    id="language-button"
    class="language-button"
    @click.stop="togglePopup"
  >
    <img src="/pic/lang.svg" alt="lang" style="width: 15px; height: 15px;" />
    日本語
  </button>

  <!-- Language Popup -->
  <Teleport to="body">
    <Transition name="popup">
      <div
        v-if="popupVisible"
        class="language-popup-overlay"
        @click="hidePopup"
      >
        <div
          class="language-popup"
          @click="onPopupClick"
        >
          <a
            v-for="(lang, index) in languages"
            :key="lang.code"
            :href="lang.link"
            target="_blank"
            class="language-item"
            :style="{ borderTop: index > 0 ? '1px solid rgba(255, 255, 255, 0.15)' : 'none' }"
          >
            <div class="language-name">
              <RichText :segments="isKanaPage ? lang.name : lang.namePlain" />
            </div>
            <div class="language-local">{{ lang.localName }}</div>
          </a>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<style>
.language-button {
  position: fixed;
  top: 20px;
  right: 20px;
  padding: 10px 18px;
  border-radius: 16px;
  background: rgba(255, 255, 255, 0.15);
  border: 1px solid rgba(255, 255, 255, 0.3);
  color: #ffffff;
  font-family: 'KleeOne-Regular', system-ui, sans-serif;
  font-size: 13px;
  cursor: pointer;
  z-index: 1000;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
}

.language-button:hover {
  background: rgba(255, 255, 255, 0.25);
}

.language-popup-overlay {
  position: fixed;
  inset: 0;
  z-index: 1001;
  background: transparent;
}

.language-popup {
  position: fixed;
  top: 70px;
  right: 20px;
  width: 200px;
  max-width: 90vw;
  max-height: 80vh;
  padding: 10px;
  border-radius: 24px;
  background: rgba(255, 255, 255, 0.12);
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
  z-index: 1001;
  display: flex;
  flex-direction: column;
  gap: 0;
  overflow-y: auto;
  scrollbar-width: thin;
  scrollbar-color: rgba(255, 255, 255, 0.3) transparent;
}

/* Transition: enter */
.popup-enter-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}
.popup-enter-from {
  opacity: 0;
  transform: translateY(-10px);
}

/* Transition: leave */
.popup-leave-active {
  transition: opacity 0.2s ease, transform 0.2s ease;
}
.popup-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

.language-item {
  display: block;
  padding: 8px 12px;
  color: #ffffff;
  text-decoration: none;
  transition: all 0.3s ease;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.language-item:hover {
  background: rgba(255, 255, 255, 0.08);
  transform: translateX(3px);
}

.language-name {
  font-weight: bold;
  margin-bottom: 3px;
  font-size: 15px;
}

.language-local {
  font-size: 13px;
  opacity: 0.8;
}

@media (max-width: 600px) {
  .language-button {
    padding: 8px 16px;
    font-size: 13px;
    right: 15px;
  }
}
</style>