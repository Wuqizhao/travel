<script setup lang="ts">
import { ref } from 'vue'

const images = [
  { id: 1, src: 'https://images.unsplash.com/photo-1500534314209-a25ddb2bd429?w=600&q=80', title: '漓江晨曦', loc: '桂林' },
  { id: 2, src: 'https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?w=600&q=80', title: '阳朔山水', loc: '阳朔' },
  { id: 3, src: 'https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=600&q=80', title: '龙脊梯田', loc: '龙胜' },
  { id: 4, src: 'https://images.unsplash.com/photo-1433086966358-54859d0ed716?w=600&q=80', title: '瀑布飞流', loc: '崇左' },
  { id: 5, src: 'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?w=600&q=80', title: '银滩落日', loc: '北海' },
  { id: 6, src: 'https://images.unsplash.com/photo-1501785888041-af3ef285b470?w=600&q=80', title: '古镇晨雾', loc: '贺州' },
  { id: 7, src: 'https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?w=600&q=80', title: '山水画廊', loc: '桂林' },
  { id: 8, src: 'https://images.unsplash.com/photo-1469474968028-56623f02e42e?w=600&q=80', title: '云海仙境', loc: '百色' },
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
  <section id="gallery" class="gallery">
    <div class="container">
      <div class="section-header" style="color: white;">
        <p class="text-xs tracking-widest uppercase" style="color: var(--stone-500);">Gallery</p>
        <h2 class="text-4xl" style="margin-top: 1rem; color: white;">影像</h2>
      </div>
    </div>

    <!-- Infinite Scroll Track -->
    <div class="marquee-wrapper">
      <div class="marquee-track">
        <div 
          v-for="(img, i) in [...images, ...images]" 
          :key="`${img.id}-${i}`"
          class="marquee-item"
          @click="open(i % images.length)"
        >
          <img :src="img.src" :alt="img.title" loading="lazy" />
          <div class="item-overlay">
            <span class="item-loc">{{ img.loc }}</span>
            <span class="item-title">{{ img.title }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Reverse Track -->
    <div class="marquee-wrapper reverse">
      <div class="marquee-track">
        <div 
          v-for="(img, i) in [...images.slice(4), ...images.slice(4), ...images.slice(4)]" 
          :key="`r-${img.id}-${i}`"
          class="marquee-item"
          @click="open((i + 4) % images.length)"
        >
          <img :src="img.src" :alt="img.title" loading="lazy" />
          <div class="item-overlay">
            <span class="item-loc">{{ img.loc }}</span>
            <span class="item-title">{{ img.title }}</span>
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
.gallery {
  background: var(--black);
  padding: clamp(4rem, 10vw, 8rem) 0 clamp(2rem, 5vw, 4rem);
  overflow: hidden;
}

.marquee-wrapper {
  overflow: hidden;
  padding: 1rem 0;
}

.marquee-track {
  display: flex;
  gap: 1rem;
  animation: scroll-left 40s linear infinite;
  width: max-content;
}

.marquee-wrapper.reverse .marquee-track {
  animation: scroll-right 35s linear infinite;
}

@keyframes scroll-left {
  0% { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}

@keyframes scroll-right {
  0% { transform: translateX(-50%); }
  100% { transform: translateX(0); }
}

.marquee-item {
  flex-shrink: 0;
  width: 350px;
  height: 250px;
  position: relative;
  overflow: hidden;
  cursor: pointer;
}

.marquee-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.6s var(--ease);
}

.marquee-item:hover img {
  transform: scale(1.1);
}

.item-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(10, 10, 10, 0.7), transparent 60%);
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding: 1.25rem;
  opacity: 0;
  transition: opacity 0.3s;
}

.marquee-item:hover .item-overlay {
  opacity: 1;
}

.item-loc {
  font-size: 0.625rem;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.5);
}

.item-title {
  font-family: var(--font-serif);
  font-size: 1.25rem;
  color: white;
  margin-top: 0.25rem;
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
  transition: all 0.3s;
}

.lb-close:hover,
.lb-nav:hover {
  background: white;
  color: black;
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

@media (max-width: 640px) {
  .marquee-item {
    width: 280px;
    height: 200px;
  }
}
</style>
