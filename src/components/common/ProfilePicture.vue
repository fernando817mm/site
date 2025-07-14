<template>
    <div class="profile-picture">
      <img 
        :src="imageUrl || '/vite.svg'" 
        :alt="altText"
        class="profile-image"
        @error="handleImageError"
      />
      <div class="profile-info">
        <h1 class="title">{{ title }}</h1>
        <h2 class="subtitle">{{ subtitle }}</h2>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  
  interface Props {
    imageUrl?: string
    title: string
    subtitle: string
    altText?: string
  }
  
  const props = withDefaults(defineProps<Props>(), {
    imageUrl: '',
    altText: 'Profile Picture'
  })
  
  const handleImageError = () => {
    console.log('Image failed to load', props)
  }
  </script>
  
  <style scoped>
  .profile-picture {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    padding: 2rem;
  }
  
  .profile-image {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    object-fit: cover;
    border: 3px solid #e2e8f0;
    margin-bottom: 1rem;
  }
  
  .profile-info {
    /* max-width: 80%; */
  }
  
  .title {
    font-size: 7rem;
    color: #D4AF37;
    margin-bottom: 0.5rem;
  }
  
  .subtitle {
    font-size: 1.6rem;
    font-weight: 400;
    /* color: #4a5568; */
    margin-bottom: 0;
  }

  .title, .subtitle {
    font-family: "Manufacturing Consent", system-ui;
    font-weight: 400;
    font-style: normal;
    /* color: white; */
  }
  
  @media (max-width: 768px) {
    .profile-image {
      width: 120px;
      height: 120px;
    }
    
    .title {
      font-size: 1.75rem;
    }
    
    .subtitle {
      font-size: 1rem;
    }
  }
  </style>