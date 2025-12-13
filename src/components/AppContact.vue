<template>
  <section id="contact" class="section contact-section">
    <div class="container">
      <h2>联系我们</h2>

      <!-- 左右布局：左边文字（0.5）+ 右边图片（1）-->
      <div class="contact-main-wrapper">
        <!-- 左侧：联系信息 -->
        <div class="contact-left">
          <div class="contact-info-card">
            <h3>如何联系</h3>

            <div class="info-item">
              <div class="info-content">
                <h4>地址</h4>
                <p>北京市延庆区妫水北街33号</p>
                <p>观澜国际公寓1217室</p>
              </div>
            </div>

            <div class="info-item">
              <div class="info-content">
                <h4>电话</h4>
                <p>+86 15201327903</p>
              </div>
            </div>

            <div class="info-item">
              <div class="info-content">
                <h4>邮箱</h4>
                <p>dunanculture@163.com</p>
              </div>
            </div>

            <div class="info-item">
              <div class="info-content">
                <h4>营业时间</h4>
                <p>周一至周五：09:00 - 18:00</p>
                <p>周六日：10:00 - 16:00</p>
              </div>
            </div>

            <div class="info-divider"></div>

            <div class="social-icons-container">
              <a
                href="https://mp.weixin.qq.com/mp/profile_ext?action=home&__biz=Mzk3NTMzNDA1Mg==#wechat_redirect"
                class="social-icon-link"
                title="微信号: Dunan-Culture"
              >
                <img src="@/assets/WeChat.svg" alt="微信" class="social-icon-img" />
              </a>
              <a href="https://xhslink.com/m/2XhPX8nHth6" class="social-icon-link" title="小红书">
                <img src="@/assets/RedNote.svg" alt="小红书" class="social-icon-img" />
              </a>
              <div class="social-icon-link" title="B站">
                <img src="@/assets/Bilibili.svg" alt="B站" class="social-icon-img" />
              </div>
              <div class="social-icon-link" title="抖音">
                <img src="@/assets/TikTok.svg" alt="抖音" class="social-icon-img" />
              </div>
            </div>
          </div>
        </div>

        <!-- 右侧：联系图片 -->
        <div class="contact-right">
          <div class="contact-right-container">
            <div class="map-container" @mouseenter="resetAnimation">
              <img src="@/assets/contact-map.png" alt="联系我们" class="contact-image" />
              <div
                class="map-overlay"
                ref="mapOverlay"
                @click="copyAddress"
                @mouseleave="hideOverlay"
              >
                <div class="map-address-title">工作室地址</div>
                <div class="map-address-divider"></div>
                <div class="map-address-line line-color-1">北京市延庆区</div>
                <div class="map-address-line line-color-2">妫水北街33号</div>
                <div class="map-address-line line-color-1">观澜国际公寓1217室</div>
              </div>
              <!-- 复制成功 dialog -->
              <div v-if="showCopyDialog" class="copy-dialog-overlay">
                <div class="copy-dialog">
                  <p>{{ copyMessage }}</p>
                </div>
              </div>
            </div>
            <div class="contact-map-info">
              <p>有任何疑问或想了解更多？欢迎与我们取得联系</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const mapOverlay = ref<HTMLElement | null>(null)
const copyMessage = ref('')
const showCopyDialog = ref(false)

const resetAnimation = () => {
  if (mapOverlay.value) {
    // 移除动画类
    mapOverlay.value.classList.remove('animate')
    // 触发重排，重新应用动画
    void mapOverlay.value.offsetWidth
    mapOverlay.value.classList.add('animate')
  }
}

const hideOverlay = () => {
  if (mapOverlay.value) {
    mapOverlay.value.classList.remove('animate')
  }
}

const copyAddress = () => {
  const address = '北京市延庆区妫水北街33号观澜国际公寓1217室'
  navigator.clipboard
    .writeText(address)
    .then(() => {
      // 复制成功，显示 dialog（保持 overlay 显示）
      copyMessage.value = '地址已复制到剪贴板'
      showCopyDialog.value = true
      setTimeout(() => {
        showCopyDialog.value = false
        // 再等一会儿才隐藏 overlay，给用户时间看完地址
        setTimeout(() => {
          if (mapOverlay.value) {
            mapOverlay.value.classList.remove('animate')
          }
        }, 300)
      }, 2000)
    })
    .catch(() => {
      // 复制失败的处理
      copyMessage.value = '复制失败，请重试'
      showCopyDialog.value = true
      setTimeout(() => {
        showCopyDialog.value = false
        setTimeout(() => {
          if (mapOverlay.value) {
            mapOverlay.value.classList.remove('animate')
          }
        }, 300)
      }, 1500)
    })
}
</script>

<style scoped>
/* Contact 样式在全局 styles.css 中 */
</style>
