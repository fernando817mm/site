<template>
    <nav class="navbar">
      <div class="nav-container">
        <div class="nav-logo">
          <router-link to="/" class="logo-link">Portfolio</router-link>
        </div>
        
        <div class="nav-menu" :class="{ 'nav-menu-active': isMenuOpen }">
          <router-link to="/" class="nav-link" @click="closeMenu">Home</router-link>
          <router-link to="/contact" class="nav-link" @click="closeMenu">Contact</router-link>
          <a href="https://linkedin.com/in/fernando817mm" target="_blank" class="nav-link external-link">
            LinkedIn
          </a>
          <a href="https://github.com/fernando817mm" target="_blank" class="nav-link external-link">
            GitHub
          </a>
          <router-link to="/blog" class="nav-link" @click="closeMenu">Blog</router-link>
        </div>
        
        <div class="nav-toggle" @click="toggleMenu">
          <span class="bar"></span>
          <span class="bar"></span>
          <span class="bar"></span>
        </div>
      </div>
    </nav>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  
  const isMenuOpen = ref(false)
  
  const toggleMenu = () => {
    isMenuOpen.value = !isMenuOpen.value
  }
  
  const closeMenu = () => {
    isMenuOpen.value = false
  }
  </script>
  
  <style scoped>
  .navbar {
    background-color: #ffffff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    position: sticky;
    top: 0;
    z-index: 1000;
  }
  
  .nav-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 1rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 60px;
  }
  
  .nav-logo .logo-link {
    font-size: 1.5rem;
    font-weight: 700;
    color: #2d3748;
    text-decoration: none;
  }
  
  .nav-menu {
    display: flex;
    align-items: center;
    gap: 2rem;
  }
  
  .nav-link {
    color: #4a5568;
    text-decoration: none;
    font-weight: 500;
    transition: color 0.3s ease;
  }
  
  .nav-link:hover {
    color: #2b6cb0;
  }
  
  .nav-link.router-link-active {
    color: #2b6cb0;
  }
  
  .external-link::after {
    content: '↗';
    font-size: 0.8rem;
    margin-left: 0.25rem;
  }
  
  .nav-toggle {
    display: none;
    flex-direction: column;
    cursor: pointer;
  }
  
  .bar {
    width: 25px;
    height: 3px;
    background-color: #4a5568;
    margin: 3px 0;
    transition: 0.3s;
  }
  
  @media (max-width: 768px) {
    .nav-menu {
      position: fixed;
      left: -100%;
      top: 60px;
      flex-direction: column;
      background-color: #ffffff;
      width: 100%;
      text-align: center;
      transition: 0.3s;
      box-shadow: 0 10px 27px rgba(0, 0, 0, 0.05);
      padding: 2rem 0;
    }
  
    .nav-menu-active {
      left: 0;
    }
  
    .nav-toggle {
      display: flex;
    }
  }
  </style>