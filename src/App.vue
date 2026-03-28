<script setup lang="ts">
import { ref } from 'vue'
import Navbar from './components/Navbar.vue'
import Hero from './components/Hero.vue'
import Attractions from './components/Attractions.vue'
import Features from './components/Features.vue'
import Honeycomb from './components/Honeycomb.vue'
import Gallery from './components/Gallery.vue'
import Footer from './components/Footer.vue'
import SpotModal from './components/SpotModal.vue'

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

const selectedSpot = ref<Spot | null>(null)
const isModalOpen = ref(false)

const openModal = (spot: Spot) => {
  selectedSpot.value = spot
  isModalOpen.value = true
  document.body.style.overflow = 'hidden'
}

const closeModal = () => {
  isModalOpen.value = false
  document.body.style.overflow = ''
}
</script>

<template>
  <div class="app">
    <Navbar />
    <Hero />
    <Attractions @select-spot="openModal" />
    <Features />
    <Honeycomb />
    <Gallery />
    <Footer />
    <SpotModal 
      v-if="isModalOpen" 
      :spot="selectedSpot" 
      @close="closeModal" 
    />
  </div>
</template>

<style scoped>
.app {
  min-height: 100vh;
}
</style>
