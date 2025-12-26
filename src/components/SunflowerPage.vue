<script setup>
import { ref } from 'vue'
import sunflowerImg from '../assets/SUNFLOWER-removebg-preview.png' 

const emit = defineEmits(['next'])

const isAnimateHeart = ref(false)

// ฟังก์ชันเมื่อกดที่รูปดอกไม้
const handleFlowerClick = () => {
  if (isAnimateHeart.value) return // ป้องกันการกดซ้ำรัวๆ
  
  // 1. เริ่มเล่น Animation หัวใจ
  isAnimateHeart.value = true
  
  // 2. รอ 2 วินาที ให้ดูหัวใจฟุ้งๆ ก่อน แล้วค่อยไปหน้าถัดไป
  setTimeout(() => {
    emit('next')
  }, 2000)
}
</script>

<template>
  <div class="relative flex flex-col items-center justify-center min-h-screen overflow-hidden bg-pink-50 font-sans">
    
    <div class="absolute inset-0 pointer-events-none">
      <div class="floating-heart" style="left: 10%; animation-delay: 0s;">💖</div>
      <div class="floating-heart" style="left: 30%; animation-delay: 2s;">💕</div>
      <div class="floating-heart" style="left: 70%; animation-delay: 4s;">💗</div>
      <div class="floating-heart" style="left: 90%; animation-delay: 1s;">✨</div>
      <div class="floating-heart" style="left: 50%; animation-delay: 3s;">💝</div>
    </div>

    <div class="z-10 bg-white/90 p-8 md:p-10 rounded-[2.5rem] shadow-[0_20px_50px_rgba(255,107,139,0.3)] w-full max-w-md text-center mx-4 border-4 border-pink-100 backdrop-blur-sm animate-popup">

      <div class="mb-6 relative flex justify-center items-center">
        
        <img 
          :src="sunflowerImg" 
          alt="ช่อดอกทานตะวัน"
          @click="handleFlowerClick"
          :class="[
            'w-80 md:w-96 mx-auto drop-shadow-xl transition-transform duration-500 rounded-2xl cursor-pointer animate-pop-in',
            isAnimateHeart ? 'animate-bounce-click' : 'hover:scale-105'
          ]"
        />

        <div v-if="isAnimateHeart" class="absolute inset-0 pointer-events-none flex items-center justify-center z-20">
          <span class="particle-heart particle-1">💖</span>
          <span class="particle-heart particle-2">💕</span>
          <span class="particle-heart particle-3">✨</span>
          <span class="particle-heart particle-4">💝</span>
          <span class="particle-heart particle-5">💗</span>
          <span class="particle-heart particle-6">💖</span>
        </div>
      </div>

      <div class="space-y-2 relative z-10">
        <p class="text-xl md:text-2xl text-gray-700 font-medium">
          ดอกทานตะวันสำหรับคนเก่ง
        </p>
        
        <p 
          :class="['text-pink-400 text-sm font-semibold transition-opacity duration-300', isAnimateHeart ? 'opacity-0' : 'animate-pulse']"
        >
          ( จิ้มที่ดอกไม้เพื่อรับนะ 👆 )
        </p>
      </div>

      </div>
  </div>
</template>

<style scoped>
/* --- Animation เดิม --- */
.floating-heart {
  position: absolute;
  bottom: -50px;
  font-size: 2rem;
  opacity: 0.6;
  animation: floatUp 10s linear infinite;
}

@keyframes floatUp {
  0% { transform: translateY(0) rotate(0deg); opacity: 0; }
  10% { opacity: 0.8; }
  100% { transform: translateY(-120vh) rotate(360deg); opacity: 0; }
}

.animate-popup {
  animation: popup 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);
}

@keyframes popup {
  from { transform: scale(0.8) translateY(50px); opacity: 0; }
  to { transform: scale(1) translateY(0); opacity: 1; }
}

.animate-pop-in {
  animation: popIn 0.6s cubic-bezier(0.34, 1.56, 0.64, 1) backwards;
  animation-delay: 0.3s;
}
@keyframes popIn {
  from { transform: scale(0.5); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}

/* --- Animation ตอนคลิก --- */
.animate-bounce-click {
  animation: bounceClick 0.8s cubic-bezier(0.28, 0.84, 0.42, 1);
}
@keyframes bounceClick {
  0% { transform: scale(1); }
  30% { transform: scale(1.15) rotate(3deg); }
  50% { transform: scale(0.95) rotate(-3deg); }
  70% { transform: scale(1.05) rotate(1deg); }
  100% { transform: scale(1) rotate(0); }
}

/* หัวใจพุ่งกระจาย */
.particle-heart {
  position: absolute;
  font-size: 2.5rem;
  opacity: 0;
}

.particle-1 { animation: flyTopLeft 1.5s ease-out forwards; }
.particle-2 { animation: flyTopRight 1.5s ease-out forwards; }
.particle-3 { animation: flyBottomLeft 1.2s ease-out forwards; }
.particle-4 { animation: flyBottomRight 1.2s ease-out forwards; }
.particle-5 { animation: flyTop 1.5s ease-out forwards; }
.particle-6 { animation: flyBottom 1.5s ease-out forwards; }

@keyframes flyTopLeft {
  0% { opacity: 1; transform: translate(0, 0) scale(0.5); }
  100% { opacity: 0; transform: translate(-100px, -150px) scale(1.5) rotate(-45deg); }
}
@keyframes flyTopRight {
  0% { opacity: 1; transform: translate(0, 0) scale(0.5); }
  100% { opacity: 0; transform: translate(100px, -150px) scale(1.5) rotate(45deg); }
}
@keyframes flyBottomLeft {
  0% { opacity: 1; transform: translate(0, 0) scale(0.5); }
  100% { opacity: 0; transform: translate(-80px, 100px) scale(1.2) rotate(-20deg); }
}
@keyframes flyBottomRight {
  0% { opacity: 1; transform: translate(0, 0) scale(0.5); }
  100% { opacity: 0; transform: translate(80px, 100px) scale(1.2) rotate(20deg); }
}
@keyframes flyTop {
  0% { opacity: 1; transform: translate(0, 0) scale(0.5); }
  100% { opacity: 0; transform: translate(0, -180px) scale(1.8); }
}
@keyframes flyBottom {
  0% { opacity: 1; transform: translate(0, 0) scale(0.5); }
  100% { opacity: 0; transform: translate(0, 120px) scale(1); }
}
</style>