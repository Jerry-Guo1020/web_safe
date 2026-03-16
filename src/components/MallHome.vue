<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// 商品数据
const hotProducts = ref([
  { id: 1, name: '智能手表 Pro', price: 299, originalPrice: 599, discount: '5折', image: 'https://picsum.photos/200/200?random=1' },
  { id: 2, name: '无线蓝牙耳机', price: 89, originalPrice: 199, discount: '4.5折', image: 'https://picsum.photos/200/200?random=2' },
  { id: 3, name: '便携充电宝', price: 59, originalPrice: 129, discount: '限时特惠', image: 'https://picsum.photos/200/200?random=3' },
  { id: 4, name: '护眼台灯', price: 129, originalPrice: 259, discount: '新品上市', image: 'https://picsum.photos/200/200?random=4' },
])

const categories = ref([
  { id: 1, name: '数码电子', icon: '📱', color: '#4facfe' },
  { id: 2, name: '家居生活', icon: '🏠', color: '#43e97b' },
  { id: 3, name: '服饰鞋包', icon: '👔', color: '#fa709a' },
  { id: 4, name: '美妆护肤', icon: '💄', color: '#a18cd1' },
  { id: 5, name: '食品饮料', icon: '🍎', color: '#ffecd2' },
  { id: 6, name: '运动户外', icon: '⚽', color: '#667eea' },
  { id: 7, name: '图书文具', icon: '📚', color: '#f093fb' },
  { id: 8, name: '母婴用品', icon: '🍼', color: '#f5576c' },
])

const flashDeals = ref([
  { id: 1, name: '智能音箱 Mini', price: 99, originalPrice: 299, sold: 1234, total: 2000, image: 'https://picsum.photos/150/150?random=5' },
  { id: 2, name: '机械键盘 RGB', price: 199, originalPrice: 399, sold: 856, total: 1500, image: 'https://picsum.photos/150/150?random=6' },
  { id: 3, name: '运动手环', price: 49, originalPrice: 149, sold: 2341, total: 3000, image: 'https://picsum.photos/150/150?random=7' },
])

const banners = ref([
  { id: 1, title: '春季焕新季', subtitle: '全场低至3折起', bg: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)' },
  { id: 2, title: '品质生活节', subtitle: '满300减50', bg: 'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)' },
  { id: 3, title: '新品首发', subtitle: '限时抢购中', bg: 'linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)' },
])

const currentBanner = ref(0)

// 自动轮播
let bannerInterval = null

onMounted(() => {
  bannerInterval = setInterval(() => {
    currentBanner.value = (currentBanner.value + 1) % banners.value.length
  }, 4000)
})

onUnmounted(() => {
  if (bannerInterval) clearInterval(bannerInterval)
})

function getProgress(sold, total) {
  return Math.min((sold / total) * 100, 100)
}
</script>

<template>
  <div class="mall-home">
    <!-- 顶部通知栏 -->
    <div class="top-bar">
      <div class="notice">
        <span class="icon">📢</span>
        <span class="text">新人注册立享188元优惠券包</span>
      </div>
    </div>

    <!-- 导航栏 -->
    <header class="header">
      <div class="header-content">
        <div class="logo">
          <span class="logo-icon">🛒</span>
          <span class="logo-text">品质商城</span>
        </div>
        <div class="search-box">
          <input type="text" placeholder="搜索商品..." />
          <button class="search-btn">🔍</button>
        </div>
        <div class="header-actions">
          <div class="action-item">
            <span class="icon">👤</span>
          </div>
          <div class="action-item cart">
            <span class="icon">🛒</span>
            <span class="badge">3</span>
          </div>
        </div>
      </div>
    </header>

    <!-- 主内容区 -->
    <main class="main-content">
      <!-- 轮播 -->
      <section class="banner-section">
        <div class="banner-container">
          <div
            class="banner-slide"
            :style="{ background: banners[currentBanner].bg }"
          >
            <h2 class="banner-title">{{ banners[currentBanner].title }}</h2>
            <p class="banner-subtitle">{{ banners[currentBanner].subtitle }}</p>
            <button class="banner-btn">立即抢购</button>
          </div>
          <div class="banner-dots">
            <span
              v-for="(banner, index) in banners"
              :key="banner.id"
              class="dot"
              :class="{ active: index === currentBanner }"
              @click="currentBanner = index"
            ></span>
          </div>
        </div>
      </section>

      <!-- 快捷入口 -->
      <section class="quick-entry">
        <div class="promo-row">
          <div class="promo-item">
            <span class="promo-icon">🚀</span>
            <span class="promo-label">闪电发货</span>
          </div>
          <div class="promo-item">
            <span class="promo-icon">✅</span>
            <span class="promo-label">正品保障</span>
          </div>
          <div class="promo-item">
            <span class="promo-icon">🎁</span>
            <span class="promo-label">新人专享</span>
          </div>
        </div>
      </section>

      <!-- 分类入口 -->
      <section class="category-section">
        <div class="category-grid">
          <div v-for="cat in categories" :key="cat.id" class="category-card">
            <span class="cat-icon">{{ cat.icon }}</span>
            <span class="cat-name">{{ cat.name }}</span>
          </div>
        </div>
      </section>

      <!-- 限时秒杀 -->
      <section class="flash-deal-section">
        <div class="section-header">
          <div class="section-title">
            <span class="icon">⚡</span>
            <span class="text">限时秒杀</span>
          </div>
          <div class="countdown">
            <span class="time">02:34:56</span>
          </div>
        </div>
        <div class="flash-deal-scroll">
          <div v-for="deal in flashDeals" :key="deal.id" class="flash-deal-item">
            <div class="deal-image">
              <img :src="deal.image" :alt="deal.name" />
            </div>
            <div class="deal-info">
              <div class="deal-name">{{ deal.name }}</div>
              <div class="deal-price">
                <span class="current">¥{{ deal.price }}</span>
                <span class="original">¥{{ deal.originalPrice }}</span>
              </div>
              <div class="deal-progress">
                <div class="progress-bar">
                  <div class="progress" :style="{ width: getProgress(deal.sold, deal.total) + '%' }"></div>
                </div>
                <span class="progress-text">已抢{{ Math.round(getProgress(deal.sold, deal.total)) }}%</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- 热门推荐 -->
      <section class="hot-products-section">
        <div class="section-header">
          <div class="section-title">
            <span class="icon">🔥</span>
            <span class="text">热门推荐</span>
          </div>
          <a href="#" class="view-more">更多 ></a>
        </div>
        <div class="product-grid">
          <div v-for="product in hotProducts" :key="product.id" class="product-card">
            <div class="product-image">
              <img :src="product.image" :alt="product.name" />
              <span class="discount-tag">{{ product.discount }}</span>
            </div>
            <div class="product-info">
              <div class="product-name">{{ product.name }}</div>
              <div class="product-price">
                <span class="current">¥{{ product.price }}</span>
                <span class="original">¥{{ product.originalPrice }}</span>
              </div>
              <div class="product-sales">已售 1.2万+</div>
            </div>
          </div>
        </div>
      </section>

      <!-- 品牌专区 -->
      <section class="brands-section">
        <div class="section-header">
          <div class="section-title">
            <span class="icon">🏷️</span>
            <span class="text">品牌专区</span>
          </div>
        </div>
        <div class="brands-scroll">
          <div class="brand-item">
            <span class="brand-name">华为</span>
          </div>
          <div class="brand-item">
            <span class="brand-name">小米</span>
          </div>
          <div class="brand-item">
            <span class="brand-name">联想</span>
          </div>
          <div class="brand-item">
            <span class="brand-name">美的</span>
          </div>
          <div class="brand-item">
            <span class="brand-name">格力</span>
          </div>
          <div class="brand-item">
            <span class="brand-name">海尔</span>
          </div>
        </div>
      </section>
    </main>

    <!-- 页脚 -->
    <footer class="footer">
      <div class="footer-bottom">
        <p>© 2024 品质生活商城</p>
        <p class="footer-tip">仅供网络安全教学演示使用</p>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.mall-home {
  min-height: 100vh;
  background: #f5f5f5;
  padding-bottom: 20px;
}

/* 顶部通知栏 */
.top-bar {
  background: linear-gradient(90deg, #ff6b6b, #ff8e53);
  color: white;
  padding: 6px 10px;
  font-size: 12px;
}

.notice {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  overflow: hidden;
}

.notice .text {
  white-space: nowrap;
}

/* 导航栏 */
.header {
  background: white;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  position: sticky;
  top: 0;
  z-index: 100;
}

.header-content {
  padding: 10px 12px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.logo {
  display: flex;
  align-items: center;
  gap: 4px;
  flex-shrink: 0;
}

.logo-icon {
  font-size: 22px;
}

.logo-text {
  font-size: 15px;
  font-weight: bold;
  color: #333;
}

.search-box {
  flex: 1;
  display: flex;
  border: 2px solid #ff6b6b;
  border-radius: 20px;
  overflow: hidden;
  height: 36px;
}

.search-box input {
  flex: 1;
  padding: 0 12px;
  border: none;
  outline: none;
  font-size: 13px;
  min-width: 0;
}

.search-btn {
  padding: 0 12px;
  background: #ff6b6b;
  border: none;
  color: white;
  cursor: pointer;
  font-size: 14px;
}

.header-actions {
  display: flex;
  gap: 15px;
  flex-shrink: 0;
}

.action-item {
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  position: relative;
}

.action-item .icon {
  font-size: 20px;
}

.cart .badge {
  position: absolute;
  top: -4px;
  right: -8px;
  background: #ff4757;
  color: white;
  font-size: 10px;
  padding: 1px 5px;
  border-radius: 8px;
  min-width: 16px;
  text-align: center;
}

/* 主内容 */
.main-content {
  padding: 10px;
}

/* 轮播 */
.banner-section {
  margin-bottom: 10px;
}

.banner-container {
  position: relative;
  border-radius: 10px;
  overflow: hidden;
}

.banner-slide {
  height: 150px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: white;
  text-align: center;
  padding: 15px;
}

.banner-title {
  font-size: 22px;
  margin: 0 0 6px;
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.2);
}

.banner-subtitle {
  font-size: 13px;
  margin: 0 0 12px;
  opacity: 0.9;
}

.banner-btn {
  padding: 8px 20px;
  background: white;
  border: none;
  border-radius: 20px;
  font-size: 12px;
  font-weight: bold;
  cursor: pointer;
}

.banner-dots {
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 6px;
}

.dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
}

.dot.active {
  background: white;
  width: 16px;
  border-radius: 3px;
}

/* 快捷入口 */
.quick-entry {
  margin-bottom: 10px;
}

.promo-row {
  display: flex;
  gap: 10px;
}

.promo-item {
  flex: 1;
  background: white;
  padding: 12px 8px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
}

.promo-icon {
  font-size: 20px;
}

.promo-label {
  font-size: 11px;
  color: #333;
  font-weight: 500;
}

/* 分类入口 */
.category-section {
  margin-bottom: 10px;
}

.category-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 8px;
}

.category-card {
  background: white;
  padding: 12px 6px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  cursor: pointer;
  transition: transform 0.2s;
}

.category-card:active {
  transform: scale(0.95);
}

.category-card .cat-icon {
  font-size: 28px;
}

.category-card .cat-name {
  font-size: 11px;
  color: #333;
}

/* 限时秒杀 */
.flash-deal-section {
  background: white;
  border-radius: 10px;
  padding: 12px;
  margin-bottom: 10px;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.section-title {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 15px;
  font-weight: bold;
}

.section-title .icon {
  font-size: 16px;
}

.countdown .time {
  font-size: 13px;
  font-weight: bold;
  color: #ff4757;
  background: #fff5f5;
  padding: 4px 8px;
  border-radius: 4px;
}

.flash-deal-scroll {
  display: flex;
  gap: 10px;
  overflow-x: auto;
  padding-bottom: 5px;
  -webkit-overflow-scrolling: touch;
}

.flash-deal-scroll::-webkit-scrollbar {
  display: none;
}

.flash-deal-item {
  flex-shrink: 0;
  width: 140px;
  background: #fafafa;
  border-radius: 10px;
  overflow: hidden;
  cursor: pointer;
}

.deal-image {
  width: 100%;
  height: 100px;
  overflow: hidden;
}

.deal-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.deal-info {
  padding: 10px;
}

.deal-name {
  font-size: 12px;
  color: #333;
  margin-bottom: 6px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.deal-price .current {
  font-size: 15px;
  font-weight: bold;
  color: #ff4757;
}

.deal-price .original {
  font-size: 10px;
  color: #999;
  text-decoration: line-through;
  margin-left: 4px;
}

.deal-progress {
  margin-top: 6px;
}

.progress-bar {
  height: 4px;
  background: #ffe0e0;
  border-radius: 2px;
  overflow: hidden;
}

.progress {
  height: 100%;
  background: linear-gradient(90deg, #ff6b6b, #ff8e53);
  border-radius: 2px;
}

.progress-text {
  font-size: 10px;
  color: #999;
  margin-top: 3px;
  display: block;
}

/* 热门推荐 */
.hot-products-section {
  margin-bottom: 10px;
}

.view-more {
  font-size: 12px;
  color: #999;
  text-decoration: none;
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
  margin-top: 10px;
}

.product-card {
  background: white;
  border-radius: 10px;
  overflow: hidden;
  cursor: pointer;
}

.product-image {
  position: relative;
  aspect-ratio: 1;
  overflow: hidden;
}

.product-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.discount-tag {
  position: absolute;
  top: 8px;
  left: 8px;
  background: #ff4757;
  color: white;
  font-size: 10px;
  padding: 2px 6px;
  border-radius: 3px;
}

.product-info {
  padding: 10px;
}

.product-name {
  font-size: 13px;
  color: #333;
  margin-bottom: 6px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.product-price .current {
  font-size: 16px;
  font-weight: bold;
  color: #ff4757;
}

.product-price .original {
  font-size: 11px;
  color: #999;
  text-decoration: line-through;
  margin-left: 4px;
}

.product-sales {
  font-size: 10px;
  color: #999;
  margin-top: 4px;
}

/* 品牌专区 */
.brands-section {
  margin-bottom: 10px;
}

.brands-scroll {
  display: flex;
  gap: 10px;
  overflow-x: auto;
  padding-bottom: 5px;
  -webkit-overflow-scrolling: touch;
}

.brands-scroll::-webkit-scrollbar {
  display: none;
}

.brand-item {
  flex-shrink: 0;
  background: white;
  padding: 15px 25px;
  border-radius: 10px;
  text-align: center;
  cursor: pointer;
}

.brand-name {
  font-size: 14px;
  font-weight: bold;
  color: #333;
}

/* 页脚 */
.footer {
  background: #2d3436;
  color: #dfe6e9;
  padding: 15px 10px;
  margin-top: 10px;
  text-align: center;
}

.footer-bottom p {
  font-size: 11px;
  color: #636e72;
  margin: 3px 0;
}

.footer-tip {
  color: #b2bec3;
}
</style>