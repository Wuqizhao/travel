<script setup lang="ts">
import { ref, computed } from 'vue'

const cities = [
  { name: '南宁', counties: ['兴宁区', '青秀区', '江南区', '西乡塘区', '良庆区', '邕宁区', '武鸣区', '隆安县', '马山县', '上林县', '宾阳县', '横州市'] },
  { name: '柳州', counties: ['城中区', '鱼峰区', '柳南区', '柳北区', '柳江区', '柳城县', '鹿寨县', '融安县', '融水县', '三江县'] },
  { name: '桂林', counties: ['秀峰区', '叠彩区', '象山区', '七星区', '雁山区', '临桂区', '阳朔县', '灵川县', '全州县', '兴安县', '永福县', '灌阳县', '龙胜县', '资源县', '平乐县', '恭城县', '荔浦市'] },
  { name: '梧州', counties: ['万秀区', '长洲区', '龙圩区', '苍梧县', '藤县', '蒙山县', '岑溪市'] },
  { name: '北海', counties: ['海城区', '银海区', '铁山港区', '合浦县'] },
  { name: '防城港', counties: ['港口区', '防城区', '上思县', '东兴市'] },
  { name: '钦州', counties: ['钦南区', '钦北区', '灵山县', '浦北县'] },
  { name: '贵港', counties: ['港北区', '港南区', '覃塘区', '平南县', '桂平市'] },
  { name: '玉林', counties: ['玉州区', '福绵区', '容县', '陆川县', '博白县', '兴业县', '北流市'] },
  { name: '百色', counties: ['右江区', '田阳区', '田东县', '德保县', '那坡县', '凌云县', '乐业县', '田林县', '西林县', '隆林县', '靖西市', '平果市'] },
  { name: '贺州', counties: ['八步区', '平桂区', '昭平县', '钟山县', '富川县'] },
  { name: '河池', counties: ['金城江区', '宜州区', '南丹县', '天峨县', '凤山县', '东兰县', '罗城县', '环江县', '巴马县', '都安县', '大化县'] },
  { name: '来宾', counties: ['兴宾区', '忻城县', '象州县', '武宣县', '金秀县', '合山市'] },
  { name: '崇左', counties: ['江州区', '扶绥县', '宁明县', '龙州县', '大新县', '天等县', '凭祥市'] }
]

const images = [
  'https://images.unsplash.com/photo-1500534314209-a25ddb2bd429?w=200&q=60',
  'https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?w=200&q=60',
  'https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=200&q=60',
  'https://images.unsplash.com/photo-1433086966358-54859d0ed716?w=200&q=60',
  'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?w=200&q=60',
  'https://images.unsplash.com/photo-1501785888041-af3ef285b470?w=200&q=60',
  'https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?w=200&q=60',
  'https://images.unsplash.com/photo-1469474968028-56623f02e42e?w=200&q=60',
  'https://images.unsplash.com/photo-1519681393784-d120267933ba?w=200&q=60',
  'https://images.unsplash.com/photo-1426604966848-d7adac402bff?w=200&q=60',
  'https://images.unsplash.com/photo-1494500764479-0c8f2919a3d8?w=200&q=60',
  'https://images.unsplash.com/photo-1472396961693-142e6e269027?w=200&q=60',
  'https://images.unsplash.com/photo-1511884642898-4c92249e20b6?w=200&q=60',
  'https://images.unsplash.com/photo-1500534623283-312aade485b7?w=200&q=60',
  'https://images.unsplash.com/photo-1506953823976-52e1fdc0149a?w=200&q=60'
]

const activeCity = ref(0)
const currentCounties = computed(() => cities[activeCity.value].counties)
const getImage = (i: number) => images[i % images.length]

// 左括号弧线
const getArcX = (t: number) => {
  const max = 20
  return max - Math.sin(t * Math.PI) * max
}

// 蜂巢网格 - 使用grid布局 + 行偏移
const hexRows = computed(() => {
  const counties = currentCounties.value
  const rows: string[][] = []
  const c = counties.length

  // 确定每行数量，中间行最长
  let sizes: number[] = []
  if (c <= 3) sizes = [c]
  else if (c <= 5) sizes = [2, c - 2]
  else if (c <= 8) sizes = [3, c - 3]
  else if (c <= 12) sizes = [3, 4, c - 7].filter(x => x > 0)
  else if (c <= 17) sizes = [3, 4, 5, c - 12].filter(x => x > 0)
  else sizes = [4, 5, 6, c - 15].filter(x => x > 0)

  let idx = 0
  for (const size of sizes) {
    const row: string[] = []
    for (let i = 0; i < size && idx < c; i++) {
      row.push(counties[idx++])
    }
    if (row.length) rows.push(row)
  }
  return rows
})
</script>

<template>
  <section id="counties" class="honeycomb-section">
    <div class="section-header">
      <p class="text-xs tracking-widest uppercase text-stone-400">Counties & Districts</p>
      <h2 class="text-4xl" style="margin-top: 1rem; color: white;">区县全景</h2>
    </div>

    <div class="honeycomb-wrapper">
      <!-- Left: Parenthesis Arc -->
      <div class="arc-list">
        <svg class="arc-svg" viewBox="0 0 60 500" preserveAspectRatio="xMidYMid meet">
          <path d="M42 15 C12 170 12 330 42 485" fill="none" stroke="rgba(255,255,255,0.06)" stroke-width="1" stroke-dasharray="3 6"/>
        </svg>
        <button 
          v-for="(city, i) in cities" 
          :key="city.name"
          :class="['arc-item', { active: activeCity === i }]"
          :style="{ transform: `translateX(${getArcX(i / (cities.length - 1))}px)` }"
          @click="activeCity = i"
        >
          <span class="arc-name">{{ city.name }}</span>
        </button>
      </div>

      <!-- Right: Hex Grid -->
      <div class="hex-area">
        <div 
          v-for="(row, ri) in hexRows" 
          :key="ri"
          class="hex-row"
          :class="{ 'hex-row-offset': ri % 2 === 1 }"
        >
          <div 
            v-for="(name, ci) in row" 
            :key="name"
            class="hex"
            :style="{ '--bg': `url(${getImage(ri * 5 + ci)})`, '--d': `${(ri * row.length + ci) * 0.03}s` }"
          >
            <div class="hex-bg"></div>
            <div class="hex-dim"></div>
            <span class="hex-label">{{ name }}</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.honeycomb-section {
  padding: clamp(4rem, 10vw, 6rem) 0;
  background: var(--black);
}

.section-header {
  text-align: center;
  margin-bottom: 3rem;
}

.honeycomb-wrapper {
  max-width: 1100px;
  margin: 0 auto;
  padding: 0 2rem;
  display: grid;
  grid-template-columns: 120px 1fr;
  gap: 2rem;
  align-items: center;
}

/* Arc List */
.arc-list {
  position: sticky;
  top: 100px;
  height: 480px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 0.5rem 0;
}

.arc-svg {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}

.arc-item {
  position: relative;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.25rem 0;
  transition: all 0.3s var(--ease);
  z-index: 1;
  text-align: left;
}

.arc-name {
  font-family: var(--font-serif);
  font-size: 0.875rem;
  color: rgba(255,255,255,0.28);
  transition: color 0.3s;
}

.arc-item:hover .arc-name { color: rgba(255,255,255,0.6); }
.arc-item.active .arc-name { color: white; }

/* Hex Grid */
.hex-area {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}

.hex-row {
  display: flex;
  gap: 16px;
}

/* 奇数行右移 */
.hex-row-offset {
  margin-left: 63px;
}

/* 单个蜂巢 - 正六边形 */
.hex {
  --size: 110px;
  width: var(--size);
  height: var(--size);
  position: relative;
  /* 正六边形顶点：270° 330° 30° 90° 150° 210° */
  clip-path: polygon(50% 0%, 93.3% 25%, 93.3% 75%, 50% 100%, 6.7% 75%, 6.7% 25%);
  cursor: pointer;
  animation: hex-pop 0.4s var(--ease) both;
  animation-delay: var(--d);
  transition: transform 0.3s var(--ease);
}

.hex:hover {
  transform: scale(1.15);
  z-index: 10;
}

.hex:active {
  transform: scale(0.95);
}

@keyframes hex-pop {
  from { opacity: 0; transform: scale(0.5) rotate(-10deg); }
  to { opacity: 1; transform: scale(1) rotate(0deg); }
}

.hex-bg {
  position: absolute;
  inset: 0;
  background-image: var(--bg);
  background-size: cover;
  background-position: center;
  transition: transform 0.5s var(--ease);
}

.hex:hover .hex-bg {
  transform: scale(1.2) rotate(5deg);
}

.hex-dim {
  position: absolute;
  inset: 0;
  background: rgba(10,10,10,0.5);
  transition: background 0.3s;
}

.hex:hover .hex-dim {
  background: rgba(10,10,10,0.15);
}

.hex-label {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.6875rem;
  color: white;
  font-weight: 500;
  text-shadow: 0 1px 3px rgba(0,0,0,0.8);
  padding: 0.5rem;
  text-align: center;
  line-height: 1.15;
  transition: all 0.3s;
}

.hex:hover .hex-label {
  font-size: 0.75rem;
  font-weight: 600;
}

/* 首尾行半透明 */
.hex-row:first-child .hex,
.hex-row:last-child .hex {
  opacity: 0.5;
}

.hex-row:first-child .hex:hover,
.hex-row:last-child .hex:hover {
  opacity: 1;
}

@media (max-width: 900px) {
  .honeycomb-wrapper {
    grid-template-columns: 1fr;
  }
  
  .arc-list {
    flex-direction: row;
    flex-wrap: wrap;
    height: auto;
    position: static;
    gap: 0.25rem;
    justify-content: center;
  }
  
  .arc-item { transform: none !important; }
  .arc-svg { display: none; }
  
  .hex { --size: 80px; }
  .hex-row-offset { margin-left: 46px; }
}
</style>
