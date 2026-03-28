<script setup lang="ts">
const features = [
  {
    num: '01',
    title: '喀斯特地貌',
    desc: '峰林、溶洞、地下河遍布，形成独特的自然奇观'
  },
  {
    num: '02',
    title: '风味美食',
    desc: '桂林米粉、柳州螺蛳粉，口味独特，回味无穷'
  },
  {
    num: '03',
    title: '民族风情',
    desc: '壮、瑶、苗、侗等十二个世居民族，文化多彩'
  },
  {
    num: '04',
    title: '碧海银滩',
    desc: '一千六百公里海岸线，海滨风光旖旎'
  },
  {
    num: '05',
    title: '生态绿洲',
    desc: '森林覆盖率超过百分之六十二，天然森林氧吧'
  },
  {
    num: '06',
    title: '千年文脉',
    desc: '从灵渠到花山岩画，历史文化遗产见证文明'
  }
]

const stats = [
  { value: '1600+', label: '公里海岸线' },
  { value: '50+', label: 'A级景区' },
  { value: '12', label: '世居民族' },
  { value: '62%', label: '森林覆盖率' }
]
</script>

<template>
  <section id="features" class="section">
    <div class="container">
      <div class="split">
        <div class="left">
          <p class="text-xs tracking-widest uppercase text-stone-400">Why Guangxi</p>
          <h2 class="text-5xl" style="margin-top: 1rem;">为何选择<br/>广西</h2>
          <p class="text-stone-500" style="margin-top: 1.5rem; line-height: 1.8;">
            这片神奇的土地，集山水之秀美、人文之深厚、风情之独特于一体，等待你的探索。
          </p>
          
          <div class="stats">
            <div v-for="stat in stats" :key="stat.label" class="stat">
              <span class="stat-value">{{ stat.value }}</span>
              <span class="stat-label">{{ stat.label }}</span>
            </div>
          </div>
        </div>

        <div class="right">
          <div 
            v-for="feature in features" 
            :key="feature.num"
            class="feature-item"
          >
            <span class="feature-num">{{ feature.num }}</span>
            <div class="feature-content">
              <h3>{{ feature.title }}</h3>
              <p>{{ feature.desc }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.split {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: start;
}

.stats {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;
  margin-top: 3rem;
  padding-top: 2rem;
  border-top: 1px solid var(--stone-200);
}

.stat-value {
  font-family: var(--font-serif);
  font-size: 2rem;
  display: block;
}

.stat-label {
  font-size: 0.8125rem;
  color: var(--stone-400);
  margin-top: 0.25rem;
  display: block;
}

.feature-item {
  display: flex;
  gap: 1.5rem;
  padding: 1.5rem 0;
  border-bottom: 1px solid var(--stone-100);
  cursor: default;
  transition: all 0.3s;
}

.feature-item:hover {
  padding-left: 1rem;
}

.feature-num {
  font-family: var(--font-serif);
  font-size: 0.875rem;
  color: var(--stone-300);
  flex-shrink: 0;
  padding-top: 0.25rem;
}

.feature-content h3 {
  font-size: 1.25rem;
  margin-bottom: 0.375rem;
}

.feature-content p {
  font-size: 0.875rem;
  color: var(--stone-500);
  line-height: 1.6;
}

@media (max-width: 768px) {
  .split {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
}
</style>
