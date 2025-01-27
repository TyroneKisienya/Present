<script setup>
import { ref, onMounted } from 'vue';

const videoLoaded = ref(false);

const handleVideoLoad = () => {
  videoLoaded.value = true;
};

onMounted(() => {
  // Auto-play the video when component mounts
  const video = document.getElementById('celebration-video');
  if (video) {
    video.play().catch(error => {
      console.log('Auto-play was prevented:', error);
    });
  }
});
</script>

<template>
  <div class="inspiration-container">
    <div class="message-content">
      <h1 class="title">Happy Birthday Fatma</h1>
      <div class="video-container">
        <video
          id="celebration-video"
          @loadeddata="handleVideoLoad"
          controls
          loop
          class="celebration-video"
        >
          <source src="../assets/celebration.mp4" type="video/mp4">
          Your browser does not support the video tag.
        </video>
      </div>
      <div class="message-text">
        <p class="birthday-message">
          May your day be filled with joy, laughter, and beautiful moments.
          Here's to another amazing year ahead! 🎉
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.inspiration-container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #ff9a9e 0%, #fad0c4 100%);
  padding: 2rem;
}

.message-content {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20px;
  padding: 2rem;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  max-width: 800px;
  width: 100%;
  text-align: center;
}

.title {
  font-size: 3rem;
  color: #ff69b4;
  margin-bottom: 2rem;
  font-family: 'Pacifico', cursive;
  animation: fadeIn 1.5s ease-out;
}

.video-container {
  margin: 2rem 0;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

.celebration-video {
  width: 100%;
  max-height: 400px;
  object-fit: cover;
}

.birthday-message {
  font-size: 1.5rem;
  line-height: 1.6;
  color: #666;
  margin-top: 2rem;
  animation: slideUp 1s ease-out;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from {
    transform: translateY(20px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}
</style>