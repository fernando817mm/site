<template>
    <nav class="navbar">
      <div class="nav-container">
        <div class="nav-menu">
          <router-link to="/" class="nav-link">HQ</router-link>
          <p>/</p>
          <a href="https://github.com/fernando817mm" target="_blank" class="nav-link">Code</a>
          <p>/</p>
          <router-link to="/blog" class="nav-link">Brain</router-link>
          <p>/</p>
          <a href="https://linkedin.com/in/fernando817mm" target="_blank" class="nav-link">In</a>
          <p>/</p>
          <router-link to="/contact" class="nav-link">Connect</router-link>
        </div>
      </div>
    </nav>
  </template>
  
  <script setup lang="ts">
  </script>
  
  <style scoped>
    .navbar {
      background-color: #ffffff;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      background: none;
      position: sticky;
      top: 0;
      z-index: 1000;
    }
    
    .nav-container {
      background-color: #ffffff;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      margin: 0 auto;
      padding: 0 1.5rem;
      display: flex;
      justify-content: right;
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
      align-items: right;
      gap: .5rem;
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
        font-size: 0.75rem;
      }

      .nav-container {
        justify-content: center;
      }
    }
  </style>