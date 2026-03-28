<script setup lang="ts">
import { ref, computed } from 'vue'

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

const emit = defineEmits<{
  (e: 'select-spot', spot: Spot): void
}>()

const handleTilt = (e: MouseEvent) => {
  const card = e.currentTarget as HTMLElement
  const rect = card.getBoundingClientRect()
  const x = e.clientX - rect.left
  const y = e.clientY - rect.top
  const centerX = rect.width / 2
  const centerY = rect.height / 2
  const rotateX = (y - centerY) / 30
  const rotateY = (centerX - x) / 30
  
  card.style.transform = `perspective(1000px) rotateX(${-rotateX}deg) rotateY(${-rotateY}deg) translateZ(10px)`
}

const resetTilt = (e: MouseEvent) => {
  const card = e.currentTarget as HTMLElement
  card.style.transform = 'perspective(1000px) rotateX(0) rotateY(0) translateZ(0)'
}

const spots: Spot[] = [
  {
    id: 1,
    name: '漓江风景区',
    location: '桂林市',
    description: '世界自然遗产，百里画廊，喀斯特地貌的典范',
    longDescription: '漓江风景区是世界上规模最大、风景最美的岩溶山水游览区。从桂林至阳朔的83公里漓江河段，是喀斯特地貌发育最典型、最集中、最丰富的地段。',
    image: 'https://images.unsplash.com/photo-1500534314209-a25ddb2bd429?w=800&q=80',
    rating: 4.9,
    bestTime: '4月-10月',
    highlights: ['竹筏漂流', '象鼻山', '九马画山', '黄布倒影']
  },
  {
    id: 2,
    name: '阳朔西街',
    location: '桂林市阳朔县',
    description: '中西文化交融的古镇街道，充满异国情调',
    longDescription: '阳朔西街有1400多年历史，是阳朔最古老最繁华的街道。街道两旁是桂北明清时期的青瓦白墙建筑。',
    image: 'https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?w=800&q=80',
    rating: 4.7,
    bestTime: '全年',
    highlights: ['西街夜景', '印象刘三姐', '啤酒鱼', '攀岩体验']
  },
  {
    id: 3,
    name: '龙脊梯田',
    location: '桂林市龙胜县',
    description: '千年壮族梯田奇观，世界梯田原乡',
    longDescription: '龙脊梯田始建于元朝，完工于清初，距今已有650多年历史。从山脚盘绕到山顶，层层叠叠，规模磅礴壮观。',
    image: 'https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=800&q=80',
    rating: 4.8,
    bestTime: '5月-6月, 9月-10月',
    highlights: ['金坑梯田', '平安壮寨', '云海日出', '龙脊古壮寨']
  },
  {
    id: 4,
    name: '德天跨国瀑布',
    location: '崇左市大新县',
    description: '亚洲最大跨国瀑布，中越边境奇观',
    longDescription: '德天瀑布位于中越边境，是亚洲第一、世界第四大跨国瀑布。瀑布宽约200米，落差70米，气势磅礴。',
    image: 'https://images.unsplash.com/photo-1433086966358-54859d0ed716?w=800&q=80',
    rating: 4.8,
    bestTime: '6月-11月',
    highlights: ['瀑布观景', '中越边境', '归春河', '明仕田园']
  },
  {
    id: 5,
    name: '北海银滩',
    location: '北海市',
    description: '中国第一滩，细白如银的沙滩',
    longDescription: '北海银滩以"滩长平、沙细白、水温净、浪柔软、无鲨鱼"等特点，被誉为"中国第一滩"。',
    image: 'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?w=800&q=80',
    rating: 4.6,
    bestTime: '4月-11月',
    highlights: ['银滩日落', '海滩游泳', '涠洲岛', '海鲜美食']
  },
  {
    id: 6,
    name: '黄姚古镇',
    location: '贺州市昭平县',
    description: '千年古镇，梦境家园，岭南文化典范',
    longDescription: '黄姚古镇发祥于宋朝年间，保存有300多间明清宅院，青砖黛瓦，飞檐画栋。',
    image: 'https://images.unsplash.com/photo-1501785888041-af3ef285b470?w=800&q=80',
    rating: 4.5,
    bestTime: '3月-5月, 9月-11月',
    highlights: ['古戏台', '带龙桥', '石跳桥', '仙人古井']
  }
]

const activeCategory = ref('all')

const categories = [
  { id: 'all', name: '全部' },
  { id: 'guilin', name: '桂林' },
  { id: 'coast', name: '滨海' },
  { id: 'culture', name: '人文' }
]

const filtered = computed(() => {
  if (activeCategory.value === 'all') return spots
  if (activeCategory.value === 'guilin') return spots.filter(s => [1, 2, 3].includes(s.id))
  if (activeCategory.value === 'coast') return spots.filter(s => s.id === 5)
  if (activeCategory.value === 'culture') return spots.filter(s => [4, 6].includes(s.id))
  return spots
})
</script>

<template>
  <section id="attractions" class="section">
    <div class="container">
      <div class="section-header">
        <p class="text-xs tracking-widest uppercase text-stone-400">Destinations</p>
        <h2 class="text-4xl" style="margin-top: 1rem;">精选目的地</h2>
        <p class="text-stone-500" style="margin-top: 1rem; max-width: 500px;">
          从桂林山水到北部湾海岸，探索广西最迷人的风景
        </p>
      </div>

      <div class="filters">
        <button 
          v-for="cat in categories"
          :key="cat.id"
          :class="['filter-btn', { active: activeCategory === cat.id }]"
          @click="activeCategory = cat.id"
        >
          {{ cat.name }}
        </button>
      </div>

      <div class="grid">
        <article 
          v-for="(spot, index) in filtered" 
          :key="spot.id"
          :class="['card', { 'card-large': index === 0 }]"
          @click="emit('select-spot', spot)"
          @mousemove="handleTilt"
          @mouseleave="resetTilt"
        >
          <div class="card-image">
            <img :src="spot.image" :alt="spot.name" loading="lazy" />
            <div class="card-shine"></div>
          </div>
          <div class="card-body">
            <span class="card-location">{{ spot.location }}</span>
            <h3 class="card-title">{{ spot.name }}</h3>
            <p class="card-desc">{{ spot.description }}</p>
          </div>
        </article>
      </div>
    </div>
  </section>
</template>

<style scoped>
.section {
  background: var(--stone-50);
}

.section-header {
  margin-bottom: 3rem;
}

.filters {
  display: flex;
  gap: 0.25rem;
  margin-bottom: 2rem;
}

.filter-btn {
  padding: 0.5rem 1rem;
  font-size: 0.8125rem;
  color: var(--stone-500);
  transition: all 0.3s;
}

.filter-btn:hover {
  color: var(--black);
}

.filter-btn.active {
  color: var(--black);
  border-bottom: 1px solid var(--black);
}

.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
}

.card {
  background: var(--white);
  cursor: pointer;
  transition: transform 0.3s var(--ease), box-shadow 0.3s var(--ease);
  transform-style: preserve-3d;
  will-change: transform;
}

.card:hover {
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.15);
}

.card-large {
  grid-column: span 2;
  grid-row: span 2;
}

.card-image {
  position: relative;
  overflow: hidden;
  aspect-ratio: 4/3;
}

.card-large .card-image {
  aspect-ratio: auto;
  height: 100%;
  min-height: 400px;
}

.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.8s var(--ease);
}

.card:hover .card-image img {
  transform: scale(1.08);
}

.card-shine {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    135deg,
    rgba(255, 255, 255, 0.2) 0%,
    transparent 40%,
    transparent 60%,
    rgba(255, 255, 255, 0.1) 100%
  );
  opacity: 0;
  transition: opacity 0.4s;
  pointer-events: none;
}

.card:hover .card-shine {
  opacity: 1;
}

.card-body {
  padding: 1.25rem;
}

.card-large .card-body {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 2rem;
  background: linear-gradient(to top, rgba(10, 10, 10, 0.8), transparent);
  color: var(--white);
}

.card-location {
  font-size: 0.6875rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--stone-400);
}

.card-large .card-location {
  color: rgba(255, 255, 255, 0.6);
}

.card-title {
  font-size: 1.25rem;
  margin-top: 0.375rem;
}

.card-large .card-title {
  font-size: 1.75rem;
}

.card-desc {
  font-size: 0.8125rem;
  color: var(--stone-500);
  margin-top: 0.5rem;
  line-height: 1.5;
}

.card-large .card-desc {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.875rem;
}

@media (max-width: 1024px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 640px) {
  .grid {
    grid-template-columns: 1fr;
  }
  
  .card-large {
    grid-column: span 1;
    grid-row: span 1;
  }
  
  .card-large .card-image {
    min-height: 280px;
  }
}
</style>
