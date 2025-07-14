<script setup lang="ts">
import { ref } from 'vue'
import GlassPanel from './GlassPanel.vue'

interface TimelineItem {
  date: string
  title: string
  description: string
  tags?: string[]
}

const timelineItems = ref<TimelineItem[]>([
  {
    date: '2025',
    title: 'Freelance Developer',
    description:
      'Freelancing as a frontend developer while pursuing a Computer Science B.S. Managing freelance projects alongside academic commitments to maintain steady progress toward degree completion and advance career goals.',
    tags: ['Vue.js', 'TypeScript', 'Node.js'],
  },
  {
    date: '2024',
    title: 'Frontend Developer @ Ensue LLC',
    description:
      'Worked as a Frontend Developer at Ensue LLC, building and maintaining web apps using React, Angular, and Svelte. Collaborated on a small team delivering features for clients like GE HealthCare Command Center.',
    tags: ['Vue.js', 'TypeScript', 'Node.js'],
  },
  {
    date: '2021',
    title: 'Lambda/BloomTech Coding Bootcamp',
    description:
      'Completed an intensive full-stack coding bootcamp at Lambda School (now BloomTech), focusing on practical skills in JavaScript, React, Node.js, and backend technologies. The program accelerated my transition into professional software development.',
    tags: ['Coding Bootcamp', 'Learning', 'Full Stack', 'Web'],
  },
  {
    date: '2020',
    title: 'Tarrant County College',
    description:
      'Studied Computer Science at TCC, taking 15 to 18 credit hours a semester with plans to transfer to UT Arlington. When COVID hit, school shut down and my plans changed.',
    tags: ['TCC', 'Computer Science', 'Education', 'Pivot'],
  },
  {
    date: '2019',
    title: 'Sam Houston High School',
    description:
      'Graduated in the top 5% of nearly 500 students with a STEM Endorsement from Sam Houston High School in Arlington, TX.',
    tags: ['Learning', 'Growth', 'STEM', 'Graduate'],
  },
])
</script>

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
        <GlassPanel
          :width="400" 
          :height="200"
          :transmission="0.9"
          :roughness="0.1"
          :ior="1.5">
          <div>
            <div class="timeline-date">{{ item.date }}</div>
            <h3 class="timeline-title-item">{{ item.title }}</h3>
            <p class="timeline-description">{{ item.description }}</p>
            <div v-if="item.tags" class="timeline-tags">
              <span v-for="tag in item.tags" :key="tag" class="tag">{{ tag }}</span>
            </div>
          </div>
        </GlassPanel>
        <div class="timeline-dot"></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.timeline {
  max-width: 1000px;
  margin: 0 auto;
  padding: 2rem;
  position: relative;
  z-index: 1;
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
  z-index: 0;
}

.timeline-item {
  display: flex;
  margin: 2rem 0;
  position: relative;
  z-index: 2;
}

.timeline-item:nth-child(odd) {
  justify-content: flex-end;
}

.timeline-item:nth-child(even) {
  justify-content: flex-start;
}

.timeline-content {
  /* Remove default background, shadow, border to rely on glass panel */
  background: transparent !important;
  box-shadow: none !important;
  border: none !important;
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
  z-index: 20;
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
