<template>
  <div class="page-wrapper">
    <Header />
    <main>
      <Carousel />
      <About />
      <Services />
      <Goods />
      <Contact />
    </main>
    <Footer />
  </div>
</template>

<script setup lang="ts">
import Header from './components/AppHeader.vue'
import Carousel from './components/AppCarousel.vue'
import About from './components/AppAbout.vue'
import Services from './components/AppServices.vue'
import Goods from './components/AppGoods.vue'
import Contact from './components/AppContact.vue'
import Footer from './components/AppFooter.vue'
</script>

<style>
@import './assets/styles.css';
</style>
