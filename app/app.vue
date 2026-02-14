<script setup>
import { ref, computed } from 'vue'

const isAccepted = ref(false)
const noButtonPosition = ref({ top: '0px', left: '0px' })
const hasMoved = ref(false)
const moveCount = ref(0)

// Sweet messages that change as they try to click "No"
const messages = [
  "I have a special surprise for you...",
  "Wait, did you misclick? 🥺",
  "Think about the tacos we could have! 🌮",
  "I'll be the happiest person alive, I promise!",
  "Is that your final answer? 💔",
  "You're breaking my heart! Just click Yes!",
]

const currentMessage = computed(() => {
  return messages[Math.min(moveCount.value, messages.length - 1)]
})

const moveButton = () => {
  hasMoved.value = true
  moveCount.value++
  
  const x = Math.random() * (window.innerWidth - 120)
  const y = Math.random() * (window.innerHeight - 60)
  
  noButtonPosition.value = { 
    top: `${y}px`, 
    left: `${x}px` 
  }
}
watch(isAccepted, (newVal) => {
  if (newVal) {
    useHead({
      title: 'SHE SAID YES! 🎉'
    })
  }
})
</script>

<template>
  <div class="h-screen w-full bg-gradient-to-br from-pink-100 via-red-50 to-pink-200 flex flex-col items-center justify-center font-sans overflow-hidden relative">
    
    <div class="absolute inset-0 pointer-events-none opacity-20">
      <div v-for="n in 10" :key="n" 
           class="absolute animate-pulse"
           :style="{ 
             top: Math.random() * 100 + '%', 
             left: Math.random() * 100 + '%',
             animationDelay: n + 's' 
           }">
        ❤️
      </div>
    </div>

    <div v-if="!isAccepted" class="text-center p-12 bg-white/80 backdrop-blur-md rounded-[40px] shadow-2xl border-4 border-white z-10 max-w-md mx-4">
      <div class="text-7xl mb-6 drop-shadow-sm animate-bounce">✨💖✨</div>
      
      <h1 class="text-4xl font-extrabold text-gray-800 mb-2">
        My Dearest,
      </h1>
      <h2 class="text-3xl font-bold text-red-500 mb-6 leading-tight">
        Will you be my Valentine?
      </h2>
      
      <p class="text-lg text-pink-600 font-medium mb-10 min-h-[1.5rem] italic transition-all duration-300">
        "{{ currentMessage }}"
      </p>
      
      <div class="flex gap-6 justify-center items-center">
        <button 
          @click="isAccepted = true"
          class="bg-gradient-to-r from-red-500 to-pink-500 hover:from-red-600 hover:to-pink-600 text-white font-black py-4 px-10 rounded-2xl transition-all transform hover:scale-125 active:scale-90 shadow-xl"
        >
          YES! 💍
        </button>

        <button 
          @mouseover="moveButton"
          @click="moveButton"
          :style="hasMoved ? { position: 'fixed', ...noButtonPosition } : {}"
          class="bg-gray-200 text-gray-500 font-bold py-3 px-6 rounded-xl transition-all duration-200 hover:bg-gray-300"
        >
          No
        </button>
      </div>
    </div>

    <div v-else class="text-center p-8 z-10">
      <div class="relative inline-block">
        <h1 class="text-8xl font-black text-red-600 drop-shadow-xl animate-bounce">YES! ❤️</h1>
        <div class="absolute -top-10 -right-10 text-5xl rotate-12">🍭</div>
        <div class="absolute -bottom-5 -left-10 text-5xl -rotate-12">🎀</div>
      </div>
      
      <div class="mt-12 space-y-4">
        <p class="text-3xl font-serif text-gray-800 italic">
          "My heart just skipped a beat..."
        </p>
        <p class="text-xl text-pink-700 font-light">
          You’ve made me the luckiest person in the world. <br>
          I can't wait to spend this day with you! I love you so much! ❤️
        </p>
      </div>

      <div class="mt-12 flex justify-center gap-4">
        <span class="text-6xl animate-pulse">🌹</span>
        <span class="text-6xl animate-pulse delay-75">🍫</span>
        <span class="text-6xl animate-pulse delay-150">🥂</span>
      </div>
    </div>

  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&display=swap');

body { 
  margin: 0; 
  padding: 0;
}

h1 {
  font-family: 'Dancing Script', cursive;
}

.animate-pulse {
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 1; transform: scale(1); }
  50% { opacity: .7; transform: scale(1.1); }
}
</style>