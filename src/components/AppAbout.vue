<template>
  <section id="courses" class="section courses-section">
    <div class="container">
      <h2>敦安简介</h2>
      <p class="section-subtitle">
        致力于中医文化传播与传承，通过创新的教育模式和丰富的内容资源，让更多人了解和学习中医智慧。致力于中医文化传播与
        传承，通过创新的教育模式和丰富的内容资源，让更多人了解和学习中医智慧。致力于中医文化传播与传承，通过创新的教育
        模式和丰富的内容资源，让更多人了解和学习中医智慧。致力于中医文化传播与传承，通过创新的教育模式和丰富的内容资源，
        让更多人了解和学习中医智慧。
      </p>

      <!-- 上方向右滚动展示条 -->
      <div
        class="scroll-banner scroll-left-to-right"
        :class="{ paused: topHoveredCardId !== null }"
      >
        <div class="scroll-content">
          <div
            v-for="item in topBannerItemsLooped"
            :key="`${item.id}-${item.year}`"
            class="banner-card"
            @mouseenter="topHoveredCardId = item.id"
            @mouseleave="topHoveredCardId = null"
            @click="openLink(item)"
            :class="{ active: topHoveredCardId === item.id }"
          >
            <div class="card-image" :style="{ backgroundImage: `url(${item.image})` }"></div>
            <div class="card-info">
              <div class="card-year">{{ item.year }}</div>
              <div class="card-title">{{ item.title }}</div>
              <div class="card-description">{{ item.description }}</div>
            </div>
            <div class="card-title-bar">{{ item.title }}</div>
          </div>
        </div>
      </div>

      <!-- 下方向左滚动展示条 -->
      <div
        class="scroll-banner scroll-right-to-left"
        :class="{ paused: bottomHoveredCardId !== null }"
      >
        <div class="scroll-content">
          <div
            v-for="item in bottomBannerItemsLooped"
            :key="`${item.id}-${item.year}`"
            class="banner-card"
            @mouseenter="bottomHoveredCardId = item.id"
            @mouseleave="bottomHoveredCardId = null"
            @click="openLink(item)"
            :class="{ active: bottomHoveredCardId === item.id }"
          >
            <div class="card-image" :style="{ backgroundImage: `url(${item.image})` }"></div>
            <div class="card-info">
              <div class="card-year">{{ item.year }}</div>
              <div class="card-title">{{ item.title }}</div>
              <div class="card-description">{{ item.description }}</div>
            </div>
            <div class="card-title-bar">{{ item.title }}</div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

// 导入本地图片
const showCard1 = new URL('@/assets/ShowCard-1.png', import.meta.url).href
const showCard2 = new URL('@/assets/ShowCard-2.png', import.meta.url).href
const showCard3 = new URL('@/assets/ShowCard-3.png', import.meta.url).href
const showCard4 = new URL('@/assets/ShowCard-4.png', import.meta.url).href

const topHoveredCardId = ref<number | null>(null)
const bottomHoveredCardId = ref<number | null>(null)

const allBannerItems = [
  {
    id: 1,
    title: '佛香珠文化',
    description: '既是腕间雅饰，亦是守护身心的天然瑰宝。',
    bgColor: '#4F201C',
    image: showCard1,
    url: 'https://mp.weixin.qq.com/s/WjNU7opd1FBeOyasLpoYGA',
  },
  {
    id: 2,
    year: '2012',
    title: '首个项目启动',
    description: '推出第一个中医教育系列课程，获得广泛认可。',
    bgColor: '#692A27',
    image: showCard2,
  },
  {
    id: 3,
    year: '2015',
    title: '平台扩展',
    description: '扩展至多个社交媒体平台，覆盖用户超百万。',
    bgColor: '#6A412A',
    image: showCard3,
    url: 'https://example.com',
  },
  {
    id: 4,
    year: '2018',
    title: '国际合作',
    description: '与国际中医机构建立合作关系，传播中医智慧。',
    bgColor: '#D6651D',
    image: showCard4,
    url: 'https://example.com',
  },
  {
    id: 5,
    year: '2020',
    title: '数字转型',
    description: '完成数字化转型，推出在线学习平台。',
    bgColor: '#ea580c',
    image: showCard4,
    url: 'https://example.com',
  },
  {
    id: 6,
    year: '2022',
    title: '品牌升级',
    description: '品牌全新升级，推出企业文化新形象。',
    bgColor: '#0284c7',
    image: showCard3,
    url: 'https://example.com',
  },
  {
    id: 7,
    year: '2023',
    title: '用户突破',
    description: '累计用户突破千万，成为行业领先平台。',
    bgColor: '#16a34a',
    image: showCard2,
    url: 'https://example.com',
  },
  {
    id: 8,
    year: '2024',
    title: '新纪元开启',
    description: '继续创新发展，为中医传承贡献力量。',
    bgColor: '#9333ea',
    image: showCard1,
    url: 'https://example.com',
  },
]

// 上方显示前4张，下方显示后4张
const topBannerItems = ref(allBannerItems.slice(0, 4))
const bottomBannerItems = ref(allBannerItems.slice(4, 8))

// 用于模板中的循环，复制数据使首尾相接
const topBannerItemsLooped = computed(() => [...topBannerItems.value, ...topBannerItems.value])
const bottomBannerItemsLooped = computed(() => [
  ...bottomBannerItems.value,
  ...bottomBannerItems.value,
])

// 点击卡片打开外链
interface BannerItem {
  id: number
  year?: string
  title: string
  description: string
  bgColor: string
  image: string
  url?: string
}

const openLink = (item: BannerItem) => {
  if (item.url) {
    window.open(item.url, '_blank')
  }
}
</script>

<style scoped>
/* Courses 样式在全局 styles.css 中 */
</style>
