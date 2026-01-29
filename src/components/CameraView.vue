<script setup lang="ts">
import { ref, onUnmounted } from 'vue'

const emit = defineEmits<{
  status: [text: string]
}>()

const videoRef = ref<HTMLVideoElement | null>(null)
const canvasRef = ref<HTMLCanvasElement | null>(null)
const isStreaming = ref(false)
const errorMessage = ref('')
const streamRef = ref<MediaStream | null>(null)

async function startCamera() {
  try {
    errorMessage.value = ''
    emit('status', '正在启动摄像头...')

    const stream = await navigator.mediaDevices.getUserMedia({
      video: {
        width: { ideal: 1920 },
        height: { ideal: 1080 },
        facingMode: 'user'
      },
      audio: false
    })

    streamRef.value = stream

    if (videoRef.value) {
      videoRef.value.srcObject = stream
      await videoRef.value.play()
      isStreaming.value = true
      emit('status', '摄像头运行中')
    }
  } catch (error) {
    console.error('摄像头访问错误:', error)
    errorMessage.value = '无法访问摄像头，请检查权限设置'
    emit('status', '摄像头启动失败')
    isStreaming.value = false
  }
}

function stopCamera() {
  if (streamRef.value) {
    streamRef.value.getTracks().forEach(track => track.stop())
    streamRef.value = null
  }

  if (videoRef.value) {
    videoRef.value.srcObject = null
  }

  isStreaming.value = false
  emit('status', '摄像头已停止')
}

function takeScreenshot() {
  if (!videoRef.value || !canvasRef.value || !isStreaming.value) {
    return
  }

  const video = videoRef.value
  const canvas = canvasRef.value
  const ctx = canvas.getContext('2d')

  if (!ctx) return

  canvas.width = video.videoWidth
  canvas.height = video.videoHeight

  ctx.drawImage(video, 0, 0, canvas.width, canvas.height)

  const link = document.createElement('a')
  const timestamp = new Date().toISOString().replace(/[:.]/g, '-')
  link.download = `screenshot-${timestamp}.png`
  link.href = canvas.toDataURL('image/png')
  link.click()

  emit('status', `截图已保存: screenshot-${timestamp}.png`)
  setTimeout(() => {
    if (isStreaming.value) {
      emit('status', '摄像头运行中')
    }
  }, 3000)
}

onUnmounted(() => {
  stopCamera()
})
</script>

<template>
  <div class="camera-container">
    <video
      ref="videoRef"
      class="video-feed"
      autoplay
      playsinline
      muted
    ></video>
    <canvas ref="canvasRef" class="hidden-canvas"></canvas>

    <div v-if="!isStreaming && !errorMessage" class="placeholder">
      <svg class="camera-icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M15 10l4.553-2.276A1 1 0 0121 8.618v6.764a1 1 0 01-1.447.894L15 14M5 18h8a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v8a2 2 0 002 2z" />
      </svg>
      <p>点击下方按钮启动摄像头</p>
    </div>

    <div v-if="errorMessage" class="error-message">
      <svg class="error-icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
      </svg>
      <p>{{ errorMessage }}</p>
    </div>
  </div>

  <div class="controls">
    <button
      v-if="!isStreaming"
      @click="startCamera"
      class="control-btn start-btn"
      title="启动摄像头"
    >
      <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z" />
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
      </svg>
      <span>启动</span>
    </button>

    <button
      v-if="isStreaming"
      @click="stopCamera"
      class="control-btn stop-btn"
      title="停止摄像头"
    >
      <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 10a1 1 0 011-1h4a1 1 0 011 1v4a1 1 0 01-1 1h-4a1 1 0 01-1-1v-4z" />
      </svg>
      <span>停止</span>
    </button>

    <button
      v-if="isStreaming"
      @click="takeScreenshot"
      class="control-btn screenshot-btn"
      title="截图"
    >
      <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z" />
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 13a3 3 0 11-6 0 3 3 0 016 0z" />
      </svg>
      <span>截图</span>
    </button>
  </div>
</template>

<style scoped>
.camera-container {
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 9;
  max-height: 60vh;
  background: linear-gradient(135deg, rgba(99, 102, 241, 0.1), rgba(168, 85, 247, 0.1));
  border-radius: 24px;
  overflow: hidden;
  box-shadow:
    0 8px 32px rgba(0, 0, 0, 0.3),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.video-feed {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.hidden-canvas {
  display: none;
}

.placeholder {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: rgba(255, 255, 255, 0.6);
  gap: 1rem;
}

.camera-icon {
  width: 80px;
  height: 80px;
  opacity: 0.5;
}

.error-message {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: #ef4444;
  gap: 1rem;
}

.error-icon {
  width: 60px;
  height: 60px;
}

.controls {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-top: 2rem;
}

.control-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 1rem 2rem;
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  backdrop-filter: blur(10px);
  min-width: 120px;
  justify-content: center;
}

.control-btn svg {
  width: 20px;
  height: 20px;
}

.start-btn {
  background: linear-gradient(135deg, rgba(34, 197, 94, 0.2), rgba(16, 185, 129, 0.2));
  color: #22c55e;
  box-shadow:
    0 4px 15px rgba(34, 197, 94, 0.3),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
}

.start-btn:hover {
  background: linear-gradient(135deg, rgba(34, 197, 94, 0.3), rgba(16, 185, 129, 0.3));
  box-shadow:
    0 6px 20px rgba(34, 197, 94, 0.4),
    inset 0 1px 0 rgba(255, 255, 255, 0.15);
  transform: translateY(-2px);
}

.stop-btn {
  background: linear-gradient(135deg, rgba(239, 68, 68, 0.2), rgba(220, 38, 38, 0.2));
  color: #ef4444;
  box-shadow:
    0 4px 15px rgba(239, 68, 68, 0.3),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
}

.stop-btn:hover {
  background: linear-gradient(135deg, rgba(239, 68, 68, 0.3), rgba(220, 38, 38, 0.3));
  box-shadow:
    0 6px 20px rgba(239, 68, 68, 0.4),
    inset 0 1px 0 rgba(255, 255, 255, 0.15);
  transform: translateY(-2px);
}

.screenshot-btn {
  background: linear-gradient(135deg, rgba(99, 102, 241, 0.2), rgba(168, 85, 247, 0.2));
  color: #818cf8;
  box-shadow:
    0 4px 15px rgba(99, 102, 241, 0.3),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
}

.screenshot-btn:hover {
  background: linear-gradient(135deg, rgba(99, 102, 241, 0.3), rgba(168, 85, 247, 0.3));
  box-shadow:
    0 6px 20px rgba(99, 102, 241, 0.4),
    inset 0 1px 0 rgba(255, 255, 255, 0.15);
  transform: translateY(-2px);
}

.control-btn:active {
  transform: translateY(0);
}
</style>
