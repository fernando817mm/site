<template>
    <div class="timeline">
      <h2 class="timeline-title">My Journey</h2>
      <div class="timeline-container">
        <div 
          v-for="(item, index) in timelineItems" 
          :key="index"
          :id="`timeline-${index}`"
          class="timeline-item"
          :class="{ 'timeline-item-right': index % 2 === 1 }"
        >
          <div class="timeline-content">
            <div class="timeline-date">{{ item.date }}</div>
            <h3 class="timeline-title-item">{{ item.title }}</h3>
            <p class="timeline-description">{{ item.description }}</p>
            <div v-if="item.tags" class="timeline-tags">
              <span v-for="tag in item.tags" :key="tag" class="tag">{{ tag }}</span>
            </div>
          </div>
          <div class="timeline-dot"></div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  
  interface TimelineItem {
    date: string
    title: string
    description: string
    tags?: string[]
  }
  
  const timelineItems = ref<TimelineItem[]>([
    {
      date: '2024',
      title: 'Full Stack Developer',
      description: 'Started building modern web applications with Vue.js and TypeScript',
      tags: ['Vue.js', 'TypeScript', 'Node.js']
    },
    {
      date: '2023',
      title: 'Frontend Developer',
      description: 'Focused on creating responsive and interactive user interfaces',
      tags: ['HTML', 'CSS', 'JavaScript']
    },
    {
      date: '2022',
      title: 'Web Development Journey',
      description: 'Began learning web development fundamentals',
      tags: ['Learning', 'Growth']
    }
  ])
  </script>
  
  <style scoped>
  .timeline {
    max-width: 1000px;
    margin: 0 auto;
    padding: 2rem;
  }
  
  .timeline-title {
    text-align: center;
    font-size: 2.5rem;
    margin-bottom: 3rem;
    color: #2d3748;
  }
  
  .timeline-container {
    position: relative;
  }
  
  .timeline-container::before {
    content: '';
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    width: 2px;
    height: 100%;
    background: linear-gradient(to bottom, #e2e8f0, #cbd5e0);
  }
  
  .timeline-item {
    display: flex;
    margin: 2rem 0;
    position: relative;
  }
  
  .timeline-item:nth-child(odd) {
    justify-content: flex-end;
  }
  
  .timeline-item:nth-child(even) {
    justify-content: flex-start;
  }
  
  .timeline-content {
    background: #ffffff;
    border: 2px solid #e2e8f0;
    border-radius: 8px;
    padding: 1.5rem;
    width: 45%;
    position: relative;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
  
  .timeline-date {
    color: #2b6cb0;
    font-weight: 600;
    font-size: 0.9rem;
    margin-bottom: 0.5rem;
  }
  
  .timeline-title-item {
    color: #2d3748;
    font-size: 1.25rem;
    margin-bottom: 0.75rem;
  }
  
  .timeline-description {
    color: #4a5568;
    line-height: 1.6;
    margin-bottom: 1rem;
  }
  
  .timeline-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }
  
  .tag {
    background: #edf2f7;
    color: #2d3748;
    padding: 0.25rem 0.75rem;
    border-radius: 20px;
    font-size: 0.8rem;
    font-weight: 500;
  }
  
  .timeline-dot {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    width: 16px;
    height: 16px;
    background: #2b6cb0;
    border: 4px solid #ffffff;
    border-radius: 50%;
    top: 1.5rem;
    z-index: 10;
  }
  
  @media (max-width: 768px) {
    .timeline-container::before {
      left: 20px;
    }
    
    .timeline-item {
      justify-content: flex-start !important;
    }
    
    .timeline-content {
      width: calc(100% - 60px);
      margin-left: 40px;
    }
    
    .timeline-dot {
      left: 20px;
    }
  }
  </style>