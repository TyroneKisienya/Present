<script setup>
import { ref, onMounted } from 'vue'

const messages = ref([
  "Dear Fatma,",
  "On your 23rd birthday, we celebrate not just another year, but the amazing person you've become.",
  "Your strength, kindness, and determination inspire everyone around you.",
  "May this year bring you more joy, success, and beautiful moments.",
  "Keep shining bright! 🌟"
])

const currentMessageIndex = ref(0)
const showMessage = ref(false)

onMounted(() => {
  setTimeout(() => {
    showMessage.value = true
    const messageInterval = setInterval(() => {
      if (currentMessageIndex.value < messages.value.length - 1) {
        currentMessageIndex.value++
      } else {
        clearInterval(messageInterval)
      }
    }, 2000)
  }, 500)
})
</script>

<template>
  <div class="inspiration-container">
    <div class="message-box" :class="{ 'show': showMessage }">
      <div v-for="(message, index) in messages.slice(0, currentMessageIndex + 1)" 
           :key="index" 
           class="message"
           :class="{ 'active': index === currentMessageIndex }">
        {{ message }}
      </div>
    </div>
    
    <div class="video-container" v-if="currentMessageIndex === messages.length - 1">
      <div class="video-placeholder">
        <p>🎥 Your Special Birthday Video</p>
        <p class="video-note">Add your personal video here</p>
      </div>
    </div>

    <div class="confetti-decoration">
      <div class="confetti" v-for="n in 10" :key="n"></div>
    </div>
  </div>
</template>

<style scoped>
.inspiration-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  position: relative;
  min-height: 80vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.message-box {
  background: linear-gradient(145deg, #ffffff, #f0f0f0);
  padding: 2rem;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.5s ease;
  width: 100%;
  max-width: 600px;
  margin-bottom: 2rem;
}

.message-box.show {
  opacity: 1;
  transform: translateY(0);
}

.message {
  font-size: 1.2rem;
  margin: 1rem 0;
  line-height: 1.6;
  color: #333;
  opacity: 0;
  transform: translateX(-20px);
  animation: slideIn 0.5s forwards;
}

.message.active {
  font-weight: bold;
  color: #ff69b4;
}

.video-container {
  margin-top: 2rem;
  width: 100%;
  max-width: 560px;
  opacity: 0;
  animation: fadeIn 1s forwards;
}

.video-placeholder {
  background: linear-gradient(145deg, #ff69b4, #ff8dc7);
  padding: 2rem;
  border-radius: 10px;
  color: white;
  text-align: center;
  min-height: 315px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.video-note {
  font-size: 0.9rem;
  opacity: 0.8;
  margin-top: 1rem;
}

.confetti-decoration {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: hidden;
}

.confetti {
  position: absolute;
  width: 10px;
  height: 10px;
  background: #ff69b4;
  animation: confettiFall 3s linear infinite;
}

.confetti:nth-child(2n) {
  background: #FFD700;
}

.confetti:nth-child(3n) {
  background: #4CAF50;
}

@keyframes slideIn {
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes fadeIn {
  to {
    opacity: 1;
  }
}

@keyframes confettiFall {
  0% {
    transform: translateY(-100%) rotate(0deg);
    opacity: 1;
  }
  100% {
    transform: translateY(100vh) rotate(360deg);
    opacity: 0;
  }
}

/* Position confetti pieces randomly */
.confetti:nth-child(1) { left: 10%; animation-delay: 0s; }
.confetti:nth-child(2) { left: 20%; animation-delay: 0.5s; }
.confetti:nth-child(3) { left: 30%; animation-delay: 1s; }
.confetti:nth-child(4) { left: 40%; animation-delay: 1.5s; }
.confetti:nth-child(5) { left: 50%; animation-delay: 2s; }
.confetti:nth-child(6) { left: 60%; animation-delay: 2.5s; }
.confetti:nth-child(7) { left: 70%; animation-delay: 3s; }
.confetti:nth-child(8) { left: 80%; animation-delay: 3.5s; }
.confetti:nth-child(9) { left: 90%; animation-delay: 4s; }
.confetti:nth-child(10) { left: 95%; animation-delay: 4.5s; }
</style>