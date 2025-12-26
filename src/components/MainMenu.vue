<script setup>
import { computed } from 'vue'

const emit = defineEmits(['select'])

// --- 1. ระบบทักทาย (เหมือนเดิม) ---
const timeGreeting = computed(() => {
  const hour = new Date().getHours()
  if (hour < 12) return 'อรุณสวัสดิ์ค้าบ ☀️'
  if (hour < 18) return 'สวัสดีตอนบ่ายคับ 🌤️'
  return 'สวัสดีตอนค่ำงับ 🌙'
})

// --- 2. ปรับสีปุ่มให้เป็น "พื้นขาว" ตัดขอบสี (ให้ดูเด่นขึ้น) ---
const menus = [
  { 
    id: 'timer', 
    label: 'Timer', 
    subLabel: 'คบกันนานเท่าไหร่แล้ว', 
    icon: '📅', 
    // พื้นขาว + เงาสีชมพู + ขอบชมพู
    cardClass: 'bg-white text-pink-600 shadow-pink-200/50 border-pink-100 hover:border-pink-300',
    iconBg: 'bg-pink-100',
    delay: '0ms'
  },
  { 
    id: 'letter', 
    label: 'Letter', 
    subLabel: 'จดหมายถึงเธอ', 
    icon: '💌', 
    // พื้นขาว + เงาสีม่วง + ขอบม่วง
    cardClass: 'bg-white text-purple-600 shadow-purple-200/50 border-purple-100 hover:border-purple-300',
    iconBg: 'bg-purple-100',
    delay: '100ms'
  },
  { 
    id: 'quiz', 
    label: 'Quiz', 
    subLabel: 'ทายใจกันหน่อย', 
    icon: '🎮', 
    // พื้นขาว + เงาสีส้ม + ขอบส้ม
    cardClass: 'bg-white text-orange-600 shadow-orange-200/50 border-orange-100 hover:border-orange-300',
    iconBg: 'bg-orange-100',
    delay: '200ms'
  },
  { 
    id: 'gallery', 
    label: 'Gallery', 
    subLabel: 'ความทรงจำของเรา', 
    icon: '📸', 
    // พื้นขาว + เงาสีฟ้า + ขอบฟ้า
    cardClass: 'bg-white text-blue-600 shadow-blue-200/50 border-blue-100 hover:border-blue-300',
    iconBg: 'bg-blue-100',
    delay: '300ms'
  }
]
</script>

<template>
  <div class="relative min-h-screen flex flex-col items-center justify-center p-6 overflow-hidden bg-[#faf5ff] font-sans selection:bg-pink-200">
    
    <div class="fixed inset-0 pointer-events-none">
      <div class="absolute top-[-10%] left-[-10%] w-96 h-96 bg-pink-300/20 rounded-full blur-[100px] animate-pulse-slow"></div>
      <div class="absolute bottom-[-10%] right-[-10%] w-96 h-96 bg-purple-300/20 rounded-full blur-[100px] animate-pulse-slow delay-1000"></div>
    </div>

    <div class="z-10 w-full max-w-md mb-8 pl-2 animate-slide-down text-center md:text-left">
      <p class="text-gray-400 text-xs md:text-sm font-bold tracking-widest uppercase mb-2">System Menu</p>
      <h1 class="text-3xl md:text-4xl font-extrabold text-gray-800 tracking-tight drop-shadow-sm">
        {{ timeGreeting }}
      </h1>
      <p class="text-gray-500 mt-2 text-base md:text-lg font-medium">
        ยินดีต้อนรับนะคนเก่ง ❤️
      </p>
    </div>

    <div class="grid grid-cols-2 gap-4 md:gap-5 w-full max-w-md z-10">
      
      <button
        v-for="menu in menus"
        :key="menu.id"
        @click="emit('select', menu.id)"
        class="group relative h-44 md:h-48 rounded-[2rem] p-5 md:p-6 flex flex-col justify-between items-start text-left transition-all duration-300 hover:-translate-y-1 active:scale-95 border-2 shadow-xl animate-stagger"
        :class="menu.cardClass"
        :style="{ animationDelay: menu.delay }"
      >
        <div 
          class="w-12 h-12 md:w-14 md:h-14 rounded-2xl flex items-center justify-center text-2xl md:text-3xl shadow-inner mb-2 transition-transform duration-500 group-hover:scale-110 group-hover:rotate-6"
          :class="menu.iconBg"
        >
          {{ menu.icon }}
        </div>

        <div class="relative z-10">
          <h3 class="text-lg md:text-xl font-bold transition-colors">
            {{ menu.label }}
          </h3>
          <p class="text-[10px] md:text-xs text-gray-400 font-medium mt-1 group-hover:text-gray-500">
            {{ menu.subLabel }}
          </p>
        </div>

        <div class="absolute top-6 right-6 opacity-0 -translate-x-2 group-hover:opacity-100 group-hover:translate-x-0 transition-all duration-300">
          ➜
        </div>
      </button>

    </div>

    <footer class="mt-12 text-center opacity-40 text-[10px] font-bold tracking-widest text-gray-400 animate-fade-in delay-500">
      DESIGNED FOR เม่นน้อย
    </footer>

  </div>
</template>

<style scoped>
/* Animation เหมือนเดิม */
.animate-pulse-slow {
  animation: pulseSlow 6s ease-in-out infinite;
}
@keyframes pulseSlow {
  0%, 100% { transform: scale(1); opacity: 0.5; }
  50% { transform: scale(1.1); opacity: 0.8; }
}

.animate-slide-down {
  animation: slideDown 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}
@keyframes slideDown {
  from { transform: translateY(-30px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

.animate-stagger {
  animation: staggerPop 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) backwards;
}
@keyframes staggerPop {
  from { transform: scale(0.8) translateY(40px); opacity: 0; }
  to { transform: scale(1) translateY(0); opacity: 1; }
}

.animate-fade-in {
  animation: fadeIn 1s ease-out backwards;
}
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 0.6; }
}
</style>