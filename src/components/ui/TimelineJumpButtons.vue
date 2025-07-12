<template>
    <div class="timeline-jump-buttons">
      <h3 class="jump-buttons-title">Quick Navigation</h3>
      <div class="buttons-container">
        <button
          v-for="(item, index) in jumpButtons"
          :key="index"
          @click="scrollToSection(index)"
          class="jump-button"
          :class="{ 'jump-button-active': activeSection === index }"
        >
          {{ item.label }}
        </button>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted, onUnmounted } from 'vue'
  
  interface JumpButton {
    label: string
    year: string
  }
  
  const jumpButtons = ref<JumpButton[]>([
    { label: '2024', year: '2024' },
    { label: '2023', year: '2023' },
    { label: '2022', year: '2022' }
  ])
  
  const activeSection = ref(0)
  
  const scrollToSection = (index: number) => {
    const element = document.getElementById(`timeline-${index}`)
    if (element) {
      element.scrollIntoView({ 
        behavior: 'smooth', 
        block: 'center' 
      })
      activeSection.value = index
    }
  }
  
  const handleScroll = () => {
    const scrollPosition = window.scrollY
    const windowHeight = window.innerHeight
    
    jumpButtons.value.forEach((_: any, index: any) => {
      const element = document.getElementById(`timeline-${index}`)
      if (element) {
        const elementTop = element.offsetTop
        const elementHeight = element.offsetHeight
        
        if (scrollPosition >= elementTop - windowHeight / 2 && 
            scrollPosition < elementTop + elementHeight - windowHeight / 2) {
          activeSection.value = index
        }
      }
    })
  }
  
  onMounted(() => {
    window.addEventListener('scroll', handleScroll)
  })
  
  onUnmounted(() => {
    window.removeEventListener('scroll', handleScroll)
  })
  </script>
  
  <style scoped>
  .timeline-jump-buttons {
    position: sticky;
    top: 80px;
    background: #ffffff;
    border: 1px solid #e2e8f0;
    border-radius: 8px;
    padding: 1.5rem;
    margin: 2rem 0;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
  
  .jump-buttons-title {
    font-size: 1.1rem;
    font-weight: 600;
    color: #2d3748;
    margin-bottom: 1rem;
    text-align: center;
  }
  
  .buttons-container {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    justify-content: center;
  }
  
  .jump-button {
    background: #f7fafc;
    color: #4a5568;
    border: 1px solid #e2e8f0;
    border-radius: 6px;
    padding: 0.5rem 1rem;
    font-size: 0.9rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.3s ease;
  }
  
  .jump-button:hover {
    background: #edf2f7;
    border-color: #cbd5e0;
  }
  
  .jump-button-active {
    background: #2b6cb0;
    color: #ffffff;
    border-color: #2b6cb0;
  }
  
  @media (max-width: 768px) {
    .timeline-jump-buttons {
      position: relative;
      top: 0;
    }
    
    .buttons-container {
      flex-direction: column;
    }
    
    .jump-button {
      width: 100%;
      text-align: center;
    }
  }
  </style>