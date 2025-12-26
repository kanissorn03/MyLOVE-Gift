<script setup>
import { ref } from 'vue'

const emit = defineEmits(['back'])
const isOpen = ref(false)

const toggleLetter = () => {
  isOpen.value = !isOpen.value
}
</script>

<template>
  <div class="min-h-screen flex flex-col items-center justify-center p-4 bg-[#fdf2f8] relative overflow-hidden font-sans">
    
    <div class="absolute top-[-150px] left-[-150px] w-96 h-96 bg-pink-200/40 rounded-full blur-[100px] pointer-events-none z-0"></div>
    <div class="absolute bottom-[-150px] right-[-150px] w-96 h-96 bg-purple-200/30 rounded-full blur-[100px] pointer-events-none z-0"></div>

    <div class="absolute inset-0 pointer-events-none z-0">
      <div class="floating-heart text-2xl" style="left: 10%; animation-delay: 0s; animation-duration: 15s;">💖</div>
      <div class="floating-heart text-xl" style="left: 20%; animation-delay: 5s; animation-duration: 12s;">✨</div>
      <div class="floating-heart text-3xl" style="left: 85%; animation-delay: 2s; animation-duration: 18s;">💌</div>
      <div class="floating-heart text-xl" style="left: 30%; animation-delay: 4s; animation-duration: 14s;">🥰</div>
      <div class="floating-heart text-2xl" style="left: 60%; animation-delay: 1s; animation-duration: 16s;">💗</div>
      <div class="floating-heart text-4xl" style="left: 50%; animation-delay: 7s; animation-duration: 20s;">💝</div>
      <div class="floating-heart text-xl" style="left: 75%; animation-delay: 3s; animation-duration: 13s;">✨</div>
    </div>

    <button 
      @click="emit('back')" 
      class="absolute top-6 left-6 z-50 w-10 h-10 bg-white/80 backdrop-blur rounded-full shadow-md flex items-center justify-center text-pink-500 hover:scale-110 transition-transform"
    >
      ❮
    </button>

    <div class="relative w-full max-w-lg h-[500px] flex items-end justify-center pb-20 perspective-container z-10">
      
      <div 
        class="relative w-[320px] h-[220px] bg-[#ff8fa7] rounded-xl shadow-2xl transition-transform duration-700 ease-in-out"
        :class="{ 'translate-y-[80px]': isOpen }" 
      >
        
        <div 
          class="absolute left-4 right-4 bg-white rounded-lg transition-all duration-1000 ease-[cubic-bezier(0.34,1.56,0.64,1)] flex flex-col items-center border border-gray-100 shadow-sm overflow-hidden"
          :class="isOpen ? '-translate-y-[300px] h-[420px] z-10' : 'top-2 bottom-2 h-[90%] z-10'"
        >
          <div class="w-full h-full overflow-y-auto custom-scrollbar p-6 text-center space-y-5">
            
            <h3 class="text-2xl font-bold text-pink-600 pb-2 border-b border-pink-100">
              ถึง คนเก่งของเค้า 🐰
            </h3>
            
            <div class="text-gray-700 leading-relaxed text-base md:text-lg font-medium space-y-3 pt-2">

              <p>ขอบคุณที่อยู่ข้างกันมาตลอด...</p>
              <p>ขอบคุณที่เป็นรอยยิ้มให้กันในทุกๆ วันน้าา</p>
              <p>ขอให้เราแข็งแรง อยู่ด้วยกันตลอด</p>
              <p>ขอให้เรามีความสุขด้วยกันแบบนี้ไปนานๆ เลยนะ</p>
              <p class="text-pink-500 font-bold text-xl animate-pulse-slow">รักมากๆ เลยนะรู้มั้ย ❤️</p>

            </div>

            <p class="text-right text-pink-400 font-bold text-base mt-8">
              จาก... คนที่รักเธอที่สุด 💕
            </p>

            <div class="h-32"></div> 
            
            <div v-if="isOpen" class="pb-6">
               <button 
                @click="toggleLetter"
                class="text-sm text-pink-400 underline hover:text-pink-600 transition-colors flex items-center justify-center gap-1 mx-auto"
              >
                <span>💕</span> เก็บจดหมายลงซอง
              </button>
            </div>
          </div>
        </div>

        <div class="absolute bottom-0 left-0 w-full h-[65%] bg-[#ff6b8b] rounded-b-xl z-20 shadow-inner border-t border-pink-400/20"></div>

        <div 
          class="absolute top-0 left-0 w-full h-[45%] bg-[#ff5c82] rounded-b-[3rem] rounded-t-xl origin-top transition-all duration-700 ease-in-out shadow-md border-b-2 border-pink-600/10"
          :class="isOpen ? 'rotate-x-180 z-0' : 'z-30 delay-500'"
        ></div>

        <div 
          @click="toggleLetter"
          class="absolute top-[45%] left-1/2 -translate-x-1/2 -translate-y-1/2 w-14 h-14 bg-white rounded-full shadow-[0_4px_15px_rgba(255,107,139,0.4)] flex items-center justify-center cursor-pointer z-40 hover:scale-110 transition-transform duration-300 group border-[3px] border-pink-50"
          :class="{ 'opacity-0 pointer-events-none scale-50': isOpen }"
        >
          <span class="text-3xl group-hover:animate-heartbeat">💌</span>
        </div>

      </div>
    </div>

    <div class="absolute bottom-8 z-50 text-center w-full pointer-events-none">
      <p v-if="!isOpen" class="text-pink-500 font-bold text-lg animate-bounce mb-4 drop-shadow-sm bg-white/50 backdrop-blur-sm py-2 px-4 rounded-full inline-block">
        แตะที่ซองเพื่อเปิดอ่าน 👆
      </p>
    </div>

  </div>
</template>

<style scoped>
.rotate-x-180 {
  transform: rotateX(180deg);
}

.perspective-container {
  perspective: 1000px;
}

/* Scrollbar */
.custom-scrollbar::-webkit-scrollbar {
  width: 4px;
}
.custom-scrollbar::-webkit-scrollbar-track {
  background: transparent;
}
.custom-scrollbar::-webkit-scrollbar-thumb {
  background: #ffb6c1;
  border-radius: 10px;
}
.custom-scrollbar::-webkit-scrollbar-thumb:hover {
  background: #ff8fa7;
}

/* Animation */
@keyframes heartbeat {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.2); }
}
.group-hover\:animate-heartbeat:hover {
  animation: heartbeat 1s infinite;
}

@keyframes pulseSlow {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.7; }
}
.animate-pulse-slow {
  animation: pulseSlow 3s ease-in-out infinite;
}

.floating-heart {
  position: absolute;
  bottom: -100px; /* เริ่มต้นต่ำกว่าเดิม */
  opacity: 0.6;
  /* ใช้ animation-duration จาก inline style */
  animation-name: floatUp;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
}
@keyframes floatUp {
  0% { transform: translateY(0) rotate(0deg); opacity: 0; }
  10% { opacity: 0.6; }
  90% { opacity: 0.6; }
  100% { transform: translateY(-120vh) rotate(360deg); opacity: 0; }
}
</style>