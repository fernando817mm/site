<template>
    <div class="blog">
      <div class="blog-container">
        <h1 class="blog-title">Blog</h1>
        <p class="blog-subtitle">Coming Soon!</p>
        <div class="blog-placeholder">
          <p>This is where I'll share my thoughts, tutorials, and insights about web development.</p>
          <p>Stay tuned for exciting content!</p>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  // Blog component logic will go here
  </script>
  
  <style scoped>
  .blog {
    min-height: 100vh;
    background: linear-gradient(135deg, #f7fafc 0%, #edf2f7 100%);
    padding: 2rem;
  }
  
  .blog-container {
    max-width: 800px;
    margin: 0 auto;
    text-align: center;
  }
  
  .blog-title {
    font-size: 2.5rem;
    font-weight: 700;
    color: #2d3748;
    margin-bottom: 1rem;
  }
  
  .blog-subtitle {
    font-size: 1.25rem;
    color: #4a5568;
    margin-bottom: 2rem;
  }
  
  .blog-placeholder {
    background: #ffffff;
    padding: 3rem;
    border-radius: 12px;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  }
  
  .blog-placeholder p {
    font-size: 1.1rem;
    color: #4a5568;
    margin-bottom: 1rem;
  }
  </style>