<template>
  <div id="app">
    <OrganicWaveLines />
    <div class="content">
      <Navbar />
      <router-view />
    </div>
  </div>
</template>

<script setup lang="ts">
import Navbar from './components/common/Navbar.vue'
import OrganicWaveLines from './components/common/OrganicWaveLines.vue'
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  overflow-x: hidden;
}

body {
  font-family: "Fira Mono", monospace;
  font-weight: 500;
  font-style: normal;
  line-height: 1.6;
  color: #333;
  background-color: white;
}

#app {
  position: relative;
  min-height: 100vh;
  width: 100%;
  overflow-x: hidden;
  overflow-y: auto;
}

.content {
  position: relative;
  z-index: 1;
  min-height: 100vh;
  width: 100%;
  padding: 1rem;
  background: transparent;
}
</style>
