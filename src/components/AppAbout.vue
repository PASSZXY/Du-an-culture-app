<template>
  <section id="courses" class="section courses-section">
    <div class="container">
      <h2>敦安简介</h2>
      <p class="section-subtitle">
        传承中华优秀传统文化，涵盖玄学咨询、中医理疗、香道文创。
        以科学视角审视传统，客观理性，取精去糟，摒弃迷信偏执，让传统智慧真正赋能当代生活。
      </p>

      <!-- 上方向右滚动展示条 -->
      <div
        class="scroll-banner scroll-left-to-right"
        :class="{ paused: topHoveredCardId !== null }"
      >
        <div class="scroll-content">
          <div
            v-for="item in topBannerItemsLooped"
            :key="item.id"
            class="banner-card"
            @mouseenter="topHoveredCardId = item.id"
            @mouseleave="topHoveredCardId = null"
            @click="openLink(item)"
            :class="{ active: topHoveredCardId === item.id }"
          >
            <div class="card-image" :style="{ backgroundImage: `url(${item.image})` }"></div>
            <div class="card-info">
              <!-- <div class="card-year">{{ item.year }}</div> -->
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
            :key="item.id"
            class="banner-card"
            @mouseenter="bottomHoveredCardId = item.id"
            @mouseleave="bottomHoveredCardId = null"
            @click="openLink(item)"
            :class="{ active: bottomHoveredCardId === item.id }"
          >
            <div class="card-image" :style="{ backgroundImage: `url(${item.image})` }"></div>
            <div class="card-info">
              <!-- <div class="card-year">{{ item.year }}</div> -->
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
const showCard5 = new URL('@/assets/ShowCard-5.png', import.meta.url).href
const showCard6 = new URL('@/assets/ShowCard-6.png', import.meta.url).href
const showCard7 = new URL('@/assets/ShowCard-7.png', import.meta.url).href
const showCard8 = new URL('@/assets/ShowCard-8.png', import.meta.url).href

const topHoveredCardId = ref<number | null>(null)
const bottomHoveredCardId = ref<number | null>(null)

const allBannerItems = [
  {
    id: 1,
    title: '敦安文化创始人 - 孙镇',
    description: '自天佑之，吉无不利',
    bgColor: '#4F201C',
    image: showCard1,
  },
  {
    id: 2,
    title: '沈阳市周易研究会风水研修',
    description:
      '沈阳市周易研究会是中国唯一获评国家5A级资质的周易专业机构。本研修项目由其权威认证并全程指导，依托协会深厚的学术底蕴与专家资源，提供体系完整、理实结合的正统风水研习课程，为专业人才成长提供官方认可的学术平台与执业背书。',
    bgColor: '#692A27',
    image: showCard2,
  },
  {
    id: 3,
    title: '师承源流',
    description:
      '师承，远不止于技艺传授，更是连接往圣先贤智慧与当代实践的文化纽带。它是确保易学精髓一脉相承、严谨有序的千年基石。',
    bgColor: '#6A412A',
    image: showCard3,
  },
  {
    id: 4,
    title: '医易同源',
    description: '中医与周易本属同源，理用兼顾',
    bgColor: '#D6651D',
    image: showCard4,
  },
  {
    id: 5,
    title: '风水布局',
    description:
      '风水布局是中国传统智慧的核心实践，旨在通过对家居或办公环境中物品、方位与气流的合理安排，调和空间能量（气），促进居住者的健康、和谐与运势，实现人与环境的深度平衡。',
    image: showCard5,
  },
  {
    id: 6,
    title: '纯手工非遗',
    description:
      '每一件手作物都凝结着时光与心意。从精挑细选的天然材质，到匠人指尖的温度与专注，我们创造的不仅是一件用品，更是可触摸的情感记忆与独一无二的生活美学。让“拾光手造”的作品，成为您生活中那份特别的陪伴与珍藏。',
    bgColor: '#0284c7',
    image: showCard6,
  },
  {
    id: 7,
    title: '行业认同',
    description:
      '伏羲，肇画八卦，一画开天，奠中华文明之基；道家思想，渊源于此，倡法天贵真、自然无为，成东方智慧之巅。本机构（或公司）立于这千年道统之源，以“承羲皇道脉，弘黄老真章”为己任，是一家致力于伏羲文化与道家哲学体系研究、传承、创新与应用的综合性文化品牌。',
    bgColor: '#16a34a',
    image: showCard7,
  },
  {
    id: 8,
    title: '文化探源',
    description:
      '伏羲庙是中华文明始源的精神象征，专为祭祀人文始祖伏羲而建。它以天水伏羲庙为典型，作为现存最完整的祭祀建筑群，既是历代国家祀典的庄严圣地，也是全球华人寻根谒祖、凝聚文化认同的核心殿堂。',
    bgColor: '#9333ea',
    image: showCard8,
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
  bgColor?: string
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
