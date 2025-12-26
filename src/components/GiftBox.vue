<script setup>
import { ref } from 'vue'

const emit = defineEmits(['next'])
const isOpening = ref(false)

const openGift = () => {
  isOpening.value = true
  // รอ Animation จบ 1.5 วิ แล้วค่อยเปลี่ยนหน้า
  setTimeout(() => {
    emit('next')
  }, 1500)
}
</script>

<template>
  <div class="flex flex-col items-center justify-center min-h-screen px-4 select-none">
    
    <h2 
      :class="[
        'text-2xl md:text-4xl font-bold text-pink-500 mb-8 text-center transition-all duration-500',
        isOpening ? 'opacity-0 -translate-y-4' : 'opacity-100'
      ]"
    >
      ของขวัญสำหรับคนน่ารัก
    </h2>

    <div 
      @click="openGift"
      :class="[
        'text-[10rem] md:text-[15rem] leading-none cursor-pointer transition-all duration-1500 ease-in-out transform filter drop-shadow-2xl',
        isOpening ? 'scale-150 opacity-0 rotate-12' : 'hover:scale-110 hover:-rotate-6 animate-float'
      ]"
      role="img"
      aria-label="Gift box"
    >
      🎁
    </div>

    <p 
      :class="[
        'text-pink-400 mt-12 text-xl font-medium transition-all duration-500 flex items-center gap-2',
        isOpening ? 'opacity-0 translate-y-4' : 'animate-bounce opacity-100'
      ]"
    >
      <span>👆</span> แตะที่กล่องเพื่อเปิด
    </p>

  </div>
</template>

<style scoped>
/* Animation ลอยขึ้นลงเบาๆ */
@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-15px); }
}

.animate-float {
  animation: float 3s ease-in-out infinite;
}
</style>