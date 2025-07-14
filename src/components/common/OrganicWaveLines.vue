<template>
  <svg
    class="background-svg"
    :width="width"
    :height="height"
    xmlns="http://www.w3.org/2000/svg"
  >
    <path
      v-for="(line, i) in lines"
      :key="i"
      :d="line.d"
      :stroke="line.color"
      :stroke-width="line.thickness"
      fill="none"
      stroke-linecap="round"
      stroke-linejoin="round"
      opacity="0.9"
    />
  </svg>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'

// === Constants & Settings ===
const width = window.innerWidth
const height = window.innerHeight
const numLines = 7
const segments = 200
const baseSpacing = 35

// === Tunable variables ===
const variation = 0.3               // 0 = aligned, 1 = chaotic (affects wavelength, amplitude, speed)
const thicknessVariation = 4        // 0 = uniform thickness, 4 = up to 5x normal

const baseWavelength = 200          // wave length
const baseAmplitude = 40            // crest height
const baseSpeed = 0.002             // wave flow speed
const baseThickness = 6             // minimum stroke thickness

const colors = [
  '#fb8500',
  '#ffb703',
  '#f72585',
  '#023047',
  '#219ebc',
  '#8ecae6',
  '#b5179e',
]

// === Utils ===
function getVaried(base: number, variance: number) {
  const offset = (Math.random() * 2 - 1) * variance
  return base * (1 + offset)
}

function getRandom(min: number, max: number) {
  return Math.random() * (max - min) + min
}

interface LineData {
  baseX: number
  thickness: number
  color: string
  d: string
  wavelength: number
  amplitude: number
  speed: number
}

const lines = ref<LineData[]>([])

function generateLines() {
  lines.value = Array.from({ length: numLines }, (_, i) => {
    return {
      baseX: i * baseSpacing + width / 2 - (numLines * baseSpacing) / 2,
      thickness: getRandom(
        baseThickness,
        baseThickness + baseThickness * thicknessVariation
      ),
      color: colors[i],
      d: '',
      wavelength: getVaried(baseWavelength, variation * 0.15),
      amplitude: getVaried(baseAmplitude, variation * 0.2),
      speed: getVaried(baseSpeed, variation * 0.15),
    }
  })
}

let animationFrameId: number

function cubicBezierPath(points: number[][]): string {
  if (points.length < 2) return ''

  let d = `M${points[0][0]},${points[0][1]} `
  for (let i = 0; i < points.length - 1; i++) {
    const p0 = points[i]
    const p1 = points[i + 1]

    const cp1x = p0[0] + (p1[0] - (points[i - 1]?.[0] || p0[0])) / 3 || p0[0]
    const cp1y = p0[1] + (p1[1] - (points[i - 1]?.[1] || p0[1])) / 3 || p0[1]
    const cp2x = p1[0] - ((points[i + 2]?.[0] || p1[0]) - p0[0]) / 3 || p1[0]
    const cp2y = p1[1] - ((points[i + 2]?.[1] || p1[1]) - p0[1]) / 3 || p1[1]

    d += `C${cp1x},${cp1y} ${cp2x},${cp2y} ${p1[0]},${p1[1]} `
  }

  return d
}

const updateLines = (t: number) => {
  lines.value.forEach((line) => {
    const points: number[][] = []

    for (let i = 0; i <= segments; i++) {
      const y = (height / segments) * i
      const offsetX =
        line.baseX +
        Math.sin(y / line.wavelength + t * line.speed) * line.amplitude
      points.push([offsetX, y])
    }

    line.d = cubicBezierPath(points)
  })

  animationFrameId = requestAnimationFrame(updateLines)
}

onMounted(() => {
  generateLines()
  animationFrameId = requestAnimationFrame(updateLines)
})

onUnmounted(() => {
  cancelAnimationFrame(animationFrameId)
})
</script>

<style scoped>
.background-svg {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 0;
  pointer-events: none;
  width: 100%;
  height: 100%;
  overflow: visible;
}
</style>
