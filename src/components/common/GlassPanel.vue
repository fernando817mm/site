<template>
  <div class="glass-panel-container" ref="containerRef" :style="containerStyle">
    <!-- Three.js canvas positioned absolutely behind content -->
    <canvas 
      ref="canvasRef" 
      class="glass-canvas"
      :style="{ width: canvasWidth + 'px', height: canvasHeight + 'px' }"
    ></canvas>
    
    <!-- Content slot with padding -->
    <div class="glass-content" ref="contentRef" :style="contentStyle">
      <slot></slot>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, nextTick, watch, computed } from 'vue'
import * as THREE from 'three'
import { MeshTransmissionMaterial } from '@pmndrs/vanilla'

interface Props {
  width?: number | string
  height?: number | string
}

const props = withDefaults(defineProps<Props>(), {
  width: 'auto',
  height: 'auto'
})

const containerRef = ref<HTMLElement>()
const canvasRef = ref<HTMLCanvasElement>()
const contentRef = ref<HTMLElement>()

const canvasWidth = ref(0)
const canvasHeight = ref(0)

const containerStyle = computed(() => ({
  width: typeof props.width === 'number' ? `${props.width}px` : props.width,
  height: typeof props.height === 'number' ? `${props.height}px` : props.height
}))

const contentStyle = computed(() => ({
  width: typeof props.width === 'number' ? `${props.width}px` : props.width,
  height: typeof props.height === 'number' ? `${props.height}px` : props.height
}))

let scene: THREE.Scene
let camera: THREE.OrthographicCamera
let renderer: THREE.WebGLRenderer
let glassMesh: THREE.Mesh
let resizeObserver: ResizeObserver

const updateCanvasSize = async () => {
  if (!contentRef.value) return
  
  await nextTick()
  
  const contentRect = contentRef.value.getBoundingClientRect()
  canvasWidth.value = contentRect.width
  canvasHeight.value = contentRect.height
  
  if (renderer && camera) {
    renderer.setSize(canvasWidth.value, canvasHeight.value)
    
    // Update camera to match new size
    const aspect = canvasWidth.value / canvasHeight.value
    camera.left = -canvasWidth.value / 2
    camera.right = canvasWidth.value / 2
    camera.top = canvasHeight.value / 2
    camera.bottom = -canvasHeight.value / 2
    camera.updateProjectionMatrix()
    
    // Update glass mesh size
    if (glassMesh) {
      glassMesh.scale.set(canvasWidth.value, canvasHeight.value, 1)
    }
  }
}

const initThreeJS = () => {
  if (!canvasRef.value) return
  
  // Scene
  scene = new THREE.Scene()
  
  // Camera (orthographic for 2D effect)
  camera = new THREE.OrthographicCamera(
    -canvasWidth.value / 2, canvasWidth.value / 2,
    canvasHeight.value / 2, -canvasHeight.value / 2,
    0.1, 1000
  )
  camera.position.z = 5
  
  // Renderer
  renderer = new THREE.WebGLRenderer({
    canvas: canvasRef.value,
    alpha: true,
    antialias: true
  })
  renderer.setSize(canvasWidth.value, canvasHeight.value)
  renderer.setClearColor(0x000000, 0)
  renderer.toneMapping = THREE.ACESFilmicToneMapping
  renderer.toneMappingExposure = 1.0
  
  // Create glass geometry (plane)
  const geometry = new THREE.PlaneGeometry(1, 1)
  
  // Create a simple background scene for transmission
  const backgroundGeometry = new THREE.PlaneGeometry(2, 2)
  const backgroundMaterial = new THREE.MeshBasicMaterial({ 
    color: 0xffffff,
    transparent: true,
    opacity: 0.1
  })
  const backgroundMesh = new THREE.Mesh(backgroundGeometry, backgroundMaterial)
  backgroundMesh.position.z = -1
  scene.add(backgroundMesh)
  
  // Create glass material - fallback to MeshPhysicalMaterial if transmission doesn't work
  let glassMaterial
  try {
    glassMaterial = new MeshTransmissionMaterial({
      transmission: 1.0,
      thickness: 0.2,
      roughness: 0.0
    })
  } catch (error) {
    // Fallback to standard glass material
    glassMaterial = new THREE.MeshPhysicalMaterial({
      transmission: 1.0,
      thickness: 0.2,
      roughness: 0.0,
      metalness: 0.0,
      clearcoat: 1.0,
      clearcoatRoughness: 0.0,
      color: new THREE.Color(0xffffff),
      ior: 1.5,
      reflectivity: 0.1,
      transparent: true,
      opacity: 0.8,
      side: THREE.DoubleSide
    })
  }
  
  // Create glass mesh
  glassMesh = new THREE.Mesh(geometry, glassMaterial)
  glassMesh.scale.set(canvasWidth.value, canvasHeight.value, 1)
  scene.add(glassMesh)
  
  // Add lights for better transmission effect
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.8)
  scene.add(ambientLight)
  
  const directionalLight = new THREE.DirectionalLight(0xffffff, 1.0)
  directionalLight.position.set(1, 1, 1)
  scene.add(directionalLight)
  
  const pointLight = new THREE.PointLight(0xffffff, 0.8)
  pointLight.position.set(-1, -1, 2)
  scene.add(pointLight)
  
  // Create a simple environment for transmission
  const cubeRenderTarget = new THREE.WebGLCubeRenderTarget(128)
  const cubeCamera = new THREE.CubeCamera(0.1, 10, cubeRenderTarget)
  scene.add(cubeCamera)
  
  // Set environment map for transmission
  scene.environment = cubeRenderTarget.texture
  glassMaterial.envMap = cubeRenderTarget.texture
  
  // Animation loop
  const animate = () => {
    requestAnimationFrame(animate)
    
    // Subtle animation for glass distortion
    if (glassMesh) {
      glassMesh.rotation.z = Math.sin(Date.now() * 0.001) * 0.002
    }
    
    renderer.render(scene, camera)
  }
  
  animate()
}

onMounted(async () => {
  await nextTick()
  
  // Set initial size
  await updateCanvasSize()
  
  // Initialize Three.js
  initThreeJS()
  
  // Set up resize observer
  if (contentRef.value) {
    resizeObserver = new ResizeObserver(() => {
      updateCanvasSize()
    })
    resizeObserver.observe(contentRef.value)
  }
})

onUnmounted(() => {
  if (resizeObserver) {
    resizeObserver.disconnect()
  }
  
  if (renderer) {
    renderer.dispose()
  }
})

// Watch for canvas size changes
watch([canvasWidth, canvasHeight], () => {
  if (renderer && camera && glassMesh) {
    renderer.setSize(canvasWidth.value, canvasHeight.value)
    
    camera.left = -canvasWidth.value / 2
    camera.right = canvasWidth.value / 2
    camera.top = canvasHeight.value / 2
    camera.bottom = -canvasHeight.value / 2
    camera.updateProjectionMatrix()
    
    glassMesh.scale.set(canvasWidth.value, canvasHeight.value, 1)
  }
})
</script>

<style scoped>
.glass-panel-container {
  position: relative;
  display: inline-block;
  border-radius: 12px;
  overflow: hidden;
}

.glass-canvas {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 0;
  pointer-events: none;
}

.glass-content {
  position: relative;
  z-index: 1;
  padding: 1.5rem;
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.05);
  box-shadow: 
    0 8px 32px rgba(0, 0, 0, 0.1),
    inset 0 1px 0 rgba(255, 255, 255, 0.2);
  box-sizing: border-box;
}

/* Fallback for browsers without Three.js support */
@supports not (display: flex) {
  .glass-content {
    background: rgba(255, 255, 255, 0.15);
  }
}
</style>