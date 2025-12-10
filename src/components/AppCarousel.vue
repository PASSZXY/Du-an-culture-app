<template>
  <section id="home" class="carousel-section">
    <div class="carousel-slide">
      <div class="container">
        <div class="carousel-content">
          <h1>敦安文化</h1>
          <p>玄学不是答案，是探索世界的另一种视角。</p>
          <a href="#" class="cta-button">联系我们</a>
        </div>
      </div>
      <!-- 下滑提示箭头 -->
      <div class="scroll-hint" :class="{ hidden: isScrolling }" @click="scrollToNext">
        <p class="scroll-text">向下滑动</p>
        <svg
          class="scroll-arrow"
          width="40"
          height="40"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
        >
          <polyline points="6 9 12 15 18 9"></polyline>
        </svg>
      </div>
    </div>
    <div class="carousel-dots">
      <span
        v-for="(dot, index) in dots"
        :key="index"
        class="dot"
        :class="{ active: currentSlide === index }"
        @click="currentSlide = index"
      ></span>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const currentSlide = ref(0)
const dots = ref([1, 2, 3])
const isScrolling = ref(false)
let autoplayTimer: ReturnType<typeof setInterval> | null = null

const startAutoplay = () => {
  autoplayTimer = setInterval(() => {
    currentSlide.value = (currentSlide.value + 1) % dots.value.length
  }, 5000)
}

const stopAutoplay = () => {
  if (autoplayTimer !== null) {
    clearInterval(autoplayTimer)
    autoplayTimer = null
  }
}

const scrollToNext = () => {
  // 滚动到下一个组件（敦安文化传媒简介）
  const coursesSection = document.getElementById('courses')
  if (coursesSection) {
    coursesSection.scrollIntoView({ behavior: 'smooth' })
  }
}

const handleScroll = () => {
  // 当窗口向下滚动时，隐藏提示
  isScrolling.value = window.scrollY > 10
}

onMounted(() => {
  startAutoplay()
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  stopAutoplay()
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
/* Carousel 样式在全局 styles.css 中 */
</style>
