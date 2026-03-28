<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const slides = [
  {
    id: 1,
    title: '桂林山水',
    subtitle: '世界遗产 · 喀斯特典范',
    image: 'https://images.unsplash.com/photo-1500534314209-a25ddb2bd429?w=1920&q=80'
  },
  {
    id: 2,
    title: '阳朔田园',
    subtitle: '诗意栖居 · 人间仙境',
    image: 'https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?w=1920&q=80'
  },
  {
    id: 3,
    title: '龙脊梯田',
    subtitle: '千年壮乡 · 世界之冠',
    image: 'https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=1920&q=80'
  },
  {
    id: 4,
    title: '德天瀑布',
    subtitle: '跨国奇观 · 亚洲第一',
    image: 'https://images.unsplash.com/photo-1433086966358-54859d0ed716?w=1920&q=80'
  }
]

const current = ref(0)
let timer: ReturnType<typeof setInterval> | null = null

const next = () => {
  current.value = (current.value + 1) % slides.length
}

const prev = () => {
  current.value = (current.value - 1 + slides.length) % slides.length
}

const goTo = (i: number) => {
  current.value = i
}

const startTimer = () => {
  timer = setInterval(next, 6000)
}

const stopTimer = () => {
  if (timer) clearInterval(timer)
}

onMounted(startTimer)
onUnmounted(stopTimer)
</script>

<template>
  <section id="home" class="hero">
    <div class="slides">
      <div 
        v-for="(slide, i) in slides" 
        :key="slide.id"
        :class="['slide', { active: current === i }]"
        :style="{ backgroundImage: `url(${slide.image})` }"
      />
    </div>

    <div class="overlay" />

    <div class="content">
      <div class="container">
        <div class="content-inner">
          <div class="text">
            <p class="subtitle">{{ slides[current].subtitle }}</p>
            <h1 class="title">{{ slides[current].title }}</h1>
            <div class="actions">
              <a href="#attractions" class="btn btn-dark" @click.prevent="$el.closest('section')?.nextElementSibling?.scrollIntoView({ behavior: 'smooth' })">
                探索目的地
              </a>
              <a href="#gallery" class="btn btn-ghost">
                浏览图集
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="nav-arrows">
      <button @click="prev" @mouseenter="stopTimer" @mouseleave="startTimer">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M15 18l-6-6 6-6"/>
        </svg>
      </button>
      <button @click="next" @mouseenter="stopTimer" @mouseleave="startTimer">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M9 18l6-6-6-6"/>
        </svg>
      </button>
    </div>

    <div class="indicators">
      <button 
        v-for="(slide, i) in slides" 
        :key="slide.id"
        :class="{ active: current === i }"
        @click="goTo(i)"
      />
    </div>

    <div class="scroll-hint">
      <span>SCROLL</span>
      <div class="line" />
    </div>
  </section>
</template>

<style scoped>
.hero {
  position: relative;
  height: 100vh;
  min-height: 600px;
  background: var(--black);
  overflow: hidden;
}

.slides {
  position: absolute;
  inset: 0;
}

.slide {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  opacity: 0;
  transform: scale(1.05);
  transition: opacity 1.5s var(--ease), transform 8s linear;
}

.slide.active {
  opacity: 1;
  transform: scale(1);
}

.overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    135deg,
    rgba(10, 10, 10, 0.6) 0%,
    rgba(10, 10, 10, 0.3) 50%,
    rgba(10, 10, 10, 0.5) 100%
  );
}

.content {
  position: relative;
  height: 100%;
  display: flex;
  align-items: center;
  z-index: 10;
}

.content-inner {
  max-width: 700px;
}

.subtitle {
  font-size: 0.75rem;
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: var(--stone-400);
  margin-bottom: 1.5rem;
}

.title {
  font-size: clamp(3rem, 8vw, 6rem);
  font-weight: 400;
  color: var(--white);
  line-height: 1;
  margin-bottom: 3rem;
  letter-spacing: 0.05em;
}

.actions {
  display: flex;
  gap: 1rem;
}

.nav-arrows {
  position: absolute;
  bottom: 50%;
  left: 0;
  right: 0;
  transform: translateY(50%);
  display: flex;
  justify-content: space-between;
  padding: 0 2rem;
  z-index: 20;
  pointer-events: none;
}

.nav-arrows button {
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: white;
  pointer-events: auto;
  transition: all 0.3s;
}

.nav-arrows button:hover {
  background: white;
  color: black;
  border-color: white;
}

.indicators {
  position: absolute;
  bottom: 4rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 0.5rem;
  z-index: 20;
}

.indicators button {
  width: 32px;
  height: 2px;
  background: rgba(255, 255, 255, 0.3);
  transition: all 0.4s;
}

.indicators button.active {
  width: 48px;
  background: white;
}

.scroll-hint {
  position: absolute;
  bottom: 2rem;
  right: 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  z-index: 20;
}

.scroll-hint span {
  font-size: 0.625rem;
  letter-spacing: 0.2em;
  color: rgba(255, 255, 255, 0.4);
}

.scroll-hint .line {
  width: 1px;
  height: 40px;
  background: linear-gradient(to bottom, rgba(255, 255, 255, 0.4), transparent);
  animation: scrollLine 2s infinite;
}

@keyframes scrollLine {
  0% { transform: scaleY(0); transform-origin: top; }
  50% { transform: scaleY(1); transform-origin: top; }
  51% { transform: scaleY(1); transform-origin: bottom; }
  100% { transform: scaleY(0); transform-origin: bottom; }
}

@media (max-width: 768px) {
  .title {
    font-size: clamp(2.5rem, 12vw, 4rem);
  }
  
  .actions {
    flex-direction: column;
  }
  
  .nav-arrows {
    display: none;
  }
  
  .scroll-hint {
    right: 1rem;
    bottom: 1rem;
  }
}
</style>
