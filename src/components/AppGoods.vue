<template>
  <section id="forum" class="section products-section">
    <div class="container">
      <h2>祝吉用品</h2>
      <p class="section-subtitle">精选天然灵物，承载东方文化智慧，助力您的修行与提升</p>

      <div class="carousel-wrapper">
        <div class="news-carousel" ref="carouselContainer">
          <div class="goods-carousel-content" :style="carouselContentStyle">
            <div
              v-for="(item, index) in allCards"
              :key="index"
              class="news-card"
              :style="{
                backgroundImage: `url('${item.image}')`,
                width: `${cardWidth}px`,
              }"
            >
              <h3 class="news-title">{{ item.title }}</h3>
            </div>
          </div>
        </div>
      </div>

      <div class="carousel-controls">
        <div class="pagination-dots">
          <span
            v-for="i in totalPages"
            :key="i"
            :class="['dot', { active: i === currentPage }]"
            @click="goToPage(i)"
          ></span>
          <span class="page-number">{{ currentPage }} / {{ totalPages }}</span>
        </div>
        <div class="carousel-buttons">
          <button class="carousel-btn prev" @click="prevPage" :disabled="currentPage === 1">
            ←
          </button>
          <button
            class="carousel-btn next"
            @click="nextPage"
            :disabled="currentPage === totalPages"
          >
            →
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

const currentPage = ref(1)
const carouselContainer = ref<HTMLElement | null>(null)
const containerLeftOffset = ref(0)
const windowWidth = ref(typeof window !== 'undefined' ? window.innerWidth : 0)

const productImage1 = new URL('@/assets/goods_1.png', import.meta.url).href
const productImage2 = new URL('@/assets/goods_2.png', import.meta.url).href
const productImage3 = new URL('@/assets/goods_3.png', import.meta.url).href
const productImage4 = new URL('@/assets/goods_4.png', import.meta.url).href
const productImage5 = new URL('@/assets/goods_5.png', import.meta.url).href
const productImage6 = new URL('@/assets/goods_6.png', import.meta.url).href
const productImage10 = new URL('@/assets/goods_10.png', import.meta.url).href
const productImage11 = new URL('@/assets/goods_11.png', import.meta.url).href
const productImage12 = new URL('@/assets/goods_12.png', import.meta.url).href
const productImage13 = new URL('@/assets/goods_13.png', import.meta.url).href

const allCards = ref([
  { title: '曲直款', image: productImage1 },
  { title: '炎上款', image: productImage2 },
  { title: '稼穑款', image: productImage3 },
  { title: '从革款', image: productImage4 },
  { title: '润下款', image: productImage5 },
  { title: '通行款', image: productImage6 },
  { title: '定制五行款', image: productImage10 },
  { title: '合香莲花牌', image: productImage11 },
  { title: '合香龙珠', image: productImage12 },
  { title: '合香葫芦', image: productImage13 },
])

// 卡片宽度 + 间隙
const cardWidth = ref(380)
const cardGap = ref(30)
const cardUnit = computed(() => cardWidth.value + cardGap.value)

// 计算容器左侧偏移量，使第一个卡片对齐内容区域
const updateLayout = () => {
  windowWidth.value = window.innerWidth
  const w = windowWidth.value
  const containerMaxWidth = 1200

  // 响应式调整基础内边距
  const basePadding = w < 768 ? 20 : 40

  // 响应式调整卡片尺寸
  if (w < 768) {
    // 移动端：卡片宽度占满屏幕减去一些边距
    // 保持左右各20px的间距 (basePadding)
    cardWidth.value = w - basePadding * 2
    cardGap.value = 15
  } else {
    // 桌面端：固定宽度
    cardWidth.value = 380
    cardGap.value = 30
  }

  if (w > containerMaxWidth) {
    containerLeftOffset.value = (w - containerMaxWidth) / 2 + basePadding
  } else {
    containerLeftOffset.value = basePadding
  }
}

// 计算每页能显示的卡片数
const cardsPerPage = computed(() => {
  if (!carouselContainer.value) return 3
  // 使用窗口宽度计算，保持一致性
  const count = Math.floor(windowWidth.value / cardUnit.value)
  return Math.max(count, 1)
})

// 总页数：根据每页能显示的卡片数动态计算
const totalPages = computed(() => {
  const cardsCount = allCards.value.length
  const perPage = cardsPerPage.value
  return Math.ceil(cardsCount / perPage)
})

// 计算平移距离
const translateDistance = computed(() => {
  // 如果是最后一页，且总页数大于1，则强制右对齐
  if (currentPage.value === totalPages.value && totalPages.value > 1) {
    const totalCards = allCards.value.length
    const paddingLeft = containerLeftOffset.value
    const w = windowWidth.value

    // 目标右边界：容器内容的右边缘
    const targetRight = w - paddingLeft

    // 当前内容右边缘（无偏移时）：左内边距 + 所有卡片宽度(含间隙) - 最后一个间隙
    const currentRight = paddingLeft + totalCards * cardUnit.value - cardGap.value

    return targetRight - currentRight
  }

  const perPage = cardsPerPage.value
  const startIndex = (currentPage.value - 1) * perPage
  return -(startIndex * cardUnit.value)
})

// 计算内容容器的 transform 和 padding
const carouselContentStyle = computed(() => {
  // 移动端禁用 transform，使用原生滚动
  if (windowWidth.value < 768) {
    return {
      transform: 'none',
      paddingLeft: `${containerLeftOffset.value}px`,
      paddingRight: `${containerLeftOffset.value}px`, // 右侧也添加内边距保持平衡
      gap: `${cardGap.value}px`,
    }
  }

  return {
    transform: `translateX(${translateDistance.value}px)`,
    transition: 'transform 0.3s ease-in-out',
    paddingLeft: `${containerLeftOffset.value}px`, // 动态设置左内边距
    gap: `${cardGap.value}px`, // 动态设置间隙
  }
})

const goToPage = (page: number) => {
  currentPage.value = page
}

// 禁用容器上的滚轮事件（仅桌面端）
const handleWheel = (event: WheelEvent) => {
  if (windowWidth.value < 768) return // 移动端允许原生滚动
  if (event.shiftKey || event.deltaX !== 0) {
    event.preventDefault()
  }
}

onMounted(() => {
  updateLayout()
  window.addEventListener('resize', updateLayout)
  if (carouselContainer.value) {
    carouselContainer.value.addEventListener('wheel', handleWheel, { passive: false })
  }
})

onUnmounted(() => {
  window.removeEventListener('resize', updateLayout)
  if (carouselContainer.value) {
    carouselContainer.value.removeEventListener('wheel', handleWheel)
  }
})

const prevPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--
  }
}

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++
  }
}
</script>

<style scoped>
/* Products 样式在全局 styles.css 中 */
</style>
