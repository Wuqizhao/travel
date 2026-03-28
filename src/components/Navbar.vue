<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const isScrolled = ref(false)
const isMenuOpen = ref(false)

const navItems = [
  { id: 'attractions', label: '景点' },
  { id: 'features', label: '特色' },
  { id: 'counties', label: '县区' },
  { id: 'gallery', label: '图集' },
  { id: 'contact', label: '联系' }
]

const handleScroll = () => {
  isScrolled.value = window.scrollY > 80
}

const scrollTo = (id: string) => {
  document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' })
  isMenuOpen.value = false
}

onMounted(() => window.addEventListener('scroll', handleScroll))
onUnmounted(() => window.removeEventListener('scroll', handleScroll))
</script>

<template>
  <header :class="['header', { scrolled: isScrolled }]">
    <div class="container header-inner">
      <a href="#" class="logo" @click.prevent="scrollTo('home')">
        <span class="logo-cn">广西</span>
        <span class="logo-en">GUANGXI</span>
      </a>

      <nav class="nav" :class="{ open: isMenuOpen }">
        <a 
          v-for="item in navItems" 
          :key="item.id"
          :href="`#${item.id}`"
          class="nav-link"
          @click.prevent="scrollTo(item.id)"
        >
          {{ item.label }}
        </a>
      </nav>

      <button class="menu-btn" @click="isMenuOpen = !isMenuOpen">
        <span :class="{ open: isMenuOpen }"></span>
      </button>
    </div>
  </header>
</template>

<style scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  transition: all 0.5s var(--ease);
}

.header.scrolled {
  background: rgba(255, 255, 255, 0.92);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid var(--stone-100);
}

.header-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 80px;
}

.logo {
  display: flex;
  flex-direction: column;
  gap: 0;
}

.logo-cn {
  font-family: var(--font-serif);
  font-size: 1.5rem;
  font-weight: 600;
  letter-spacing: 0.1em;
  color: var(--white);
  transition: color 0.5s;
}

.header.scrolled .logo-cn {
  color: var(--black);
}

.logo-en {
  font-size: 0.625rem;
  letter-spacing: 0.3em;
  color: rgba(255, 255, 255, 0.5);
  transition: color 0.5s;
}

.header.scrolled .logo-en {
  color: var(--stone-400);
}

.nav {
  display: flex;
  gap: 2.5rem;
}

.nav-link {
  font-size: 0.8125rem;
  font-weight: 400;
  letter-spacing: 0.05em;
  color: rgba(255, 255, 255, 0.7);
  transition: color 0.3s;
  position: relative;
}

.header.scrolled .nav-link {
  color: var(--stone-500);
}

.nav-link:hover {
  color: var(--white);
}

.header.scrolled .nav-link:hover {
  color: var(--black);
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 0;
  height: 1px;
  background: currentColor;
  transition: width 0.3s;
}

.nav-link:hover::after {
  width: 100%;
}

.menu-btn {
  display: none;
  width: 24px;
  height: 24px;
  position: relative;
}

.menu-btn span,
.menu-btn span::before,
.menu-btn span::after {
  display: block;
  position: absolute;
  width: 100%;
  height: 1px;
  background: var(--white);
  transition: all 0.3s;
}

.header.scrolled .menu-btn span,
.header.scrolled .menu-btn span::before,
.header.scrolled .menu-btn span::after {
  background: var(--black);
}

.menu-btn span {
  top: 50%;
  transform: translateY(-50%);
}

.menu-btn span::before {
  content: '';
  top: -6px;
}

.menu-btn span::after {
  content: '';
  bottom: -6px;
}

.menu-btn span.open {
  background: transparent;
}

.menu-btn span.open::before {
  top: 0;
  transform: rotate(45deg);
}

.menu-btn span.open::after {
  bottom: 0;
  transform: rotate(-45deg);
}

@media (max-width: 768px) {
  .menu-btn {
    display: block;
  }
  
  .nav {
    position: fixed;
    inset: 0;
    background: var(--black);
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 2rem;
    opacity: 0;
    visibility: hidden;
    transition: all 0.4s;
  }
  
  .nav.open {
    opacity: 1;
    visibility: visible;
  }
  
  .nav-link {
    font-size: 1.5rem;
    color: var(--stone-400);
  }
  
  .nav-link:hover {
    color: var(--white);
  }
}
</style>
