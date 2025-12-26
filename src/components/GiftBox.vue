<script setup>
import { ref } from 'vue'

// ✨ เพิ่ม 'start-music' เข้าไปใน defineEmits
const emit = defineEmits(['next', 'start-music'])
const isOpening = ref(false)

const openGift = () => {
  if (isOpening.value) return
  isOpening.value = true
  
  // ✨ สั่งให้เพลงเริ่มเล่นทันทีที่กด!
  emit('start-music')
  
  // เล่น animation เปิดกล่อง แล้วค่อยไปหน้าถัดไป
  setTimeout(() => {
    emit('next')
  }, 800)
}
</script>

<template>
  <div class="min-h-screen flex flex-col items-center justify-center p-4 relative overflow-hidden bg-gradient-to-br from-pink-100 via-purple-50 to-pink-100 font-sans">
    
    <div class="absolute inset-0 pointer-events-none">
      <div v-for="n in 20" :key="n" 
           class="floating-particle absolute text-2xl opacity-60"
           :style="{
             left: `${Math.random() * 100}%`,
             top: `${Math.random() * 100}%`,
             animationDelay: `${Math.random() * 5}s`,
             animationDuration: `${10 + Math.random() * 10}s`
           }">
        {{ ['💖', '✨', '⭐', '🌸', '🎉'][Math.floor(Math.random() * 5)] }}
      </div>
    </div>

    <div class="text-center z-10 space-y-10 relative">
      
      <h1 class="text-4xl md:text-5xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-pink-500 to-purple-500 animate-bounce-slow drop-shadow-sm select-none">
        ของขวัญสำหรับคนน่ารัก
      </h1>

      <div class="relative group cursor-pointer perspective-container flex justify-center" @click="openGift">
        
        <div class="absolute -inset-10 bg-pink-300/40 rounded-full blur-3xl opacity-0 group-hover:opacity-100 transition-opacity duration-500 pointer-events-none"></div>
        
        <div class="transition-transform duration-300 ease-out group-hover:scale-125">
          
          <div 
            class="relative"
            :class="{
              'animate-gentle-shake': !isOpening, 
              'animate-pop-open': isOpening
            }"
          >
             <span class="text-[180px] md:text-[220px] filter drop-shadow-2xl leading-none block select-none">🎁</span>
          </div>

        </div>

      </div>

      <p class="text-xl text-pink-400 font-medium animate-pulse group-hover:text-pink-600 transition-colors select-none">
        👆 แตะที่กล่องเพื่อเปิดดูสิ!
      </p>
    </div>
  </div>
</template>

<style scoped>
/* Animation: ข้อความเด้งช้าๆ */
.animate-bounce-slow {
  animation: bounceSlow 3s infinite ease-in-out;
}
@keyframes bounceSlow {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

/* Animation: กล่องสั่นเบาๆ (Gentle Shake) */
.animate-gentle-shake {
  animation: gentleShake 2.5s infinite ease-in-out;
}
@keyframes gentleShake {
  0%, 100% { transform: rotate(0deg); }
  25% { transform: rotate(3deg); }
  75% { transform: rotate(-3deg); }
}

/* Animation: ตอนเปิดกล่อง (Pop Open) */
.animate-pop-open {
  animation: popOpen 0.8s forwards cubic-bezier(0.68, -0.55, 0.265, 1.55);
}
@keyframes popOpen {
  0% { transform: scale(1); }
  50% { transform: scale(1.2) rotate(5deg); opacity: 1; }
  100% { transform: scale(0) rotate(20deg); opacity: 0; }
}

/* Animation: ไอเทมลอยในฉากหลัง */
.floating-particle {
  animation-name: floatUp;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
}
@keyframes floatUp {
  0% { transform: translateY(0) rotate(0deg); opacity: 0; }
  20% { opacity: 0.7; }
  80% { opacity: 0.7; }
  100% { transform: translateY(-100vh) rotate(360deg); opacity: 0; }
}

.perspective-container {
    perspective: 1000px;
}
</style>