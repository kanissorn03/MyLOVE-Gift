<script setup>
import { ref, nextTick } from 'vue'
import GiftBox from './components/GiftBox.vue'
import PasswordGate from './components/PasswordGate.vue'
import SunflowerPage from './components/SunflowerPage.vue'
import MainMenu from './components/MainMenu.vue'
import TimerPage from './components/TimerPage.vue'
import LetterPage from './components/LetterPage.vue'
import QuizPage from './components/QuizPage.vue'
import GalleryPage from './components/GalleryPage.vue' 
import bgMusicFile from './assets/bg-music.mp3'

// ==============================
// ส่วนจัดการระบบหลัก (Navigation & Music)
// ==============================
const currentPage = ref('gift-box')
const selectedMenu = ref(null)
const audioPlayer = ref(null)
const isMusicPlaying = ref(false)
const hasStarted = ref(false)

const toPasswordPage = () => currentPage.value = 'password'
const toSunflowerPage = () => currentPage.value = 'sunflower'
const toMainMenu = () => currentPage.value = 'main-menu'
const handleMenuSelect = (menuId) => {
  selectedMenu.value = menuId
  currentPage.value = 'feature-' + menuId 
}

const startMusic = () => {
  if (audioPlayer.value) {
    audioPlayer.value.volume = 0.3
    audioPlayer.value.play().then(() => {
      isMusicPlaying.value = true
      hasStarted.value = true
    }).catch(err => console.log("Audio play failed:", err))
  }
}

const toggleMusic = () => {
  if (!audioPlayer.value) return
  if (isMusicPlaying.value) {
    audioPlayer.value.pause()
    isMusicPlaying.value = false
  } else {
    audioPlayer.value.play()
    isMusicPlaying.value = true
  }
}

// ==============================
// ✨ ส่วนจัดการ Effect อีโมจิระเบิด ✨
// ==============================
const particles = ref([]) // เก็บรายการอีโมจิที่กำลังแสดงผล
const emojiList = ['💖', '✨', '🌸', '🎉', '💝', '🥰', '💗', '🍭'] // รายการอีโมจิที่จะสุ่ม

const handleGlobalClick = (e) => {
  // ป้องกันไม่ให้ Effect ทำงานถ้ากดที่ปุ่มหรือ input (optional)
  if (e.target.tagName === 'BUTTON' || e.target.tagName === 'INPUT') return;

  const x = e.clientX
  const y = e.clientY
  const particleCount = 12 // จำนวนอีโมจิต่อการคลิก 1 ครั้ง

  for (let i = 0; i < particleCount; i++) {
    const id = Date.now() + i
    const emoji = emojiList[Math.floor(Math.random() * emojiList.length)]
    
    // สุ่มทิศทางและความแรงในการกระเด็น (ใช้ CSS Variable)
    const angle = Math.random() * Math.PI * 2 // มุม 360 องศา
    const velocity = 50 + Math.random() * 100 // ความแรง 50-150px
    const tx = Math.cos(angle) * velocity
    const ty = Math.sin(angle) * velocity
    const rotation = Math.random() * 360 // มุมหมุนเริ่มต้น

    particles.value.push({
      id, x, y, emoji,
      style: {
        '--tx': `${tx}px`,
        '--ty': `${ty}px`,
        '--rot': `${rotation}deg`
      }
    })

    // ลบอีโมจิออกจากสายตาเมื่อเล่น Animation จบ (1 วินาที)
    setTimeout(() => {
      particles.value = particles.value.filter(p => p.id !== id)
    }, 1000)
  }
}
</script>

<template>
  <div 
    class="min-h-screen bg-gradient-to-br from-pink-100 via-pink-50 to-purple-100 font-sans relative overflow-hidden"
    @click="handleGlobalClick"
  >
    
    <audio ref="audioPlayer" loop>
      <source :src="bgMusicFile" type="audio/mp3">
    </audio>
    <button 
      v-if="hasStarted"
      @click.stop="toggleMusic" 
      class="fixed top-4 right-4 z-[100] w-12 h-12 bg-white/80 backdrop-blur-sm rounded-full shadow-md flex items-center justify-center text-2xl hover:scale-110 transition-transform cursor-pointer border-2 border-pink-100"
    >
      {{ isMusicPlaying ? '🔊' : '🔇' }}
    </button>

    <div class="pointer-events-none fixed inset-0 z-[9999]">
      <div
        v-for="p in particles"
        :key="p.id"
        class="explosion-particle absolute text-2xl"
        :style="{
          left: p.x + 'px',
          top: p.y + 'px',
          ...p.style
        }"
      >
        {{ p.emoji }}
      </div>
    </div>

    <transition name="fade" mode="out-in">
      <GiftBox v-if="currentPage === 'gift-box'" @next="toPasswordPage" @start-music="startMusic" />
      <PasswordGate v-else-if="currentPage === 'password'" @unlock="toSunflowerPage" />
      <SunflowerPage v-else-if="currentPage === 'sunflower'" @next="toMainMenu" />
      <MainMenu v-else-if="currentPage === 'main-menu'" @select="handleMenuSelect" />
      <TimerPage v-else-if="currentPage === 'feature-timer'" @back="toMainMenu" />
      <LetterPage v-else-if="currentPage === 'feature-letter'" @back="toMainMenu" />
      <QuizPage v-else-if="currentPage === 'feature-quiz'" @back="toMainMenu" />
      <GalleryPage v-else-if="currentPage === 'feature-gallery'" @back="toMainMenu" />
      <div v-else class="flex flex-col items-center justify-center min-h-screen p-4 text-center">
        </div>
    </transition>

  </div>
</template>

<style>
.fade-enter-active, .fade-leave-active { transition: opacity 0.3s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }

/* ✨ CSS Animation สำหรับอีโมจิระเบิด ✨ */
.explosion-particle {
  /* จัดให้อยู่ตรงกลางจุดคลิก */
  transform: translate(-50%, -50%);
  /* เล่น Animation ชื่อ explode นาน 1 วินาที */
  animation: explode 1s cubic-bezier(0.25, 0.46, 0.45, 0.94) forwards;
}

@keyframes explode {
  0% {
    opacity: 1;
    /* เริ่มต้นที่จุดคลิก และมุมหมุนเริ่มต้น */
    transform: translate(-50%, -50%) rotate(var(--rot)) scale(0.5);
  }
  100% {
    opacity: 0;
    /* สิ้นสุดโดยการเคลื่อนที่ไปตามค่า tx, ty และหมุนเพิ่ม */
    transform: translate(calc(var(--tx) - 50%), calc(var(--ty) - 50%)) rotate(calc(var(--rot) + 180deg)) scale(1.2);
  }
}
</style>