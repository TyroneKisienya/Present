<script setup>
import { ref } from 'vue'
import confetti from 'canvas-confetti'

// eslint-disable-next-line no-undef
const emit = defineEmits(['balloons-complete'])

const popSound = new Audio(new URL('../assets/pop.mp3', import.meta.url))

const balloons = ref([
  { id: 1, visible: true, popped: false }
])

const currentBalloonIndex = ref(0)

const popBalloon = () => {
  const currentBalloon = balloons.value[currentBalloonIndex.value]
  
  if (!currentBalloon.popped) {
    // Play pop sound
    popSound.play()

    // Trigger confetti
    confetti({
      particleCount: 100,
      spread: 70,
      origin: { y: 0.6 }
    })

    // Mark current balloon as popped
    currentBalloon.popped = true

    // If not the last balloon, add next balloon
    if (currentBalloonIndex.value < 22) {
      balloons.value.push({ 
        id: currentBalloon.id + 1, 
        visible: true, 
        popped: false 
      })
      currentBalloonIndex.value++
    } else {
      emit('balloons-complete')
    }
  }
}
</script>

<template>
  <div class="balloon-container">
    <div 
      v-for="balloon in balloons" 
      :key="balloon.id"
      :class="[
        'balloon', 
        { 'popped': balloon.popped },
        { 'floating': !balloon.popped }
      ]"
      @click="popBalloon"
    >
      <span v-if="!balloon.popped">{{ balloon.id }}</span>
    </div>
  </div>
</template>

<style scoped>
.balloon-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  position: relative;
}

.balloon {
  width: 100px;
  height: 130px;
  background-color: #ff6b6b;
  border-radius: 50% 50% 0 0 / 60% 60% 0 0;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  position: absolute;
  color: white;
  font-weight: bold;
  transition: all 0.3s ease;
}

.balloon::before {
  content: '';
  position: absolute;
  bottom: -20px;
  left: 50%;
  transform: translateX(-50%);
  width: 2px;
  height: 20px;
  background-color: #333;
}

.floating {
  animation: float 2s ease-in-out infinite alternate;
}

@keyframes float {
  0% { transform: translateY(0); }
  100% { transform: translateY(-20px); }
}

.popped {
  background-color: transparent;
  border: 2px dashed #ccc;
  transform: scale(0.8);
  cursor: not-allowed;
}
</style>