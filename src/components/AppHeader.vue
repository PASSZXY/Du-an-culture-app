<template>
  <header class="main-header" :class="{ scrolled: isScrolled }">
    <div class="container">
      <div class="logo">
        <img src="@/assets/logo.png" alt="敦安文化" class="logo-img" />
        <div class="logo-text">敦安文化 <span>Dunan Culture</span></div>
      </div>
      <!-- 桌面端导航 -->
      <nav class="main-nav desktop-nav">
        <ul>
          <li>
            <a
              href="#home"
              @click="handleNavClick('home', $event)"
              :class="{ active: activeSection === 'home' }"
              >首页</a
            >
          </li>
          <li class="nav-divider">•</li>
          <li>
            <a
              href="#courses"
              @click="handleNavClick('courses', $event)"
              :class="{ active: activeSection === 'courses' }"
              >敦安简介</a
            >
          </li>
          <li class="nav-divider">•</li>
          <li>
            <a
              href="#clinics"
              @click="handleNavClick('clinics', $event)"
              :class="{ active: activeSection === 'clinics' }"
              >敦安服务</a
            >
          </li>
          <li class="nav-divider">•</li>
          <li>
            <a
              href="#forum"
              @click="handleNavClick('forum', $event)"
              :class="{ active: activeSection === 'forum' }"
              >祝吉用品
            </a>
          </li>
          <li class="nav-divider">•</li>
          <li>
            <a
              href="#contact"
              @click="handleNavClick('contact', $event)"
              :class="{ active: activeSection === 'contact' }"
              >联系我们</a
            >
          </li>
        </ul>
      </nav>
      <!-- 汉堡菜单按钮 -->
      <button
        class="mobile-menu-btn"
        :class="{ active: isMobileMenuOpen }"
        @click="toggleMobileMenu"
      >
        <span></span>
        <span></span>
        <span></span>
      </button>
    </div>

    <!-- 移动端侧边栏菜单 -->
    <div class="mobile-sidebar" :class="{ active: isMobileMenuOpen }">
      <button class="sidebar-close" @click="closeMobileMenu">
        <svg
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
        >
          <line x1="18" y1="6" x2="6" y2="18"></line>
          <line x1="6" y1="6" x2="18" y2="18"></line>
        </svg>
      </button>
      <nav class="sidebar-nav">
        <ul>
          <li><a href="#home" @click="handleNavClick('home', $event)">首页</a></li>
          <li><a href="#courses" @click="handleNavClick('courses', $event)">敦安简介</a></li>
          <li><a href="#clinics" @click="handleNavClick('clinics', $event)">敦安服务</a></li>
          <li><a href="#forum" @click="handleNavClick('forum', $event)">学习论坛</a></li>
          <li><a href="#contact" @click="handleNavClick('contact', $event)">联系我们</a></li>
        </ul>
      </nav>
    </div>

    <!-- 侧边栏遮罩 -->
    <div v-if="isMobileMenuOpen" class="sidebar-overlay" @click="closeMobileMenu"></div>
  </header>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const activeSection = ref('home')
const isScrolled = ref(false)
const isMobileMenuOpen = ref(false)
let isNavigating = false // 标记是否正在导航

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
}

// 处理导航链接点击
const handleNavClick = (section: string, e: Event) => {
  e.preventDefault() // 阻止默认的锚点行为
  e.stopPropagation() // 阻止事件冒泡

  activeSection.value = section
  closeMobileMenu() // 关闭移动端菜单

  // 标记正在导航，800ms内不要让onScroll覆盖状态
  isNavigating = true
  setTimeout(() => {
    isNavigating = false
  }, 800)

  // 延迟一帧后再跳转，确保状态已更新
  requestAnimationFrame(() => {
    const element = document.getElementById(section)
    if (element) {
      element.scrollIntoView({ behavior: 'smooth' })
    }
  })
} // 监听滚动事件更新活跃导航和导航栏背景
const onScroll = () => {
  // 检查是否在首页（id="home"）
  const homeSection = document.getElementById('home')
  if (homeSection) {
    const rect = homeSection.getBoundingClientRect()
    // 如果首页还可见（还在视口范围内）
    const isInHome = rect.top < window.innerHeight && rect.bottom > 0

    // 只在首页时根据滚动条判断背景色
    if (isInHome) {
      isScrolled.value = window.scrollY > 50 // 滚动50px以上时显示背景
    } else {
      isScrolled.value = true // 完全离开首页后始终显示白色背景
    }
  }

  // 如果正在导航中，不要更新活跃导航状态
  if (isNavigating) {
    return
  }

  // 更新活跃导航 - 找到当前视口中最接近顶部的部分
  const sections = ['home', 'courses', 'clinics', 'forum', 'contact']
  let closestSection = 'home'
  let closestDistance = Infinity

  sections.forEach((section) => {
    const element = document.getElementById(section)
    if (element) {
      const rect = element.getBoundingClientRect()

      // 只考虑顶部在视口内（rect.top <= 60）且不超过60px的元素
      // 选择距离60px位置最接近的（可以是负数，表示还没到）
      if (rect.top <= 60 && rect.bottom > 0) {
        const distance = rect.top - 60
        // 只更新如果这是最接近的
        if (Math.abs(distance) < Math.abs(closestDistance)) {
          closestDistance = distance
          closestSection = section
        }
      }
    }
  })

  activeSection.value = closestSection
}

onMounted(() => {
  window.addEventListener('scroll', onScroll)
})
onUnmounted(() => window.removeEventListener('scroll', onScroll))
</script>

<style scoped>
/* Header 样式在全局 styles.css 中 */
</style>
