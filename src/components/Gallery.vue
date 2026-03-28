<script setup lang="ts">
import { ref } from 'vue'

const images = [
  { id: 1, src: 'https://images.unsplash.com/photo-1500534314209-a25ddb2bd429?w=800&q=80', title: '漓江晨曦', loc: '桂林' },
  { id: 2, src: 'https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?w=800&q=80', title: '阳朔山水', loc: '阳朔' },
  { id: 3, src: 'https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=800&q=80', title: '龙脊梯田', loc: '龙胜' },
  { id: 4, src: 'https://images.unsplash.com/photo-1433086966358-54859d0ed716?w=800&q=80', title: '瀑布飞流', loc: '崇左' },
  { id: 5, src: 'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?w=800&q=80', title: '银滩落日', loc: '北海' },
  { id: 6, src: 'https://images.unsplash.com/photo-1501785888041-af3ef285b470?w=800&q=80', title: '古镇晨雾', loc: '贺州' },
  { id: 7, src: 'https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?w=800&q=80', title: '山水画廊', loc: '桂林' },
  { id: 8, src: 'https://images.unsplash.com/photo-1469474968028-56623f02e42e?w=800&q=80', title: '云海仙境', loc: '桂林' }
]

const current = ref(-1)

const open = (i: number) => {
  current.value = i
  document.body.style.overflow = 'hidden'
}

const close = () => {
  current.value = -1
  document.body.style.overflow = ''
}

const next = () => {
  current.value = (current.value + 1) % images.length
}

const prev = () => {
  current.value = (current.value - 1 + images.length) % images.length
}
</script>

<template>
  <section id="gallery" class="section" style="background: var(--black);">
    <div class="container">
      <div class="section-header" style="color: white;">
        <p class="text-xs tracking-widest uppercase" style="color: var(--stone-500);">Gallery</p>
        <h2 class="text-4xl" style="margin-top: 1rem; color: white;">影像</h2>
      </div>

      <div class="gallery-grid">
        <div 
          v-for="(img, i) in images" 
          :key="img.id"
          :class="['gallery-item', { large: i === 0 || i === 4 }]"
          @click="open(i)"
        >
          <img :src="img.src" :alt="img.title" loading="lazy" />
          <div class="gallery-overlay">
            <span class="gallery-loc">{{ img.loc }}</span>
            <span class="gallery-title">{{ img.title }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Lightbox -->
    <Teleport to="body">
      <div v-if="current >= 0" class="lightbox" @click.self="close">
        <button class="lb-close" @click="close">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
            <line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/>
          </svg>
        </button>
        <button class="lb-nav lb-prev" @click="prev">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
            <path d="M15 18l-6-6 6-6"/>
          </svg>
        </button>
        <img :src="images[current].src" :alt="images[current].title" class="lb-img" />
        <button class="lb-nav lb-next" @click="next">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
            <path d="M9 18l6-6-6-6"/>
          </svg>
        </button>
        <div class="lb-info">
          <span>{{ images[current].loc }}</span>
          <span>{{ current + 1 }} / {{ images.length }}</span>
        </div>
      </div>
    </Teleport>
  </section>
</template>

<style scoped>
.gallery-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 200px;
  gap: 0.5rem;
}

.gallery-item {
  position: relative;
  overflow: hidden;
  cursor: pointer;
}

.gallery-item.large {
  grid-column: span 2;
  grid-row: span 2;
}

.gallery-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.6s var(--ease);
}

.gallery-item:hover img {
  transform: scale(1.08);
}

.gallery-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(10, 10, 10, 0.7), transparent 50%);
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding: 1rem;
  opacity: 0;
  transition: opacity 0.3s;
}

.gallery-item:hover .gallery-overlay {
  opacity: 1;
}

.gallery-loc {
  font-size: 0.6875rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.6);
}

.gallery-title {
  font-family: var(--font-serif);
  font-size: 1.125rem;
  color: white;
}

/* Lightbox */
.lightbox {
  position: fixed;
  inset: 0;
  background: rgba(10, 10, 10, 0.95);
  z-index: 2000;
  display: flex;
  align-items: center;
  justify-content: center;
}

.lb-img {
  max-width: 85vw;
  max-height: 80vh;
  object-fit: contain;
}

.lb-close,
.lb-nav {
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 44px;
  height: 44px;
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.15);
  transition: all 0.3s;
}

.lb-close:hover,
.lb-nav:hover {
  background: white;
  color: black;
  border-color: white;
}

.lb-close {
  top: 1.5rem;
  right: 1.5rem;
}

.lb-prev {
  left: 1.5rem;
  top: 50%;
  transform: translateY(-50%);
}

.lb-next {
  right: 1.5rem;
  top: 50%;
  transform: translateY(-50%);
}

.lb-info {
  position: absolute;
  bottom: 1.5rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 2rem;
  font-size: 0.75rem;
  letter-spacing: 0.1em;
  color: rgba(255, 255, 255, 0.5);
}

@media (max-width: 1024px) {
  .gallery-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 640px) {
  .gallery-grid {
    grid-template-columns: 1fr;
  }
  
  .gallery-item.large {
    grid-column: span 1;
    grid-row: span 1;
  }
}
</style>
