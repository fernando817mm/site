<template>
    <div class="contact">
      <div class="contact-container">
        <h1 class="contact-title">Get In Touch</h1>
        <p class="contact-subtitle">I'd love to hear from you. Send me a message!</p>
        
        <form @submit.prevent="submitForm" class="contact-form">
          <div class="form-group">
            <label for="name">Name</label>
            <input 
              type="text" 
              id="name" 
              v-model="form.name" 
              required 
              class="form-input"
            />
          </div>
          
          <div class="form-group">
            <label for="email">Email</label>
            <input 
              type="email" 
              id="email" 
              v-model="form.email" 
              required 
              class="form-input"
            />
          </div>
          
          <div class="form-group">
            <label for="message">Message</label>
            <textarea 
              id="message" 
              v-model="form.message" 
              required 
              rows="5"
              class="form-textarea"
            ></textarea>
          </div>
          
          <button type="submit" class="submit-button">
            Send Message
          </button>
        </form>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  
  const form = ref({
    name: '',
    email: '',
    message: ''
  })
  
  const submitForm = () => {
    console.log('Form submitted:', form.value)
    alert('Thank you for your message! I\'ll get back to you soon.')
    
    form.value = {
      name: '',
      email: '',
      message: ''
    }
  }
  </script>
  
  <style scoped>
  .contact {
    min-height: 100vh;
    background: linear-gradient(135deg, #f7fafc 0%, #edf2f7 100%);
    padding: 2rem;
  }
  
  .contact-container {
    max-width: 600px;
    margin: 0 auto;
    background: #ffffff;
    padding: 3rem;
    border-radius: 12px;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  }
  
  .contact-title {
    font-size: 2.5rem;
    font-weight: 700;
    color: #2d3748;
    text-align: center;
    margin-bottom: 1rem;
  }
  
  .contact-subtitle {
    font-size: 1.1rem;
    color: #4a5568;
    text-align: center;
    margin-bottom: 2rem;
  }
  
  .contact-form {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }
  
  .form-group {
    display: flex;
    flex-direction: column;
  }
  
  .form-group label {
    font-weight: 600;
    color: #2d3748;
    margin-bottom: 0.5rem;
  }
  
  .form-input,
  .form-textarea {
    padding: 0.75rem;
    border: 1px solid #e2e8f0;
    border-radius: 6px;
    font-size: 1rem;
    transition: border-color 0.3s ease;
  }
  
  .form-input:focus,
  .form-textarea:focus {
    outline: none;
    border-color: #2b6cb0;
  }
  
  .submit-button {
    background: #2b6cb0;
    color: #ffffff;
    padding: 0.75rem 1.5rem;
    border: none;
    border-radius: 6px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .submit-button:hover {
    background: #2c5282;
  }
  </style>