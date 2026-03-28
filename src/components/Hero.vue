<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const slides = [
  { id: 1, city: '南宁', title: '绿城南宁', subtitle: '东盟之窗 · 半城绿树', image: 'https://images.unsplash.com/photo-1506953823976-52e1fdc0149a?w=1920&q=80' },
  { id: 2, city: '柳州', title: '柳州奇石', subtitle: '工业之城 · 螺蛳粉乡', image: 'https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?w=1920&q=80' },
  { id: 3, city: '桂林', title: '桂林山水', subtitle: '世界遗产 · 喀斯特典范', image: 'https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=1920&q=80' },
  { id: 4, city: '梧州', title: '百年商埠', subtitle: '岭南古郡 · 六堡茶香', image: 'https://images.unsplash.com/photo-1433086966358-54859d0ed716?w=1920&q=80' },
  { id: 5, city: '北海', title: '北海银滩', subtitle: '海上丝路 · 碧海银沙', image: 'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?w=1920&q=80' },
  { id: 6, city: '防城港', title: '边海港城', subtitle: '京族三岛 · 金滩碧海', image: 'https://images.unsplash.com/photo-1501785888041-af3ef285b470?w=1920&q=80' },
  { id: 7, city: '钦州', title: '海豚之乡', subtitle: '坭兴陶都 · 海韵钦州', image: 'https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?w=1920&q=80' },
  { id: 8, city: '贵港', title: '荷城贵港', subtitle: '西江明珠 · 古郡新城', image: 'https://images.unsplash.com/photo-1469474968028-56623f02e42e?w=1920&q=80' },
  { id: 9, city: '玉林', title: '岭南美玉', subtitle: '都会之林 · 胜景如画', image: 'https://images.unsplash.com/photo-1519681393784-d120267933ba?w=1920&q=80' },
  { id: 10, city: '百色', title: '百色起义', subtitle: '红色圣地 · 壮乡摇篮', image: 'https://images.unsplash.com/photo-1426604966848-d7adac402bff?w=1920&q=80' },
  { id: 11, city: '贺州', title: '黄姚古镇', subtitle: '梦境家园 · 长寿之乡', image: 'https://images.unsplash.com/photo-1494500764479-0c8f2919a3d8?w=1920&q=80' },
  { id: 12, city: '河池', title: '长寿河池', subtitle: '铜鼓之乡 · 生态宝地', image: 'https://images.unsplash.com/photo-1472396961693-142e6e269027?w=1920&q=80' },
  { id: 13, city: '来宾', title: '桂中明珠', subtitle: '盘古文化 · 瑶都风情', image: 'https://images.unsplash.com/photo-1500534623283-312aade485b7?w=1920&q=80' },
  { id: 14, city: '崇左', title: '德天瀑布', subtitle: '跨国奇观 · 边关风情', image: 'https://images.unsplash.com/photo-1511884642898-4c92249e20b6?w=1920&q=80' }
]

const current = ref(0)
const mouseX = ref(0)
const mouseY = ref(0)
let timer: ReturnType<typeof setInterval> | null = null

const next = () => {
  current.value = (current.value + 1) % slides.length
}

const prev = () => {
  current.value = (current.value - 1 + slides.length) % slides.length
}

const goTo = (i: number) => {
  current.value = i
  resetTimer()
}

const handleMouseMove = (e: MouseEvent) => {
  const rect = (e.currentTarget as HTMLElement).getBoundingClientRect()
  mouseX.value = ((e.clientX - rect.left) / rect.width - 0.5) * 10
  mouseY.value = ((e.clientY - rect.top) / rect.height - 0.5) * 10
}

const handleMouseLeave = () => {
  mouseX.value = 0
  mouseY.value = 0
}

const scrollToAttractions = () => {
  document.getElementById('attractions')?.scrollIntoView({ behavior: 'smooth' })
}

const startTimer = () => {
  timer = setInterval(next, 5000)
}

const stopTimer = () => {
  if (timer) clearInterval(timer)
}

const resetTimer = () => {
  stopTimer()
  startTimer()
}

onMounted(startTimer)
onUnmounted(stopTimer)
</script>

<template>
  <section 
    id="home" 
    class="hero"
    @mousemove="handleMouseMove"
    @mouseleave="handleMouseLeave"
  >
    <div class="slides">
      <div 
        v-for="(slide, i) in slides" 
        :key="slide.id"
        :class="['slide', { active: current === i }]"
        :style="{ 
          backgroundImage: `url(${slide.image})`,
          transform: `scale(1.1) translate(${mouseX * 0.3}px, ${mouseY * 0.3}px)`
        }"
      />
    </div>

    <div class="overlay" />

    <div class="content">
      <div class="container">
        <div 
          class="content-inner"
          :style="{ transform: `perspective(1000px) rotateY(${mouseX * 0.1}deg) rotateX(${mouseY * -0.1}deg)` }"
        >
          <p class="subtitle">{{ slides[current].subtitle }}</p>
          <h1 class="title">{{ slides[current].title }}</h1>
          <div class="actions">
            <a href="#attractions" class="btn btn-dark" @click.prevent="scrollToAttractions">
              探索目的地
            </a>
          </div>
        </div>
      </div>
    </div>

    <!-- Navigation Arrows -->
    <div class="nav-arrows">
      <button class="arrow-btn" @click="prev" @mouseenter="stopTimer" @mouseleave="startTimer">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M15 18l-6-6 6-6"/>
        </svg>
      </button>
      <button class="arrow-btn" @click="next" @mouseenter="stopTimer" @mouseleave="startTimer">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
          <path d="M9 18l6-6-6-6"/>
        </svg>
      </button>
    </div>

    <!-- City Indicators -->
    <div class="indicators">
      <button 
        v-for="(slide, i) in slides" 
        :key="slide.id"
        :class="['indicator', { active: current === i }]"
        @click="goTo(i)"
        @mouseenter="goTo(i)"
      >
        <span class="indicator-text">{{ slide.city }}</span>
        <span class="indicator-dot"></span>
      </button>
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
  inset: -5%;
  background-size: cover;
  background-position: center;
  opacity: 0;
  transition: opacity 1.2s var(--ease), transform 0.15s ease-out;
}

.slide.active {
  opacity: 1;
}

.overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(10,10,10,0.6) 0%, rgba(10,10,10,0.25) 50%, rgba(10,10,10,0.5) 100%);
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
  transition: transform 0.15s ease-out;
  transform-style: preserve-3d;
}

.subtitle {
  font-size: 0.75rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(255,255,255,0.5);
  margin-bottom: 1rem;
}

.title {
  font-size: clamp(3rem, 8vw, 5.5rem);
  font-weight: 400;
  color: var(--white);
  line-height: 1;
  margin-bottom: 2.5rem;
  letter-spacing: 0.08em;
}

.actions {
  display: flex;
  gap: 1rem;
}

/* Arrow Buttons */
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

.arrow-btn {
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(255,255,255,0.5);
  pointer-events: auto;
  transition: all 0.3s;
  background: transparent;
  border: none;
}

.arrow-btn:hover {
  color: white;
  transform: scale(1.1);
}

/* City Indicators - Text + Dot */
.indicators {
  position: absolute;
  bottom: 2.5rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 1.5rem;
  z-index: 20;
}

.indicator {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  background: transparent;
  border: none;
  cursor: pointer;
  padding: 0;
  transition: all 0.3s;
}

.indicator-text {
  font-size: 0.625rem;
  letter-spacing: 0.1em;
  color: rgba(255,255,255,0.3);
  transition: color 0.3s;
}

.indicator:hover .indicator-text {
  color: rgba(255,255,255,0.6);
}

.indicator.active .indicator-text {
  color: rgba(255,255,255,0.9);
}

.indicator-dot {
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background: rgba(255,255,255,0.2);
  transition: all 0.4s;
}

.indicator:hover .indicator-dot {
  background: rgba(255,255,255,0.4);
  transform: scale(1.3);
}

.indicator.active .indicator-dot {
  width: 20px;
  border-radius: 2px;
  background: rgba(255,255,255,0.8);
}

.scroll-hint {
  position: absolute;
  bottom: 1.5rem;
  right: 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  z-index: 20;
}

.scroll-hint span {
  font-size: 0.5625rem;
  letter-spacing: 0.2em;
  color: rgba(255,255,255,0.3);
}

.scroll-hint .line {
  width: 1px;
  height: 30px;
  background: linear-gradient(to bottom, rgba(255,255,255,0.3), transparent);
  animation: scroll-line 2s infinite;
}

@keyframes scroll-line {
  0% { transform: scaleY(0); transform-origin: top; }
  50% { transform: scaleY(1); transform-origin: top; }
  51% { transform: scaleY(1); transform-origin: bottom; }
  100% { transform: scaleY(0); transform-origin: bottom; }
}

@media (max-width: 768px) {
  .title {
    font-size: clamp(2.5rem, 12vw, 4rem);
  }
  
  .nav-arrows {
    display: none;
  }
  
  .indicators {
    gap: 0.75rem;
    bottom: 1.5rem;
    flex-wrap: wrap;
    justify-content: center;
    max-width: 90%;
  }
  
  .indicator-text {
    font-size: 0.5rem;
  }
}
</style>
