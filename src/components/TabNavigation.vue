<template>
  <div class="tab-nav-wrapper">
    <nav ref="navElement" class="tab-nav">
      <button
        v-for="(tab, index) in tabs"
        :key="index"
        class="tab-btn"
        :class="{ selected: modelValue === index }"
        @click="handleTabClick(index)"
      >
        {{ tab }}
      </button>
      <!-- 背景滑块 -->
      <div class="tab-bgc" ref="bgcElement"></div>
    </nav>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, watch, onUnmounted } from 'vue'

interface Props {
  tabs: string[]
  modelValue: number
  autoPlay?: boolean
  interval?: number
}

interface Emits {
  (e: 'update:modelValue', value: number): void
}

const props = withDefaults(defineProps<Props>(), {
  autoPlay: true,
  interval: 5000,
})
const emit = defineEmits<Emits>()

const navElement = ref<HTMLElement>()
const bgcElement = ref<HTMLElement>()
const windowWidth = ref(typeof window !== 'undefined' ? window.innerWidth : 1200)
let autoPlayTimer: ReturnType<typeof setInterval> | null = null
let holdTimer: ReturnType<typeof setTimeout> | null = null

// 根据屏幕宽度计算尺寸 - 更平滑的缩放
const getTabDimensions = () => {
  const width = windowWidth.value

  if (width <= 480) {
    return { minWidth: 60, height: 40, padding: '8px 12px' }
  } else if (width <= 768) {
    return { minWidth: 100, height: 40, padding: '8px 16px' }
  } else {
    return { minWidth: 140, height: 60, padding: '12px 20px' }
  }
}

const dimensions = computed(() => getTabDimensions())

// 更新背景块宽度和位置 - 获取实际的按钮尺寸
const updateSliderPosition = (index: number) => {
  if (bgcElement.value && navElement.value) {
    const buttons = navElement.value.querySelectorAll('.tab-btn')
    if (buttons[index]) {
      const btn = buttons[index] as HTMLElement
      const rect = btn.getBoundingClientRect()
      const navRect = navElement.value.getBoundingClientRect()

      // 计算按钮相对于 nav 的位置
      const relativeLeft = rect.left - navRect.left
      const width = rect.width

      bgcElement.value.style.left = relativeLeft + 'px'
      bgcElement.value.style.width = width + 'px'
    }
  }
}

// 自动轮播逻辑
const startAutoPlay = () => {
  if (!props.autoPlay) return

  autoPlayTimer = setInterval(() => {
    const nextIndex = (props.modelValue + 1) % props.tabs.length
    emit('update:modelValue', nextIndex)
  }, props.interval)
}

const stopAutoPlay = () => {
  if (autoPlayTimer) {
    clearInterval(autoPlayTimer)
    autoPlayTimer = null
  }
}

// 点击标签时的处理
const handleTabClick = (index: number) => {
  emit('update:modelValue', index)
  // 停止自动轮播
  stopAutoPlay()

  // 清除之前的停留计时器
  if (holdTimer) {
    clearTimeout(holdTimer)
  }

  // 设置 10 秒停留时间，然后恢复自动轮播
  if (props.autoPlay) {
    holdTimer = setTimeout(() => {
      startAutoPlay()
      holdTimer = null
    }, 10000) // 10秒
  }
}

// 监听窗口大小变化
onMounted(() => {
  const handleResize = () => {
    windowWidth.value = window.innerWidth
  }
  window.addEventListener('resize', handleResize)
  updateSliderPosition(props.modelValue)

  // 启动自动轮播
  if (props.autoPlay) {
    startAutoPlay()
  }

  return () => {
    window.removeEventListener('resize', handleResize)
  }
})

onUnmounted(() => {
  stopAutoPlay()
  if (holdTimer) {
    clearTimeout(holdTimer)
  }
})

// 监听 modelValue 变化并更新背景块位置
watch(
  () => props.modelValue,
  (newValue) => {
    updateSliderPosition(newValue)
  },
)

// 当尺寸改变时也更新位置
watch(dimensions, () => {
  updateSliderPosition(props.modelValue)
})
</script>
<style scoped>
.tab-nav-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 40px;
}

.tab-nav {
  position: relative;
  background-color: #ffffff;
  border-radius: 50px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  display: inline-flex;
  padding: 0;
  gap: 0;
  overflow: hidden;
}

.tab-btn {
  position: relative;
  flex: 0 0 auto;
  min-width: 140px;
  height: 60px;
  padding: 12px 20px;
  text-align: center;
  line-height: 36px;
  font-size: 20px;
  color: #000;
  border: none;
  background: transparent;
  cursor: pointer;
  transition: color 0.3s;
  font-weight: 300;
  z-index: 1;
  margin: 0;
  white-space: nowrap;
  display: flex;
  align-items: center;
  justify-content: center;
}

.tab-btn:hover {
  color: var(--color-primary);
}

.tab-btn.selected {
  color: var(--text-white) !important;
}

.tab-bgc {
  position: absolute;
  top: 0;
  left: 0;
  height: 60px;
  width: 0;
  border-radius: 50px;
  background-color: var(--color-primary);
  transition: all 0.3s;
  z-index: 0;
}

/* 平板响应式 768px */
@media (max-width: 768px) {
  .tab-btn {
    min-width: 100px;
    height: 40px;
    padding: 8px 16px;
    line-height: 24px;
    font-size: 14px;
  }

  .tab-bgc {
    height: 40px;
  }
}

/* 手机响应式 480px */
@media (max-width: 480px) {
  .tab-btn {
    min-width: 60px;
    height: 40px;
    padding: 8px 12px;
    line-height: 24px;
    font-size: 12px;
  }

  .tab-bgc {
    height: 40px;
  }
}
</style>
