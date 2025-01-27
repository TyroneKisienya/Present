// VerificationForm.vue
<script setup>
import { ref, onMounted } from 'vue'

// eslint-disable-next-line no-undef
const emit = defineEmits(['verification-success'])

const verificationSteps = ref([
  "Are you Fatma!",
  "Prove it...",
  "How old are you today?"
])
const currentStep = ref(0)
const age = ref('')
const showError = ref(false)
const isVerified = ref(false)

const revealNextStep = () => {
  if (currentStep.value < verificationSteps.value.length - 1) {
    currentStep.value++
  }
}

const verifyIdentity = () => {
  if (parseInt(age.value) === 23) {
    isVerified.value = true
    showError.value = false
    emit('verification-success')
  } else {
    showError.value = true
    age.value = ''
  }
}

onMounted(() => {
  const stepReveal = setInterval(() => {
    revealNextStep()
    if (currentStep.value === verificationSteps.value.length - 1) {
      clearInterval(stepReveal)
    }
  }, 1500)
})
</script>

<template>
  <div class="verification-container">
    <div v-if="!isVerified" class="form-container">
      <div class="verification-steps">
        <h2 v-for="(step, index) in verificationSteps.slice(0, currentStep + 1)" 
            :key="index" 
            class="step">
          {{ step }}
        </h2>
      </div>

      <form v-if="currentStep === verificationSteps.length - 1" 
            @submit.prevent="verifyIdentity" 
            class="form">
        <div class="input-group">
          <input 
            v-model="age"
            type="number"
            required
            placeholder="Enter your age"
            class="input"
          >
        </div>

        <button type="submit" class="submit-btn">
          Verify
        </button>
      </form>

      <div v-if="showError" class="error-message">
        You are not her 😊
      </div>
    </div>

    <div v-else class="success-message">
      <h2>Verification Successful!</h2>
    </div>
  </div>
</template>

<style scoped>
.verification-container {
  max-width: 500px;
  margin: 50px auto;
  padding: 20px;
  text-align: center;
}

.verification-steps {
  margin-bottom: 30px;
}

.step {
  opacity: 0;
  animation: fadeIn 1s forwards;
  margin: 15px 0;
  color: #333;
}

@keyframes fadeIn {
  to { opacity: 1; }
}

.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.input {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 16px;
  width: 200px;
}

.submit-btn {
  background: #ff69b4;
  color: white;
  padding: 12px 24px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background 0.3s;
}

.error-message {
  color: #ff4444;
  margin-top: 20px;
}
</style>