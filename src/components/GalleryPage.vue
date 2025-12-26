<script setup>
import { ref, onMounted, computed } from 'vue'

// 🖼️ นำเข้ารูปภาพทั้ง 10 รูป
import img1 from '../assets/photo1.jpg'
import img2 from '../assets/photo2.jpg'
import img3 from '../assets/photo3.jpg'
import img4 from '../assets/photo4.jpg'
import img5 from '../assets/photo5.jpg'
import img6 from '../assets/photo6.jpg'
import img7 from '../assets/photo7.jpg'
import img8 from '../assets/photo8.jpg'
import img9 from '../assets/photo9.jpg'
import img10 from '../assets/photo10.jpg'

const emit = defineEmits(['back'])

// รวมรูปทั้งหมด
const allPhotos = [
  img1, img2, img3, img4, img5, 
  img6, img7, img8, img9, img10
]

const currentPhotoIndex = ref(0)
const shownIndices = ref([]) 
const ratings = ref({}) 
const isFinished = ref(false)

// ฟังก์ชันสุ่มรูป
const nextPhoto = () => {
  if (shownIndices.value.length >= allPhotos.length) {
    isFinished.value = true
    return
  }

  let availableIndices = allPhotos
    .map((_, idx) => idx)
    .filter(idx => !shownIndices.value.includes(idx))

  const randomIndex = availableIndices[Math.floor(Math.random() * availableIndices.length)]
  
  currentPhotoIndex.value = randomIndex
  shownIndices.value.push(randomIndex)
}

const ratePhoto = (score) => {
  ratings.value[currentPhotoIndex.value] = score
}

// คำนวณการจัดอันดับ
const rankedPhotos = computed(() => {
  return allPhotos.map((img, index) => ({
    img,
    score: ratings.value[index] || 0,
    originalIndex: index
  })).sort((a, b) => b.score - a.score)
})

const restartGallery = () => {
  shownIndices.value = []
  ratings.value = {}
  isFinished.value = false
  nextPhoto()
}

// ✨ ฟังก์ชันเลือกสีตามอันดับ
const getRankColor = (index) => {
  switch(index) {
    case 0: return 'bg-yellow-400 text-white shadow-yellow-200' // 🥇 ทอง
    case 1: return 'bg-slate-400 text-white shadow-slate-200'   // 🥈 เงิน
    case 2: return 'bg-orange-400 text-white shadow-orange-200' // 🥉 ทองแดง
    default: return 'bg-gray-100 text-gray-400'                 // ⚪ ทั่วไป
  }
}

onMounted(() => {
  nextPhoto()
})
</script>

<template>
  <div class="min-h-screen flex flex-col items-center justify-center p-4 bg-[#fff5f7] font-sans relative overflow-hidden">
    
    <div class="absolute inset-0 pointer-events-none fixed">
      <div class="floating-emoji text-4xl" style="left: 10%; animation-delay: 0s;">📸</div>
      <div class="floating-emoji text-3xl" style="left: 80%; animation-delay: 2s;">💖</div>
      <div class="floating-emoji text-5xl" style="left: 30%; animation-delay: 4s;">✨</div>
      <div class="floating-emoji text-4xl" style="left: 60%; animation-delay: 1s;">🎞️</div>
    </div>

    <button 
      v-if="!isFinished"
      @click="emit('back')" 
      class="absolute top-6 left-6 z-50 w-10 h-10 bg-white/80 backdrop-blur rounded-full shadow-md flex items-center justify-center text-pink-500 hover:scale-110 transition-transform"
    >
      ❮
    </button>

    <div v-if="!isFinished" class="bg-white p-4 pb-8 rounded-lg shadow-xl rotate-1 hover:rotate-0 transition-transform duration-300 max-w-sm w-full relative z-10">
      
      <div class="aspect-[4/5] w-full bg-gray-100 rounded overflow-hidden mb-4 relative group">
        <img 
          :src="allPhotos[currentPhotoIndex]" 
          class="w-full h-full object-cover animate-fade-in"
          :key="currentPhotoIndex" 
          alt="Memory"
        />
      </div>

      <div class="text-center space-y-4">
        <div class="flex justify-between items-end px-2">
           <p class="text-gray-400 text-xs font-medium">Memory Gallery</p>
          <p class="text-pink-400 text-xs font-bold bg-pink-50 px-2 py-1 rounded-full">
            {{ shownIndices.length }} / {{ allPhotos.length }}
          </p>
        </div>
        
        <h3 class="text-xl font-bold text-gray-700">รูปนี้ให้กี่คะแนน? 🥰</h3>
        
        <div class="flex justify-center gap-2">
          <button 
            v-for="star in 5" 
            :key="star"
            @click="ratePhoto(star)"
            class="text-3xl transition-all hover:scale-125 focus:outline-none transform active:scale-90"
            :class="ratings[currentPhotoIndex] >= star ? 'text-yellow-400 drop-shadow-sm' : 'text-gray-200'"
          >
            ★
          </button>
        </div>

        <p class="text-sm text-pink-500 font-medium min-h-[24px]">
          <span v-if="ratings[currentPhotoIndex]">
            {{ ratings[currentPhotoIndex] === 5 ? 'ว้าววว! เต็ม 10 ไม่หัก! ❤️🔥' : `ให้ ${ratings[currentPhotoIndex]} คะแนน!` }}
          </span>
        </p>

        <button 
          @click="nextPhoto"
          class="w-full bg-gradient-to-r from-[#ff6b8b] to-[#ff8fa7] text-white py-3 rounded-full font-bold shadow-lg hover:shadow-pink-300/50 transition-all active:scale-95 flex items-center justify-center gap-2 mt-4"
        >
          {{ shownIndices.length === allPhotos.length ? 'ดูสรุปผล 🏆' : 'รูปต่อไป ➡️' }}
        </button>
      </div>
    </div>

    <div v-else class="w-full max-w-md bg-white/90 backdrop-blur-sm rounded-2xl shadow-2xl p-6 z-20 animate-slide-up max-h-[80vh] flex flex-col">
      <h2 class="text-2xl font-bold text-center text-pink-600 mb-6 flex items-center justify-center gap-2">
        🏆 อันดับรูปที่ชอบ 🏆
      </h2>
      
      <div class="overflow-y-auto custom-scrollbar pr-2 space-y-3 flex-grow">
        <div 
          v-for="(item, index) in rankedPhotos" 
          :key="index"
          class="flex items-center gap-4 p-3 bg-white rounded-xl shadow-sm border border-pink-100 transition-transform hover:scale-[1.02]"
        >
          <div class="flex-shrink-0 w-8 h-8 flex items-center justify-center rounded-full font-bold shadow-sm"
               :class="getRankColor(index)">
            {{ index + 1 }}
          </div>

          <div class="w-16 h-16 rounded-lg overflow-hidden flex-shrink-0 border border-gray-100">
            <img :src="item.img" class="w-full h-full object-cover" />
          </div>

          <div class="flex-grow">
             <div class="flex text-yellow-400 text-sm mb-1">
               <span v-for="s in item.score" :key="s">★</span>
               <span v-for="s in (5 - item.score)" :key="'empty'+s" class="text-gray-200">★</span>
             </div>
             <p class="text-xs text-gray-500 font-medium">
               {{ item.score === 5 ? 'ชอบที่สุดเลย! 😍' : item.score + ' คะแนน' }}
             </p>
          </div>
        </div>
      </div>

      <div class="mt-6 flex gap-3">
        <button 
          @click="restartGallery"
          class="flex-1 bg-gray-100 text-gray-600 py-3 rounded-full font-bold hover:bg-gray-200 transition-colors"
        >
          เล่นใหม่ 🔄
        </button>
        <button 
          @click="emit('back')"
          class="flex-1 bg-[#ff6b8b] text-white py-3 rounded-full font-bold shadow-lg hover:bg-[#ff5c82] transition-colors"
        >
          กลับเมนูหลัก 🏠
        </button>
      </div>
    </div>

  </div>
</template>

<style scoped>
.animate-fade-in { animation: fadeIn 0.6s cubic-bezier(0.4, 0, 0.2, 1); }
@keyframes fadeIn { from { opacity: 0; transform: scale(1.05); } to { opacity: 1; transform: scale(1); } }

.animate-slide-up { animation: slideUp 0.5s ease-out; }
@keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

.floating-emoji { position: absolute; bottom: -50px; opacity: 0.6; animation: floatUp 15s linear infinite; }
@keyframes floatUp { 0% { transform: translateY(0) rotate(0deg); opacity: 0; } 10% { opacity: 0.6; } 90% { opacity: 0.6; } 100% { transform: translateY(-110vh) rotate(360deg); opacity: 0; } }

.custom-scrollbar::-webkit-scrollbar { width: 4px; }
.custom-scrollbar::-webkit-scrollbar-track { background: transparent; }
.custom-scrollbar::-webkit-scrollbar-thumb { background: #ffb6c1; border-radius: 10px; }
</style>