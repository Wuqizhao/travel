<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'

interface Spot {
  id: number
  name: string
  location: string
  description: string
  longDescription: string
  image: string
  rating: number
  bestTime: string
  highlights: string[]
}

defineProps<{
  spot: Spot | null
}>()

const emit = defineEmits<{
  (e: 'close'): void
}>()

const handleKey = (e: KeyboardEvent) => {
  if (e.key === 'Escape') emit('close')
}

onMounted(() => document.addEventListener('keydown', handleKey))
onUnmounted(() => document.removeEventListener('keydown', handleKey))
</script>

<template>
  <Teleport to="body">
    <div v-if="spot" class="modal-overlay" @click.self="emit('close')">
      <div class="modal">
        <button class="modal-close" @click="emit('close')">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
            <line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/>
          </svg>
        </button>

        <div class="modal-layout">
          <div class="modal-image">
            <img :src="spot.image" :alt="spot.name" />
          </div>

          <div class="modal-content">
            <span class="modal-loc">{{ spot.location }}</span>
            <h2 class="modal-title">{{ spot.name }}</h2>
            <p class="modal-desc">{{ spot.description }}</p>

            <div class="modal-section">
              <h4>介绍</h4>
              <p>{{ spot.longDescription }}</p>
            </div>

            <div class="modal-section">
              <h4>最佳时间</h4>
              <p>{{ spot.bestTime }}</p>
            </div>

            <div class="modal-section">
              <h4>亮点</h4>
              <div class="highlights">
                <span v-for="h in spot.highlights" :key="h">{{ h }}</span>
              </div>
            </div>

            <div class="modal-actions">
              <button class="btn btn-dark">收藏</button>
              <button class="btn btn-outline" @click="emit('close')">关闭</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </Teleport>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(10, 10, 10, 0.8);
  backdrop-filter: blur(8px);
  z-index: 1500;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
}

.modal {
  background: var(--white);
  max-width: 900px;
  width: 100%;
  max-height: 90vh;
  overflow: hidden;
  position: relative;
}

.modal-close {
  position: absolute;
  top: 1rem;
  right: 1rem;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--white);
  z-index: 10;
  transition: all 0.3s;
}

.modal-close:hover {
  transform: rotate(90deg);
}

.modal-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.modal-image {
  min-height: 500px;
}

.modal-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.modal-content {
  padding: 2.5rem;
  overflow-y: auto;
  max-height: 90vh;
}

.modal-loc {
  font-size: 0.6875rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--stone-400);
}

.modal-title {
  font-size: 2rem;
  margin-top: 0.5rem;
  margin-bottom: 0.75rem;
}

.modal-desc {
  font-size: 0.9375rem;
  color: var(--stone-500);
  line-height: 1.6;
  margin-bottom: 2rem;
  padding-bottom: 1.5rem;
  border-bottom: 1px solid var(--stone-100);
}

.modal-section {
  margin-bottom: 1.5rem;
}

.modal-section h4 {
  font-family: var(--font-sans);
  font-size: 0.75rem;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--stone-400);
  margin-bottom: 0.5rem;
}

.modal-section p {
  font-size: 0.9375rem;
  color: var(--stone-600);
  line-height: 1.7;
}

.highlights {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.highlights span {
  padding: 0.375rem 0.75rem;
  background: var(--stone-100);
  font-size: 0.8125rem;
  color: var(--stone-600);
}

.modal-actions {
  display: flex;
  gap: 0.75rem;
  margin-top: 2rem;
  padding-top: 1.5rem;
  border-top: 1px solid var(--stone-100);
}

@media (max-width: 768px) {
  .modal-overlay {
    padding: 1rem;
  }
  
  .modal-layout {
    grid-template-columns: 1fr;
  }
  
  .modal-image {
    min-height: 250px;
    max-height: 300px;
  }
  
  .modal-content {
    padding: 1.5rem;
  }
  
  .modal-actions {
    flex-direction: column;
  }
  
  .modal-actions .btn {
    width: 100%;
    justify-content: center;
  }
}
</style>
