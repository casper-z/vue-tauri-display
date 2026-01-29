<script setup lang="ts">
import { ref } from 'vue'
import CameraView from './components/CameraView.vue'

const statusText = ref('准备就绪')

function updateStatus(text: string) {
  statusText.value = text
}
</script>

<template>
  <div class="app-container">
    <div class="background-gradient"></div>

    <header class="header">
      <div class="header-content">
        <div class="logo">
          <svg fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M15 10l4.553-2.276A1 1 0 0121 8.618v6.764a1 1 0 01-1.447.894L15 14M5 18h8a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v8a2 2 0 002 2z" />
          </svg>
          <h1>Camera App</h1>
        </div>
        <div class="status-indicator" :class="{ active: statusText.includes('运行中') }">
          <span class="status-dot"></span>
          <span class="status-text">{{ statusText }}</span>
        </div>
      </div>
    </header>

    <main class="main-content">
      <div class="camera-wrapper">
        <CameraView @status="updateStatus" />
      </div>
    </main>

    <footer class="footer">
      <p>Built with Vue 3.3.8 + Tauri 2.0.0-rc.1</p>
    </footer>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

:root {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  font-size: 16px;
  line-height: 1.5;
  font-weight: 400;

  color-scheme: dark;
}

body {
  margin: 0;
  min-height: 100vh;
  overflow-x: hidden;
}

#app {
  min-height: 100vh;
}
</style>

<style scoped>
.app-container {
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: linear-gradient(135deg, #0f0f1a 0%, #1a1a2e 50%, #16213e 100%);
  overflow: hidden;
}

.background-gradient {
  position: fixed;
  inset: 0;
  z-index: 0;
  background:
    radial-gradient(ellipse 80% 50% at 50% -20%, rgba(99, 102, 241, 0.3), transparent),
    radial-gradient(ellipse 60% 40% at 80% 100%, rgba(168, 85, 247, 0.2), transparent),
    radial-gradient(ellipse 50% 30% at 20% 80%, rgba(34, 211, 238, 0.15), transparent);
  pointer-events: none;
}

.header {
  position: relative;
  z-index: 10;
  padding: 1.5rem 2rem;
}

.header-content {
  max-width: 1400px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(20px);
  border-radius: 20px;
  padding: 1rem 2rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow:
    0 8px 32px rgba(0, 0, 0, 0.2),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
}

.logo {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.logo svg {
  width: 36px;
  height: 36px;
  color: #818cf8;
  filter: drop-shadow(0 0 10px rgba(129, 140, 248, 0.5));
}

.logo h1 {
  font-size: 1.5rem;
  font-weight: 700;
  background: linear-gradient(135deg, #818cf8 0%, #c084fc 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.status-indicator {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 1rem;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 100px;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.status-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.3);
  transition: all 0.3s ease;
}

.status-indicator.active .status-dot {
  background: #22c55e;
  box-shadow: 0 0 10px rgba(34, 197, 94, 0.6);
  animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
}

.status-text {
  font-size: 0.875rem;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.7);
}

.main-content {
  position: relative;
  z-index: 10;
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
}

.camera-wrapper {
  width: 100%;
  max-width: 1200px;
}

.footer {
  position: relative;
  z-index: 10;
  padding: 1.5rem;
  text-align: center;
}

.footer p {
  font-size: 0.875rem;
  color: rgba(255, 255, 255, 0.4);
  background: rgba(255, 255, 255, 0.05);
  display: inline-block;
  padding: 0.5rem 1rem;
  border-radius: 100px;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.05);
}

@media (max-width: 768px) {
  .header-content {
    flex-direction: column;
    gap: 1rem;
    padding: 1rem;
  }

  .logo h1 {
    font-size: 1.25rem;
  }

  .main-content {
    padding: 1rem;
  }
}
</style>
