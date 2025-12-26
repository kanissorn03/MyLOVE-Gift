<script setup>
import { ref, watch } from 'vue'

const emit = defineEmits(['unlock'])
const password = ref('')
const isError = ref(false)

// 🔑 แก้รหัสผ่านตรงนี้ (6 หลัก)
const CORRECT_PASSWORD = '141022' 

const handleInput = (e) => {
  const val = e.target.value.replace(/\D/g, '')
  password.value = val.slice(0, 6)
  isError.value = false
}

const checkPassword = () => {
  if (password.value === CORRECT_PASSWORD) {
    emit('unlock')
  } else {
    isError.value = true
    password.value = ''
    setTimeout(() => isError.value = false, 1000)
  }
}

// เช็กอัตโนมัติเมื่อครบ 6 หลัก
watch(password, (newVal) => {
  if (newVal.length === 6) {
    checkPassword()
  }
})
</script>

<template>
  <div class="relative flex flex-col items-center justify-center min-h-screen overflow-hidden bg-pink-50 font-sans">
    
    <div class="absolute inset-0 pointer-events-none">
      <div class="floating-heart" style="left: 10%; animation-delay: 0s;">💖</div>
      <div class="floating-heart" style="left: 30%; animation-delay: 2s;">💕</div>
      <div class="floating-heart" style="left: 70%; animation-delay: 1s;">💗</div>
      <div class="floating-heart" style="left: 90%; animation-delay: 1s;">✨</div>
      <div class="floating-heart" style="left: 50%; animation-delay: 3s;">💝</div>
    </div>

    <div class="z-10 bg-[#ff6b8b] p-8 md:p-10 rounded-[2.5rem] shadow-[0_20px_50px_rgba(255,107,139,0.3)] w-full max-w-md text-center mx-4 border-4 border-pink-300/30 backdrop-blur-sm animate-popup">
      
      <div class="mb-6">
        <h2 class="text-3xl font-bold text-white mb-2 drop-shadow-md">
          กรอกรหัสผ่าน
        </h2>
        <p class="text-pink-100 text-sm font-medium opacity-90">
          ( คำใบ้ : วันครบรอบของเรา )
        </p>
      </div>

      <div class="relative h-16 md:h-20 mb-6">
        <input 
          type="tel"
          :value="password"
          @input="handleInput"
          class="absolute inset-0 w-full h-full opacity-0 cursor-pointer z-20 tracking-widest"
          maxlength="6"
          autofocus
        />

        <div class="flex justify-center gap-2 md:gap-3 absolute inset-0 z-10 pointer-events-none">
          <div 
            v-for="i in 6" 
            :key="i"
            :class="[
              'w-10 h-14 md:w-14 md:h-16 bg-white rounded-2xl flex items-center justify-center text-2xl font-bold transition-all duration-300 shadow-inner',
              // Effect เมื่อพิมพ์หรือ error
              isError ? 'border-2 border-red-300 bg-red-50 animate-shake text-red-500' : 
              password.length === i - 1 ? 'scale-110 ring-4 ring-pink-300/50' : ''
            ]"
          >
            <span v-if="password[i-1]" class="text-[#ff6b8b] animate-pop">
              {{ password[i-1] }}
            </span>
            <div v-else-if="password.length === i - 1" class="w-1 h-6 bg-pink-200 rounded-full animate-pulse"></div>
          </div>
        </div>
      </div>

      <div class="h-6">
        <p v-if="isError" class="text-white text-sm font-bold bg-red-400/80 px-3 py-1 rounded-full inline-block animate-bounce shadow-sm">
          ❌ รหัสผิดนะจ๊ะคนดี
        </p>
      </div>

    </div>
  </div>
</template>

<style scoped>
/* Animation หัวใจลอย */
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

/* Animation การ์ดเด้งขึ้นมา */
.animate-popup {
  animation: popup 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
}

@keyframes popup {
  from { transform: scale(0.8) translateY(20px); opacity: 0; }
  to { transform: scale(1) translateY(0); opacity: 1; }
}

/* Animation ตัวเลขเด้ง */
.animate-pop {
  animation: pop 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

@keyframes pop {
  from { transform: scale(0); }
  to { transform: scale(1); }
}

/* Animation สั่นเมื่อผิด */
.animate-shake {
  animation: shake 0.4s cubic-bezier(.36,.07,.19,.97) both;
}

@keyframes shake {
  10%, 90% { transform: translateX(-1px); }
  20%, 80% { transform: translateX(2px); }
  30%, 50%, 70% { transform: translateX(-4px); }
  40%, 60% { transform: translateX(4px); }
}
</style>