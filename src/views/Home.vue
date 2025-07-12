<template>
    <div class="home">
      <ProfilePicture 
        title="Fernando Mauricio Martinez" 
        subtitle="Full Stack Developer with expertise in Front End"
        image-url="/images/portrait.jpg"
      />
      
      <TimelineJumpButtons />
      
      <Timeline />
    </div>
  </template>
  
  <script setup lang="ts">
  import ProfilePicture from '../components/common/ProfilePicture.vue';
  import Timeline from '../components/common/Timeline.vue'
  import TimelineJumpButtons from '../components/ui/TimelineJumpButtons.vue'
  </script>
  
  <style scoped>
  .home {
    min-height: 100vh;
    min-width: 100vw;
    background: linear-gradient(135deg, #f7fafc 0%, #edf2f7 100%);
  }
  </style>