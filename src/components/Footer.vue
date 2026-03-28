<script setup lang="ts">
const links = [
  { name: '首页', href: '#home' },
  { name: '景点', href: '#attractions' },
  { name: '特色', href: '#features' },
  { name: '图集', href: '#gallery' }
]

const destinations = [
  '桂林漓江', '阳朔西街', '龙脊梯田', '德天瀑布', '北海银滩', '黄姚古镇'
]
</script>

<template>
  <footer id="contact" class="footer">
    <div class="container">
      <div class="footer-top">
        <div class="footer-brand">
          <h3 class="footer-title">探索广西</h3>
          <p class="footer-desc">
            发现广西山水之美<br/>
            体验壮族风情
          </p>
          <div class="footer-contact">
            <p>广西壮族自治区南宁市</p>
            <p>travel@guangxi.com</p>
            <p>+86 771 123 4567</p>
          </div>
        </div>

        <div class="footer-col">
          <h4>导航</h4>
          <ul>
            <li v-for="link in links" :key="link.name">
              <a :href="link.href">{{ link.name }}</a>
            </li>
          </ul>
        </div>

        <div class="footer-col">
          <h4>目的地</h4>
          <ul>
            <li v-for="dest in destinations" :key="dest">
              <a href="#attractions">{{ dest }}</a>
            </li>
          </ul>
        </div>

        <div class="footer-col">
          <h4>订阅</h4>
          <p class="subscribe-text">获取最新旅行资讯</p>
          <form class="subscribe-form" @submit.prevent>
            <input type="email" placeholder="邮箱地址" />
            <button type="submit">
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <path d="M5 12h14M12 5l7 7-7 7"/>
              </svg>
            </button>
          </form>
        </div>
      </div>

      <div class="footer-bottom">
        <p>© 2026 探索广西</p>
        <div class="footer-links">
          <a href="#">隐私</a>
          <a href="#">条款</a>
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  background: var(--stone-100);
  padding: 4rem 0 2rem;
}

.footer-top {
  display: grid;
  grid-template-columns: 1.5fr 1fr 1fr 1.5fr;
  gap: 3rem;
  padding-bottom: 3rem;
  border-bottom: 1px solid var(--stone-200);
}

.footer-title {
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.footer-desc {
  font-size: 0.875rem;
  color: var(--stone-500);
  line-height: 1.7;
  margin-bottom: 1.5rem;
}

.footer-contact p {
  font-size: 0.8125rem;
  color: var(--stone-400);
  margin-bottom: 0.375rem;
}

.footer-col h4 {
  font-family: var(--font-sans);
  font-size: 0.75rem;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--stone-400);
  margin-bottom: 1rem;
}

.footer-col ul li {
  margin-bottom: 0.625rem;
}

.footer-col a {
  font-size: 0.875rem;
  color: var(--stone-600);
  transition: color 0.2s;
}

.footer-col a:hover {
  color: var(--black);
}

.subscribe-text {
  font-size: 0.875rem;
  color: var(--stone-500);
  margin-bottom: 1rem;
}

.subscribe-form {
  display: flex;
  gap: 0;
}

.subscribe-form input {
  flex: 1;
  padding: 0.75rem 1rem;
  background: var(--white);
  border: 1px solid var(--stone-200);
  border-right: none;
  font-size: 0.8125rem;
}

.subscribe-form input:focus {
  outline: none;
  border-color: var(--stone-400);
}

.subscribe-form button {
  padding: 0.75rem 1rem;
  background: var(--black);
  color: var(--white);
  transition: background 0.3s;
}

.subscribe-form button:hover {
  background: var(--stone-800);
}

.footer-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 2rem;
}

.footer-bottom p {
  font-size: 0.8125rem;
  color: var(--stone-400);
}

.footer-links {
  display: flex;
  gap: 1.5rem;
}

.footer-links a {
  font-size: 0.8125rem;
  color: var(--stone-400);
}

.footer-links a:hover {
  color: var(--black);
}

@media (max-width: 1024px) {
  .footer-top {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 640px) {
  .footer-top {
    grid-template-columns: 1fr;
  }
  
  .footer-bottom {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }
}
</style>
