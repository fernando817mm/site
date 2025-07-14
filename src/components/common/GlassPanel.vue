<template>
  <div class="glass-panel-container" :style="{ width: width + 'px', height: height + 'px' }">
    <!-- Three.js Canvas -->
    <canvas ref="canvas" class="glass-canvas" />
    
    <!-- Content Overlay -->
    <div class="glass-content" :style="contentStyle">
      <slot></slot>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import * as THREE from 'three'

// Props
interface Props {
  width?: number
  height?: number
  thickness?: number
  transmission?: number
  roughness?: number
  ior?: number
  distortionScale?: number
  temporalDistortion?: number
  padding?: string
}

const props = withDefaults(defineProps<Props>(), {
  width: 300,
  height: 200,
  thickness: 0.1,
  transmission: 0.9,
  roughness: 0.1,
  ior: 1.5,
  distortionScale: 0.5,
  temporalDistortion: 0.1,
  padding: '20px'
})

// Refs
const canvas = ref<HTMLCanvasElement | null>(null)

// Three.js variables
let scene: THREE.Scene | null = null
let camera: THREE.PerspectiveCamera | null = null
let renderer: THREE.WebGLRenderer | null = null
let glassMesh: THREE.Mesh | null = null
let animationId: number | null = null
let envMap: THREE.Texture | null = null

// Computed
const contentStyle = computed(() => ({
  padding: props.padding,
  color: 'rgba(255, 255, 255, 0.9)',
  textShadow: '0 1px 3px rgba(0, 0, 0, 0.3)',
  backdropFilter: 'blur(1px)'
}))

// Methods
const initThree = () => {
  if (!canvas.value) return
  
  // Scene
  scene = new THREE.Scene()
  
  // Camera
  camera = new THREE.PerspectiveCamera(
    75,
    props.width / props.height,
    0.1,
    1000
  )
  camera.position.z = 1
  
  // Renderer
  renderer = new THREE.WebGLRenderer({
    canvas: canvas.value,
    alpha: true,
    antialias: true
  })
  renderer.setSize(props.width, props.height)
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
  renderer.toneMapping = THREE.ACESFilmicToneMapping
  renderer.toneMappingExposure = 1
}

const createEnvironment = () => {
  if (!renderer || !scene) return
  
  // Create a cube render target for environment mapping
  const envMapSize = 256
  const cubeRenderTarget = new THREE.WebGLCubeRenderTarget(envMapSize)
  const envMapCamera = new THREE.CubeCamera(0.1, 100, cubeRenderTarget)
  
  // Create a simple gradient environment
  const envScene = new THREE.Scene()
  
  // Add some colored lights to create interesting reflections
  const light1 = new THREE.DirectionalLight(0x4fc3f7, 0.5)
  light1.position.set(1, 1, 1)
  envScene.add(light1)
  
  const light2 = new THREE.DirectionalLight(0xe1bee7, 0.3)
  light2.position.set(-1, -1, -1)
  envScene.add(light2)
  
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.2)
  envScene.add(ambientLight)
  
  // Create a gradient background
  const geometry = new THREE.SphereGeometry(50, 32, 16)
  const material = new THREE.MeshBasicMaterial({
    color: 0x87ceeb,
    side: THREE.BackSide
  })
  const sphere = new THREE.Mesh(geometry, material)
  envScene.add(sphere)
  
  // Generate environment map
  envMapCamera.update(renderer, envScene)
  envMap = cubeRenderTarget.texture
}

const createGlassMaterial = () => {
  if (!scene || !envMap) return
  
  // Create geometry
  const geometry = new THREE.PlaneGeometry(
    props.width / 100,
    props.height / 100
  )
  
  // Create realistic glass material using MeshPhysicalMaterial
  const material = new THREE.MeshPhysicalMaterial({
    transmission: props.transmission,
    thickness: props.thickness,
    roughness: props.roughness,
    ior: props.ior,
    envMap: envMap,
    color: 0xffffff,
    transparent: true,
    opacity: 0.1,
    clearcoat: 1,
    clearcoatRoughness: 0.1,
    metalness: 0,
    side: THREE.DoubleSide
  })
  
  // Create mesh
  glassMesh = new THREE.Mesh(geometry, material)
  scene.add(glassMesh)
  
  // Add some subtle animation data
  glassMesh.userData = { time: 0 }
}

const animate = () => {
  animationId = requestAnimationFrame(animate)
  
  // Subtle animation
  if (glassMesh) {
    glassMesh.userData.time += 0.01
    glassMesh.rotation.z = Math.sin(glassMesh.userData.time) * 0.005
    
    // Subtle scale breathing effect
    const scale = 1 + Math.sin(glassMesh.userData.time * 0.5) * 0.002
    glassMesh.scale.set(scale, scale, 1)
  }
  
  if (renderer && scene && camera) {
    renderer.render(scene, camera)
  }
}

const onResize = () => {
  if (camera && renderer) {
    camera.aspect = props.width / props.height
    camera.updateProjectionMatrix()
    renderer.setSize(props.width, props.height)
  }
}

// Lifecycle
onMounted(() => {
  initThree()
  createEnvironment()
  createGlassMaterial()
  animate()
  
  // Handle resize
  window.addEventListener('resize', onResize)
})

onUnmounted(() => {
  if (animationId) {
    cancelAnimationFrame(animationId)
  }
  window.removeEventListener('resize', onResize)
  
  // Clean up Three.js resources
  if (renderer) {
    renderer.dispose()
  }
})
</script>

<style scoped>
.glass-panel-container {
  position: relative;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.glass-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.glass-content {
  position: relative;
  z-index: 2;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  pointer-events: auto;
}

/* iOS-like glass effect fallback */
.glass-panel-container::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(
    135deg,
    rgba(255, 255, 255, 0.1) 0%,
    rgba(255, 255, 255, 0.05) 100%
  );
  z-index: 0;
}

.glass-panel-container::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.4),
    transparent
  );
  z-index: 3;
}
</style>