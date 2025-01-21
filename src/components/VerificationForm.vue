// VerificationForm.vue
<script setup>
import { ref } from 'vue'

const name = ref('')
const age = ref('')
const showError = ref(false)
const isVerified = ref(false)

const verifyIdentity = () => {
  if (name.value.toLowerCase() === 'fatma' && parseInt(age.value) === 23) {
    isVerified.value = true
    showError.value = false
  } else {
    showError.value = true
    // Reset fields
    name.value = ''
    age.value = ''
  }
}
</script>

<template>
  <div class="verification-container">
    <div v-if="!isVerified" class="form-container">
      <h1 class="title">Birthday Verification</h1>
      
      <form @submit.prevent="verifyIdentity" class="form">
        <div class="input-group">
          <label for="name">What's your name?</label>
          <input 
            id="name"
            v-model="name"
            type="text"
            required
            placeholder="Enter your name"
            class="input"
          >
        </div>

        <div class="input-group">
          <label for="age">How old are you turning this year?</label>
          <input 
            id="age"
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
      <!-- We'll add navigation to next component here later -->
    </div>
  </div>
</template>

<style scoped>
.verification-container {
  max-width: 500px;
  margin: 50px auto;
  padding: 20px;
}

.title {
  text-align: center;
  color: #333;
  margin-bottom: 30px;
}

.form-container {
  background: white;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.input {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 16px;
}

.submit-btn {
  background: #ff69b4;
  color: white;
  padding: 12px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background 0.3s;
}

.submit-btn:hover {
  background: #ff1493;
}

.error-message {
  margin-top: 20px;
  padding: 10px;
  text-align: center;
  color: #ff4444;
  background: #ffe6e6;
  border-radius: 5px;
}

.success-message {
  text-align: center;
  color: #4caf50;
}
</style>