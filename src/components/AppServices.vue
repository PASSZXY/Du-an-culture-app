<template>
  <section id="clinics" class="section clinics-section">
    <div class="container">
      <h2>敦安服务</h2>
      <!-- 使用 TabNavigation 组件 -->
      <TabNavigation v-model="activeTabIndex" :tabs="tabs" />

      <!-- 服务介绍卡片 -->
      <div class="reviews-container">
        <div class="reviews-card">
          <!-- 服务内容展示 -->
          <div v-if="currentReview" class="review-content">
            <!-- 服务图片 -->
            <Transition name="appear" mode="out-in">
              <div :key="currentReview.id" class="service-image">
                <img :src="currentReview.serviceImage" :alt="currentReview.name" />
              </div>
            </Transition>

            <!-- 服务信息 -->
            <div class="service-info">
              <div class="info-left">
                <div class="service-name">{{ currentReview.name }}</div>
                <div class="service-description">{{ currentReview.description }}</div>
              </div>

              <!-- 特色点 - 右侧竖排 -->
              <div class="features-container">
                <div
                  v-for="(feature, index) in currentReview.features"
                  :key="index"
                  class="feature-item"
                >
                  <div class="feature-icon">✓</div>
                  <div class="feature-content">
                    <div class="feature-title">{{ feature.title }}</div>
                    <div class="feature-description">{{ feature.description }}</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import TabNavigation from './TabNavigation.vue'
const showCard1 = new URL('@/assets/sever_xuanxueyuce.png', import.meta.url).href
const showCard2 = new URL('@/assets/sever_fengshubuju.png', import.meta.url).href
const showCard3 = new URL('@/assets/sever_yangshengtiaoli.png', import.meta.url).href
const showCard4 = new URL('@/assets/sever_xiangdaochanpin.png', import.meta.url).href

const activeTabIndex = ref(0)
const tabs = ['玄学预测', '风水布局', '养生调理', '香道产品']

const reviews = ref([
  {
    id: 1,
    category: '玄学预测',
    name: '玄学预测',
    title: '',
    description:
      '“通过专业的命理推演服务，为您解读人生格局，指点迷津。我们的师傅拥有多年实践经验，运用古人智慧为现代人服务。”',
    serviceImage: showCard1,
    features: [
      { title: '专业解读', description: '资深命理师团队，深入浅出' },
      { title: '准确预测', description: '基于古籍理论，科学严谨' },
      { title: '人生指导', description: '帮助规划未来发展方向' },
    ],
  },
  {
    id: 2,
    category: '风水布局',
    name: '风水布局',
    title: '',
    description:
      '精心打造居家和商务空间的风水布局，以科学的方法改善生活和工作环境。根据个人需求定制专属方案。',
    serviceImage: showCard2,
    features: [
      { title: '空间规划', description: '全方位评估环境特点' },
      { title: '能量调理', description: '应用风水原理优化格局' },
      { title: '长期效果', description: '持续改善生活质量' },
    ],
  },
  {
    id: 3,
    category: '养生调理',
    name: '养生调理',
    title: '',
    description:
      '结合中医理论和现代养生理念，为您制定个性化的健康养生方案。从调理体质到增进健康，全面守护您的身体。',
    serviceImage: showCard3,
    features: [
      { title: '体质评估', description: '科学诊断个人体质状况' },
      { title: '调理方案', description: '针对性制定养生计划' },
      { title: '健康咨询', description: '持续跟进和专业指导' },
    ],
  },
  {
    id: 4,
    category: '香道产品',
    name: '香道产品',
    title: '',
    description: '精选天然佛香珠，承载古老的东方文化与智慧。',
    serviceImage: showCard4,
    features: [
      { title: '精品选材', description: '甄选天然高质珠料' },
      { title: '文化传承', description: '融合传统手工工艺' },
      { title: '修行辅助', description: '助力身心平衡修养' },
    ],
  },
])

const currentReview = computed(
  () => reviews.value[Math.min(activeTabIndex.value, reviews.value.length - 1)],
)
</script>

<style scoped>
/* Clinics 样式在全局 styles.css 中 */
</style>
