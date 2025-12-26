<script setup>
import { ref } from 'vue'
import GiftBox from './components/GiftBox.vue'
import PasswordGate from './components/PasswordGate.vue'
import SunflowerPage from './components/SunflowerPage.vue'
import MainMenu from './components/MainMenu.vue'
import TimerPage from './components/TimerPage.vue'
import LetterPage from './components/LetterPage.vue'
import QuizPage from './components/QuizPage.vue'
import GalleryPage from './components/GalleryPage.vue'

const currentPage = ref('gift-box')
const selectedMenu = ref(null)

const toPasswordPage = () => currentPage.value = 'password'
const toSunflowerPage = () => currentPage.value = 'sunflower'
const toMainMenu = () => currentPage.value = 'main-menu'

const handleMenuSelect = (menuId) => {
  selectedMenu.value = menuId
  currentPage.value = 'feature-' + menuId 
}
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-pink-100 via-pink-50 to-purple-100 font-sans">
    
    <transition name="fade" mode="out-in">
      <GiftBox v-if="currentPage === 'gift-box'" @next="toPasswordPage" />
      <PasswordGate v-else-if="currentPage === 'password'" @unlock="toSunflowerPage" />
      <SunflowerPage v-else-if="currentPage === 'sunflower'" @next="toMainMenu" />
      <MainMenu v-else-if="currentPage === 'main-menu'" @select="handleMenuSelect" />

      <TimerPage v-else-if="currentPage === 'feature-timer'" @back="toMainMenu" />

      <LetterPage 
        v-else-if="currentPage === 'feature-letter'" 
        @back="toMainMenu" 
      />

      <QuizPage 
        v-else-if="currentPage === 'feature-quiz'" 
        @back="toMainMenu" 
      />

      <GalleryPage 
        v-else-if="currentPage === 'feature-gallery'" 
        @back="toMainMenu" 
      />

      </transition>

  </div>
</template>

<style>
.fade-enter-active, .fade-leave-active { transition: opacity 0.3s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }
</style>