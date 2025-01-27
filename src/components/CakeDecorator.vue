<script setup>
import { ref, onUnmounted } from 'vue';

// eslint-disable-next-line no-undef
const emit = defineEmits(['candle-blown']);

// eslint-disable-next-line no-undef
const props = defineProps({
  initialCakes: {
    type: Array,
    default: () => [
      { size: 1, frostingColor: '#8B4513', borderColor: '#6B3410' },
      { size: 0.75, frostingColor: '#FFA07A', borderColor: '#FF8C66' },
      { size: 0.5, frostingColor: '#FFD700', borderColor: '#DAA520' }
    ]
  }
});

const cakes = ref(props.initialCakes);
const candle = ref({
  flame: 100,
  blowCount: 0,
  isFlickering: false
});

let intervalId = null;

const startFlickering = () => {
  candle.value.isFlickering = true;
  intervalId = setInterval(() => {
    candle.value.flame = Math.max(0, candle.value.flame - Math.floor(Math.random() * 10));
    // Check if flame has gone out during flickering
    if (candle.value.flame === 0) {
      stopFlickering();
      emit('candle-blown');
    }
  }, 100);
};

const stopFlickering = () => {
  candle.value.isFlickering = false;
  clearInterval(intervalId);
};

const blowCandle = () => {
  candle.value.blowCount++;
  
  if (candle.value.blowCount === 1) {
    startFlickering();
  } else if (candle.value.blowCount === 2) {
    candle.value.flame = Math.max(0, candle.value.flame - 50);
  } else if (candle.value.blowCount >= 3) {
    candle.value.flame = 0;
    stopFlickering();
    emit('candle-blown');  // Emit event when candle is blown out
  }
};

const resetCandle = () => {
  candle.value = {
    flame: 100,
    blowCount: 0,
    isFlickering: false
  };
  stopFlickering();
};

onUnmounted(() => {
  stopFlickering();
});
</script>

<template>
  <div class="cake-container">
    <button @click="resetCandle" class="reset-button">
      Reset Candle
    </button>
    
    <div class="cake-scene">
      <div class="cake-and-candle">
        <div 
          class="candle" 
          :class="{ 'flickering': candle.isFlickering }"
          @click="blowCandle"
        >
          <div class="flame" v-if="candle.flame > 0">
            <div class="flame-inner"></div>
          </div>
          <div class="candle-base"></div>
        </div>

        <div class="cake-layers">
          <div
            v-for="(cake, index) in cakes"
            :key="index"
            class="cake-layer"
            :style="{
              height: `${cake.size * 100}px`,
              width: `${cake.size * 200}px`,
              backgroundColor: cake.frostingColor,
              borderColor: cake.borderColor,
              order: cakes.length - index
            }"
          >
            <div class="frosting-drips"></div>
            <div class="sprinkles" v-if="index === 0"></div>
            <div class="layer-shadow"></div>
          </div>
        </div>
        <div class="plate"></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.cake-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 500px;
}

.reset-button {
  margin-bottom: 1rem;
  padding: 0.5rem 1rem;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
  box-shadow: 0 2px 4px rgba(0,0,0,0.2);
}

.reset-button:hover {
  background-color: #45a049;
}

.cake-scene {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 400px;
}

.cake-and-candle {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.candle {
  width: 20px;
  height: 60px;
  position: absolute;
  top: -40px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 2;
  cursor: pointer;
}

.candle-base {
  width: 100%;
  height: 100%;
  background: linear-gradient(to right, #f4f4f4, #fff, #f4f4f4);
  border: 1px solid #ddd;
  border-radius: 2px;
}

.flame {
  position: absolute;
  top: -20px;
  left: 50%;
  transform: translateX(-50%);
  width: 16px;
  height: 20px;
  background: radial-gradient(#ffd700, #ff4500);
  border-radius: 50% 50% 20% 20%;
  animation: flicker 0.3s infinite alternate;
}

.flame-inner {
  position: absolute;
  top: 30%;
  left: 50%;
  transform: translateX(-50%);
  width: 8px;
  height: 12px;
  background: #fff;
  border-radius: 50% 50% 20% 20%;
  opacity: 0.7;
}

.cake-layers {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  position: relative;
}

.cake-layer {
  border-radius: 10px;
  position: relative;
  transition: all 0.3s ease;
  border: 2px solid;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  overflow: hidden;
}

.layer-shadow {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(to bottom, rgba(0,0,0,0.1), transparent);
}

.frosting-drips {
  position: absolute;
  top: -5px;
  left: 0;
  right: 0;
  height: 10px;
  background-image: 
    radial-gradient(circle at 10% 0, transparent 20%, currentColor 20%, currentColor 40%, transparent 40%),
    radial-gradient(circle at 30% 0, transparent 20%, currentColor 20%, currentColor 40%, transparent 40%),
    radial-gradient(circle at 50% 0, transparent 20%, currentColor 20%, currentColor 40%, transparent 40%),
    radial-gradient(circle at 70% 0, transparent 20%, currentColor 20%, currentColor 40%, transparent 40%),
    radial-gradient(circle at 90% 0, transparent 20%, currentColor 20%, currentColor 40%, transparent 40%);
  opacity: 0.3;
}

.sprinkles {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: 
    radial-gradient(circle, #ff69b4 2px, transparent 2px),
    radial-gradient(circle, #4CAF50 2px, transparent 2px),
    radial-gradient(circle, #FFD700 2px, transparent 2px);
  background-size: 30px 30px;
  background-position: 0 0, 15px 15px, 7px 7px;
  opacity: 0.6;
}

.plate {
  width: 250px;
  height: 20px;
  background: linear-gradient(to bottom, #f0f0f0, #fff);
  border-radius: 50%;
  margin-top: 10px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

@keyframes flicker {
  0% { transform: translateX(-50%) scale(1); opacity: 1; }
  100% { transform: translateX(-50%) scale(0.9); opacity: 0.8; }
}

.flickering .flame {
  animation: flicker 0.15s infinite alternate;
}
</style>