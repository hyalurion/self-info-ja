<script setup>
import { ref, onMounted } from 'vue'
import pageData from './data/page-content.json'
import pageDataKana from './data/page-content-index-kana.json'
import SplashScreen from './components/SplashScreen.vue'
import LoadingScreen from './components/LoadingScreen.vue'
import PageHeader from './components/PageHeader.vue'
import SectionRenderer from './components/sections/SectionRenderer.vue'
import PageFooter from './components/PageFooter.vue'
import UIElements from './components/UIElements.vue'
import SakuraCanvas from './components/SakuraCanvas.vue'
import DocumentPage from './components/DocumentPage.vue'
import LanguageSelector from './components/LanguageSelector.vue'
import { initAnalytics } from './analytics.js'

const consentGiven = ref(false)
const loading = ref(false)
const showContent = ref(false)
const isKanaPage = ref(false)
const isDocumentPage = ref(false)

const currentPageData = ref(pageData)

function onConsent() {
  consentGiven.value = true
  loading.value = true
  initAnalytics()
}

function onLoadingComplete() {
  loading.value = false
  showContent.value = true
}

function onDocumentBack() {
  isDocumentPage.value = false
  window.history.replaceState({}, '', '?')
}

onMounted(() => {
  const params = new URLSearchParams(window.location.search)
  if (params.get('page') === 'document') {
    isDocumentPage.value = true
  }
  if (params.get('kana') === '1') {
    isKanaPage.value = true
    currentPageData.value = pageDataKana
  }
})
</script>

<template>
  <SplashScreen v-if="!consentGiven && !isDocumentPage" :data="currentPageData.splashScreen" :isKanaPage="isKanaPage" @consent="onConsent" />

  <LoadingScreen v-if="consentGiven && loading && !isDocumentPage" :isKanaPage="isKanaPage" @complete="onLoadingComplete" />

  <DocumentPage v-if="isDocumentPage" @back="onDocumentBack" />

  <div v-if="showContent && !isDocumentPage" class="page-wrapper">
    <SakuraCanvas />
    <PageHeader :data="currentPageData.header" />

    <main>
      <div v-for="(section, index) in currentPageData.sections" :key="index" class="section">
        <SectionRenderer :section="section" :isKanaPage="isKanaPage" />
      </div>
    </main>

    <PageFooter :data="currentPageData.footer" />
    <UIElements :data="currentPageData.ui" :isKanaPage="isKanaPage" />
    <LanguageSelector :isKanaPage="isKanaPage" />
  </div>
</template>