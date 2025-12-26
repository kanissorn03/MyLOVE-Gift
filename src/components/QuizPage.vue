<script setup>
import { ref, computed } from 'vue'

const emit = defineEmits(['back'])

// 📝 ข้อมูลคำถาม-คำตอบ
const questions = ref([
  {
    question: "ดอกไม้ที่เตงชอบ?",
    options: [
      "ดอกทิวลิป", 
      "ดอกทานตะวัน",   // (Index 1) ถูกต้อง
      "ดอกกุหลาบ", 
      "ดอกเดซี่"
    ],
    correct: 1 
  },
  {
    question: "สีที่เธอชอบคือ?",
    options: [
      "สีเหลือง",      // (Index 0) ถูกต้อง
      "สีแดงอมเปรี้ยว", 
      "สีเขียวอมขม", 
      "สีดำอมเหลือง"
    ],
    correct: 0 
  },
  {
    question: "เธอชอบกินอะไรสุด?",
    options: [
      "แกงส้มปลากระเบน", 
      "ผัดเผ็ดหนูนา", 
      "พิชซ่าหน้าอึ่ง", 
      "ข้าวผัด แสนอร่อย" // (Index 3) ถูกต้อง
    ],
    correct: 3
  },
  {
    question: "อยากไปเที่ยวกันต่อที่ไหน?",
    options: [
      "ทะเล", 
      "คาเฟ่สัตว์", 
      "คาเฟ่น่ารักๆ", 
      "ห้องพี่เองครับน้อง"
    ],
    correct: 'any' // ✨ พิเศษ: ใส่ 'any' เพื่อบอกว่าถูกทุกข้อ
  }
])

const currentQuestionIndex = ref(0)
const score = ref(0)
const showScore = ref(false)
const selectedAnswer = ref(null)

const currentQuestion = computed(() => questions.value[currentQuestionIndex.value])

const handleAnswer = (index) => {
  selectedAnswer.value = index
  
  // เช็กคำตอบ (ถ้า correct เป็น 'any' หรือ index ตรงกับเฉลย ให้ถือว่าถูก)
  if (currentQuestion.value.correct === 'any' || index === currentQuestion.value.correct) {
    score.value++
  }

  // รอ 1 วินาทีแล้วไปข้อต่อไป
  setTimeout(() => {
    selectedAnswer.value = null
    if (currentQuestionIndex.value < questions.value.length - 1) {
      currentQuestionIndex.value++
    } else {
      showScore.value = true
    }
  }, 1000)
}

const restartQuiz = () => {
  currentQuestionIndex.value = 0
  score.value = 0
  showScore.value = false
  selectedAnswer.value = null
}

// ข้อความประเมินผล
const resultMessage = computed(() => {
  if (score.value === questions.value.length) return "สุดยอด! แฟนเค้าเก่งที่สุดดด 💖🥇"
  if (score.value >= questions.value.length / 2) return "เก่งมาก!แต่ไม่มากพอนะ 🥺"
  return "แงงง~ จำไม่ได้หรอ งอนนะ! 🥺💔"
})

// ฟังก์ชันเช็กว่าข้อนี้ตอบถูกไหม (เพื่อเปลี่ยนสีปุ่ม)
const isCorrect = (index) => {
  return currentQuestion.value.correct === 'any' || index === currentQuestion.value.correct
}
</script>

<template>
  <div class="min-h-screen flex flex-col items-center justify-center p-4 bg-[#fff0f5] font-sans relative overflow-hidden">
    
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

    <div class="w-full max-w-md bg-white rounded-2xl shadow-xl p-8 relative z-10 min-h-[400px] flex flex-col justify-center">
      
      <div v-if="showScore" class="text-center space-y-6 animate-pop-in">
        <h2 class="text-3xl font-bold text-pink-500">จบเกมแล้ว! 🎉</h2>
        <div class="text-6xl font-black text-[#ff6b8b] drop-shadow-sm">
          {{ score }} / {{ questions.length }}
        </div>
        <p class="text-xl text-gray-600 font-medium">
          {{ resultMessage }}
        </p>
        
        <div class="flex justify-center mt-8">
          <button 
            @click="restartQuiz"
            class="px-8 py-3 bg-[#ff6b8b] text-white rounded-full hover:bg-[#ff5c82] transition-colors font-bold shadow-lg shadow-pink-200 flex items-center gap-2"
          >
            <span>🔄</span> เล่นใหม่
          </button>
        </div>
      </div>

      <div v-else class="space-y-6">
        <div class="w-full bg-gray-100 rounded-full h-2.5 mb-6">
          <div 
            class="bg-[#ff6b8b] h-2.5 rounded-full transition-all duration-500" 
            :style="{ width: `${((currentQuestionIndex + 1) / questions.length) * 100}%` }"
          ></div>
        </div>

        <h3 class="text-xl md:text-2xl font-bold text-gray-800 text-center leading-relaxed">
          ข้อที่ {{ currentQuestionIndex + 1 }} : <br>
          <span class="text-pink-500">{{ currentQuestion.question }}</span>
        </h3>

        <div class="space-y-3 mt-4">
          <button 
            v-for="(option, index) in currentQuestion.options" 
            :key="index"
            @click="handleAnswer(index)"
            :disabled="selectedAnswer !== null"
            class="w-full p-4 rounded-xl border-2 text-left transition-all duration-200 font-medium text-gray-600 relative overflow-hidden group"
            :class="[
              selectedAnswer === null 
                ? 'border-pink-100 hover:border-pink-300 hover:bg-pink-50' 
                : isCorrect(index) 
                  ? 'border-green-400 bg-green-50 text-green-700' 
                  : selectedAnswer === index 
                    ? 'border-red-400 bg-red-50 text-red-700' 
                    : 'border-gray-100 opacity-50'
            ]"
          >
            {{ option }}
            
            <span v-if="selectedAnswer !== null && isCorrect(index)" class="absolute right-4 top-1/2 -translate-y-1/2 text-green-500 text-xl">✓</span>
            <span v-if="selectedAnswer === index && !isCorrect(index)" class="absolute right-4 top-1/2 -translate-y-1/2 text-red-500 text-xl">✗</span>
          </button>
        </div>
      </div>
    </div>

  </div>
</template>

<style scoped>
.animate-pop-in {
  animation: popIn 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

@keyframes popIn {
  0% { transform: scale(0.8); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}

/* ✨✨ เพิ่ม CSS สำหรับหัวใจลอย ✨✨ */
.floating-heart {
  position: absolute;
  bottom: -100px;
  opacity: 0.6;
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