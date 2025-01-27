<template>
  <div id="app">
    <VerificationForm v-if="!isVerified" @verification-success="handleVerification" />
    <BalloonPopping v-else-if="isVerified && !balloonsPoppedComplete" @balloons-complete="handleBalloonCompletion" />
    <CakeDecorator v-else-if="balloonsPoppedComplete" />
    <!--we'll be adding more componentsas we build -->
  </div>
</template>

<script setup>
import { ref } from 'vue'
import VerificationForm from './components/VerificationForm.vue'
import BalloonPopping from './components/BalloonPopping.vue'
import CakeDecorator from './components/CakeDecorator.vue'

const isVerified = ref(false)
const balloonsPoppedComplete = ref(false)

const handleVerification = () => {
  isVerified.value = true
}

const handleBalloonCompletion = () => {
  balloonsPoppedComplete.value = true
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>