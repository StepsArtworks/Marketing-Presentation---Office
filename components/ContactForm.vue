<script setup lang="ts">
import { ref } from 'vue'
import { useMotion } from '@vueuse/motion'
import gsap from 'gsap'

const formData = ref({
  name: '',
  email: '',
  company: '',
  message: ''
})

const formSubmitted = ref(false)
const formError = ref(false)
const formRef = ref(null)

const { motion } = useMotion(formRef, {
  initial: {
    opacity: 0,
    y: 20,
  },
  enter: {
    opacity: 1,
    y: 0,
    transition: {
      duration: 800,
      ease: 'cubic-bezier(0.4, 0, 0.2, 1)',
    },
  },
})

const submitForm = () => {
  // Validate form
  if (!formData.value.name || !formData.value.email || !formData.value.message) {
    formError.value = true
    gsap.to(formRef.value, {
      x: [-10, 10, -10, 10, 0],
      duration: 0.5,
      ease: "power2.out"
    })
    return
  }
  
  // In a real app, we would send the form data to a server
  console.log('Form submitted:', formData.value)
  formSubmitted.value = true
  formError.value = false
}

const resetForm = () => {
  formSubmitted.value = false
  formData.value = { name: '', email: '', company: '', message: '' }
  gsap.from(formRef.value, {
    scale: 0.8,
    opacity: 0,
    duration: 0.5,
    ease: "back.out(1.7)"
  })
}
</script>

<template>
  <div class="contact-form-container" ref="formRef">
    <div v-if="formSubmitted" class="success-message">
      <div class="success-icon">✓</div>
      <h3>Thank you for your message!</h3>
      <p>We'll be in touch with you shortly.</p>
      <button @click="resetForm" class="tbr-button">
        Send another message
      </button>
    </div>
    
    <form v-else @submit.prevent="submitForm" class="contact-form">
      <h3 class="form-title">Get in touch</h3>
      <p class="form-subtitle">Let us know how we can help with your business needs.</p>
      
      <div class="form-group">
        <label for="name">Full Name</label>
        <input 
          type="text" 
          id="name" 
          v-model="formData.name" 
          placeholder="Your name"
          :class="{ 'error': formError && !formData.name }"
        />
      </div>
      
      <div class="form-group">
        <label for="email">Email Address</label>
        <input 
          type="email" 
          id="email" 
          v-model="formData.email" 
          placeholder="you@example.com"
          :class="{ 'error': formError && !formData.email }"
        />
      </div>
      
      <div class="form-group">
        <label for="company">Company (Optional)</label>
        <input 
          type="text" 
          id="company" 
          v-model="formData.company" 
          placeholder="Your company"
        />
      </div>
      
      <div class="form-group">
        <label for="message">Your Message</label>
        <textarea 
          id="message" 
          v-model="formData.message" 
          placeholder="How can we help you?"
          rows="4"
          :class="{ 'error': formError && !formData.message }"
        ></textarea>
      </div>
      
      <div v-if="formError" class="error-message">
        Please fill in all required fields.
      </div>
      
      <button type="submit" class="tbr-button submit-button">
        Send Message
      </button>
    </form>
  </div>
</template>

<style scoped>
.contact-form-container {
  width: 100%;
  max-width: 500px;
  margin: 0 auto;
}

.form-title {
  font-size: 24px;
  font-weight: 600;
  margin-bottom: 8px;
  background: linear-gradient(135deg, var(--primary), var(--secondary));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.form-subtitle {
  color: var(--neutral-600);
  margin-bottom: 24px;
}

.form-group {
  margin-bottom: 16px;
  transition: transform 0.3s ease;
}

.form-group:focus-within {
  transform: translateX(5px);
}

label {
  display: block;
  font-size: 14px;
  font-weight: 500;
  margin-bottom: 6px;
  color: var(--neutral-700);
  transition: color 0.3s ease;
}

input, textarea {
  width: 100%;
  padding: 12px;
  border: 2px solid var(--neutral-300);
  border-radius: 8px;
  font-size: 16px;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  background-color: transparent;
}

input:focus, textarea:focus {
  outline: none;
  border-color: var(--primary);
  box-shadow: 0 0 0 3px rgba(0, 122, 255, 0.2);
  transform: translateY(-2px);
}

input.error, textarea.error {
  border-color: var(--error);
  animation: shake 0.5s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
}

.error-message {
  color: var(--error);
  font-size: 14px;
  margin-bottom: 16px;
  animation: slideIn 0.3s ease;
}

.submit-button {
  width: 100%;
  padding: 12px;
  font-size: 16px;
  font-weight: 500;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.submit-button::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 300%;
  height: 300%;
  background: radial-gradient(circle, rgba(255,255,255,0.2) 0%, transparent 60%);
  transform: translate(-50%, -50%) scale(0);
  opacity: 0;
  transition: transform 0.6s ease, opacity 0.6s ease;
}

.submit-button:hover::after {
  transform: translate(-50%, -50%) scale(1);
  opacity: 1;
}

.success-message {
  text-align: center;
  padding: 32px;
  background-color: white;
  border-radius: 12px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  animation: scaleIn 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.success-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 64px;
  height: 64px;
  background: linear-gradient(135deg, var(--success), var(--primary));
  color: white;
  font-size: 32px;
  border-radius: 50%;
  margin: 0 auto 16px;
  animation: bounceIn 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

@keyframes shake {
  10%, 90% { transform: translateX(-1px); }
  20%, 80% { transform: translateX(2px); }
  30%, 50%, 70% { transform: translateX(-4px); }
  40%, 60% { transform: translateX(4px); }
}

@keyframes slideIn {
  from { transform: translateY(-10px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

@keyframes scaleIn {
  from { transform: scale(0.8); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}

@keyframes bounceIn {
  0% { transform: scale(0); }
  50% { transform: scale(1.2); }
  100% { transform: scale(1); }
}

/* Dark mode styles */
:global(.dark-mode) label {
  color: var(--neutral-300);
}

:global(.dark-mode) input, :global(.dark-mode) textarea {
  background-color: var(--neutral-800);
  border-color: var(--neutral-700);
  color: var(--neutral-200);
}

:global(.dark-mode) input:focus, :global(.dark-mode) textarea:focus {
  border-color: var(--primary);
  box-shadow: 0 0 0 3px rgba(10, 132, 255, 0.3);
}

:global(.dark-mode) .success-message {
  background-color: var(--neutral-800);
}
</style>