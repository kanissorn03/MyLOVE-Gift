<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// ✨✨ เช็คชื่อไฟล์รูปของคุณให้ชัวร์นะครับ (jpg/png) ✨✨
import coupleImg from '../assets/img_timer.jpg' 

const emit = defineEmits(['back'])
const START_DATE = new Date('2022-10-14T00:00:00') 

const timeData = ref({
  totalDays: 0,
  hours: 0,
  minutes: 0,
  seconds: 0
})

// --- State สำหรับ Animation หัวใจ ---
const isAnimateHeart = ref(false)

const triggerHeartAnimation = () => {
  if (isAnimateHeart.value) return // กันกดรัว
  isAnimateHeart.value = true
  
  // เล่น 1 วินาทีแล้วหยุด
  setTimeout(() => {
    isAnimateHeart.value = false
  }, 1000)
}

const updateTimer = () => {
  const now = new Date()
  const diff = now - START_DATE 
  const totalDays = Math.floor(diff / (1000 * 60 * 60 * 24))
  const hours = Math.floor((diff / (1000 * 60 * 60)) % 24)
  const minutes = Math.floor((diff / (1000 * 60)) % 60)
  const seconds = Math.floor((diff / 1000) % 60)
  timeData.value = { totalDays, hours, minutes, seconds }
}

let timerInterval
onMounted(() => {
  updateTimer()
  timerInterval = setInterval(updateTimer, 1000)
})

onUnmounted(() => {
  clearInterval(timerInterval)
})
</script>

<template>
  <div class="min-h-screen flex flex-col items-center justify-center p-4 bg-gradient-to-br from-[#fff0f5] to-[#ffe4e1] font-sans relative overflow-hidden">
    
    <div class="absolute inset-0 pointer-events-none">
      <div class="floating-heart" style="left: 10%; animation-delay: 0s;">💖</div>
      <div class="floating-heart" style="left: 25%; animation-delay: 2.5s;">🥰</div>
      <div class="floating-heart" style="left: 40%; animation-delay: 1s;">✨</div>
      <div class="floating-heart" style="left: 60%; animation-delay: 3s;">💑</div>
      <div class="floating-heart" style="left: 80%; animation-delay: 1.5s;">💗</div>
      <div class="floating-heart" style="left: 50%; animation-delay: 4s;">💝</div>
    </div>

    <div class="z-10 bg-white/60 backdrop-blur-xl p-6 rounded-[2.5rem] shadow-[0_20px_50px_rgba(233,92,160,0.3)] w-full max-w-sm text-center border-[3px] border-white/50 relative animate-popup">
      
      <h2 class="text-gray-500 text-sm font-bold tracking-wider uppercase mb-2">Together Since</h2>
      
      <div class="text-7xl font-extrabold text-[#e95ca0] mb-0 leading-tight animate-pulse-slow drop-shadow-sm">
        {{ timeData.totalDays }}
      </div>
      <div class="text-gray-400 text-sm font-medium mb-6">Days of Love</div>

      <div class="relative w-44 h-44 mx-auto mb-8 group cursor-pointer" @click="triggerHeartAnimation">
        
        <div class="absolute inset-0 bg-pink-200/80 rounded-[2rem] rotate-6 shadow-inner transition-transform duration-500"
             :class="isAnimateHeart ? 'rotate-12 scale-105' : 'group-hover:rotate-12'"></div>
        <div class="absolute inset-0 bg-pink-100/80 rounded-[2rem] -rotate-6 shadow-inner transition-transform duration-500"
             :class="isAnimateHeart ? '-rotate-12 scale-105' : 'group-hover:-rotate-12'"></div>
        
        <img 
          :src="coupleImg" 
          class="relative w-full h-full object-cover rounded-[2rem] border-[5px] border-white shadow-md transition-all duration-300"
          :class="isAnimateHeart ? 'scale-95 brightness-110' : 'group-hover:scale-105'"
          alt="Our Photo"
        />

        <div v-if="isAnimateHeart" class="absolute inset-0 pointer-events-none flex items-center justify-center z-20">
          <span class="particle-heart particle-1">💖</span>
          <span class="particle-heart particle-2">💕</span>
          <span class="particle-heart particle-3">✨</span>
          <span class="particle-heart particle-4">💝</span>
          <span class="particle-heart particle-5">💗</span>
          <span class="particle-heart particle-6">💖</span>
        </div>

        <div class="absolute -bottom-4 -right-4 text-4xl animate-bounce drop-shadow-md z-10">💖</div>
      </div>

      <div class="grid grid-cols-4 gap-3 mb-8 px-2">
        <div class="time-box bg-gradient-to-br from-[#e95ca0] to-[#ff80bf]">
          <span class="text-xl font-extrabold">{{ timeData.totalDays }}</span>
          <span class="text-[9px] font-bold uppercase tracking-wide opacity-80">Days</span>
        </div>
        <div class="time-box bg-gradient-to-br from-[#ef7ab3] to-[#ff99cc]">
          <span class="text-xl font-extrabold">{{ timeData.hours }}</span>
          <span class="text-[9px] font-bold uppercase tracking-wide opacity-80">Hours</span>
        </div>
        <div class="time-box bg-gradient-to-br from-[#f49ac6] to-[#ffb3d9]">
          <span class="text-xl font-extrabold">{{ timeData.minutes }}</span>
          <span class="text-[9px] font-bold uppercase tracking-wide opacity-80">Mins</span>
        </div>
        <div class="time-box bg-gradient-to-br from-[#f9bad9] to-[#ffcce6]">
          <span class="text-xl font-extrabold w-7 inline-block">{{ timeData.seconds }}</span>
          <span class="text-[9px] font-bold uppercase tracking-wide opacity-80">Secs</span>
        </div>
      </div>

      <button 
        @click="emit('back')"
        class="bg-[#e95ca0] text-white px-8 py-3 rounded-full font-bold shadow-lg hover:bg-[#d4458d] hover:shadow-pink-300/50 transition-all active:scale-95 flex items-center justify-center mx-auto gap-2"
      >
        <span>❮</span> Back to Menu
      </button>

    </div>
  </div>
</template>

<style scoped>
/* สไตล์เดิม */
.time-box {
  @apply text-white rounded-2xl py-3 flex flex-col items-center justify-center shadow-sm transform transition-all hover:-translate-y-1 hover:shadow-md border border-white/20;
}
.floating-heart {
  position: absolute;
  bottom: -50px;
  font-size: 1.5rem;
  opacity: 0.5;
  animation: floatUp 12s linear infinite;
}
@keyframes floatUp {
  0% { transform: translateY(0) rotate(0deg); opacity: 0; }
  20% { opacity: 0.7; }
  100% { transform: translateY(-110vh) rotate(360deg); opacity: 0; }
}
.animate-popup {
  animation: popup 0.7s cubic-bezier(0.34, 1.56, 0.64, 1) backwards;
}
@keyframes popup {
  from { transform: scale(0.9) translateY(30px); opacity: 0; }
  to { transform: scale(1) translateY(0); opacity: 1; }
}
.animate-pulse-slow {
  animation: pulseSlow 3s ease-in-out infinite;
}
@keyframes pulseSlow {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}

/* ✨✨ Animation ใหม่สำหรับหัวใจพุ่ง (Copy มาจากหน้าดอกไม้) ✨✨ */
.particle-heart {
  position: absolute;
  font-size: 2.5rem;
  opacity: 0;
}
.particle-1 { animation: flyTopLeft 1s ease-out forwards; }
.particle-2 { animation: flyTopRight 1s ease-out forwards; }
.particle-3 { animation: flyBottomLeft 0.8s ease-out forwards; }
.particle-4 { animation: flyBottomRight 0.8s ease-out forwards; }
.particle-5 { animation: flyTop 1.2s ease-out forwards; }
.particle-6 { animation: flyBottom 1.2s ease-out forwards; }

@keyframes flyTopLeft {
  0% { opacity: 1; transform: translate(0, 0) scale(0.5); }
  100% { opacity: 0; transform: translate(-100px, -120px) scale(1.5) rotate(-45deg); }
}
@keyframes flyTopRight {
  0% { opacity: 1; transform: translate(0, 0) scale(0.5); }
  100% { opacity: 0; transform: translate(100px, -120px) scale(1.5) rotate(45deg); }
}
@keyframes flyBottomLeft {
  0% { opacity: 1; transform: translate(0, 0) scale(0.5); }
  100% { opacity: 0; transform: translate(-80px, 80px) scale(1.2) rotate(-20deg); }
}
@keyframes flyBottomRight {
  0% { opacity: 1; transform: translate(0, 0) scale(0.5); }
  100% { opacity: 0; transform: translate(80px, 80px) scale(1.2) rotate(20deg); }
}
@keyframes flyTop {
  0% { opacity: 1; transform: translate(0, 0) scale(0.5); }
  100% { opacity: 0; transform: translate(0, -150px) scale(1.8); }
}
@keyframes flyBottom {
  0% { opacity: 1; transform: translate(0, 0) scale(0.5); }
  100% { opacity: 0; transform: translate(0, 100px) scale(1); }
}
</style>