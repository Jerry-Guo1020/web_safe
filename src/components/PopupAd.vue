<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const showPopup = ref(false)
const countdown = ref(8)
const shake = ref(false)
const closeAttempts = ref(0) // 记录关闭尝试次数
const showWarning = ref(false) // 显示警示弹窗

let timer = null
let countdownTimer = null

onMounted(() => {
  // 5秒后显示弹窗
  timer = setTimeout(() => {
    showPopup.value = true
    startCountdown()
  }, 5000)
})

onUnmounted(() => {
  if (timer) clearTimeout(timer)
  if (countdownTimer) clearInterval(countdownTimer)
})

function startCountdown() {
  countdownTimer = setInterval(() => {
    countdown.value--
    if (countdown.value <= 0) {
      clearInterval(countdownTimer)
    }
  }, 1000)
}

function closePopup() {
  closeAttempts.value++

  // 前3次关闭无效，第4次才能真正关闭
  if (closeAttempts.value <= 3) {
    shake.value = true
    setTimeout(() => {
      shake.value = false
    }, 500)
    return
  }

  // 第4次及以后正常关闭
  showPopup.value = false
}

function claimPrize() {
  shake.value = true
  setTimeout(() => {
    shake.value = false
    // 显示自定义警示弹窗
    showWarning.value = true
  }, 500)
}

function closeWarning() {
  showWarning.value = false
}
</script>

<template>
  <!-- 遮罩层 -->
  <Transition name="fade">
    <div v-if="showPopup" class="popup-overlay" @click.self="closePopup">
      <!-- 弹窗主体 -->
      <div class="popup-container" :class="{ shake: shake }">
        <!-- 顶部闪烁条 -->
        <div class="flashing-bar">
          <span>恭喜！恭喜！恭喜！</span>
          <span>恭喜！恭喜！恭喜！</span>
        </div>

        <!-- 关闭按钮（假的，很难点到） -->
        <div class="fake-close-buttons">
          <span class="fake-x" @click="claimPrize">✕</span>
          <span class="fake-x" @click="claimPrize">✕</span>
          <span class="fake-x" @click="claimPrize">✕</span>
        </div>

        <!-- 真正的关闭按钮 -->
        <button class="real-close" @click="closePopup" title="关闭">
          ×
          <span class="close-hint" v-if="closeAttempts > 0 && closeAttempts <= 3">
            ({{ 4 - closeAttempts }})
          </span>
        </button>

        <!-- 主内容区 -->
        <div class="popup-content">
          <!-- 惊喜图标 -->
          <div class="prize-icon">
            <div class="gift-box">🎁</div>
            <div class="sparkles">
              <span>✨</span><span>✨</span><span>✨</span>
            </div>
          </div>

          <!-- 标题 -->
          <div class="popup-title">
            <span class="red-text">【惊喜通知】</span>
          </div>

          <!-- 主文案 -->
          <div class="popup-message">
            <p class="congrats-text">
              🎉 恭喜您获得 <span class="highlight">iPhone 17 ProMax</span> 抽奖资格！🎉
            </p>
            <p class="sub-text">
              您是今日第 <span class="red-number">888</span> 位幸运用户
            </p>
          </div>

          <!-- 倒计时 -->
          <div class="countdown-box">
            <div class="countdown-label">⏰ 限时领取，剩余时间：</div>
            <div class="countdown-timer">
              <span class="time-digit">{{ countdown }}</span>
              <span class="time-unit">秒</span>
            </div>
          </div>

          <!-- 假的iPhone图片 -->
          <div class="prize-image">
            <div class="phone-mockup">
              <div class="phone-screen">iPhone 17 ProMax</div>
            </div>
          </div>

          <!-- CTA按钮 -->
          <button class="cta-button" @click="claimPrize">
            <span class="btn-text">👉 点击立即领取 👈</span>
            <span class="btn-sub">(100%中奖)</span>
          </button>

          <!-- 底部虚假信息 -->
          <div class="popup-footer">
            <p class="verified-badge">✓ 官方认证 ✓ 安全可靠</p>
            <p class="fake-stats">今日已送出: <span class="green">1,234</span> 部</p>
          </div>
        </div>

        <!-- 底部滚动文字 -->
        <div class="scrolling-text">
          <span>
            🎁 张**刚刚领取了iPhone 17 ProMax | 🎁 李**刚刚领取了iPhone 17 ProMax | 🎁 王**刚刚领取了iPhone 17 ProMax | 🎁 赵**刚刚领取了iPhone 17 ProMax |
          </span>
        </div>
      </div>
    </div>
  </Transition>

  <!-- 角落小广告 -->
  <Transition name="slide-in">
    <div v-if="showPopup && countdown <= 6" class="corner-ad" @click="claimPrize">
      <div class="corner-close" @click.stop="closePopup">×</div>
      <div class="corner-content">
        <span class="corner-icon">📱</span>
        <span class="corner-text">您有1个未领取的iPhone</span>
      </div>
    </div>
  </Transition>

  <!-- 自定义警示弹窗 -->
  <Transition name="fade">
    <div v-if="showWarning" class="warning-overlay" @click.self="closeWarning">
      <div class="warning-modal">
        <div class="warning-icon">
          <span class="shield">🛡️</span>
        </div>
        <h3 class="warning-title">网络安全警示</h3>
        <p class="warning-text">
          点击这个是错的哦！
        </p>
        <p class="warning-subtext">
          我们不应该轻易相信这些来历不明的广告弹窗。<br/>
          这类"中奖"信息往往是网络诈骗的常见手段。
        </p>
        <div class="warning-tips">
          <div class="tip-item">❌ 不要点击可疑链接</div>
          <div class="tip-item">❌ 不要透露个人信息</div>
          <div class="tip-item">❌ 不要转账汇款</div>
        </div>
        <button class="warning-btn" @click="closeWarning">
          我知道了
        </button>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
/* 遮罩层 */
.popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
  backdrop-filter: blur(2px);
  padding: 15px;
}

/* 弹窗容器 */
.popup-container {
  position: relative;
  width: 100%;
  max-width: 380px;
  background: linear-gradient(135deg, #fff8dc, #ffe4b5, #ffd700);
  border-radius: 12px;
  box-shadow:
    0 0 30px rgba(255, 215, 0, 0.5),
    0 0 60px rgba(255, 0, 0, 0.3),
    inset 0 2px 0 rgba(255, 255, 255, 0.8);
  overflow: hidden;
  animation: popupBounce 0.5s ease-out;
}

.popup-container.shake {
  animation: shake 0.5s ease-in-out;
}

@keyframes popupBounce {
  0% { transform: scale(0.3) rotate(-10deg); opacity: 0; }
  50% { transform: scale(1.1) rotate(2deg); }
  100% { transform: scale(1) rotate(0); opacity: 1; }
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-10px) rotate(-1deg); }
  75% { transform: translateX(10px) rotate(1deg); }
}

/* 顶部闪烁条 */
.flashing-bar {
  background: linear-gradient(90deg, #ff0000, #ff6600, #ffff00, #00ff00, #0066ff, #ff00ff, #ff0000);
  background-size: 200% 100%;
  animation: flashColors 2s linear infinite;
  padding: 8px 0;
  display: flex;
  justify-content: space-around;
  color: white;
  font-weight: bold;
  text-shadow: 1px 1px 2px rgba(0,0,0,0.5);
  font-size: 12px;
}

@keyframes flashColors {
  0% { background-position: 0% 50%; }
  100% { background-position: 200% 50%; }
}

/* 假的关闭按钮 */
.fake-close-buttons {
  position: absolute;
  top: 38px;
  right: 10px;
  display: flex;
  gap: 5px;
}

.fake-x {
  width: 16px;
  height: 16px;
  background: #ff4444;
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 9px;
  cursor: pointer;
  opacity: 0.7;
}

.fake-x:active {
  opacity: 1;
}

/* 真正的关闭按钮 */
.real-close {
  position: absolute;
  top: 40px;
  left: 10px;
  width: 24px;
  height: 24px;
  background: rgba(255, 255, 255, 0.9);
  border: 1px solid #ddd;
  border-radius: 50%;
  color: #666;
  font-size: 14px;
  cursor: pointer;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.close-hint {
  position: absolute;
  font-size: 10px;
  color: #ff6b6b;
  bottom: -12px;
  left: 50%;
  transform: translateX(-50%);
  white-space: nowrap;
}

/* 主内容 */
.popup-content {
  padding: 15px;
  text-align: center;
}

/* 奖品图标 */
.prize-icon {
  position: relative;
  font-size: 60px;
  margin: 10px 0;
  animation: bounce 1s ease-in-out infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

.gift-box {
  display: inline-block;
  filter: drop-shadow(0 5px 15px rgba(255, 215, 0, 0.5));
}

.sparkles {
  position: absolute;
  width: 100%;
  top: 0;
  left: 0;
  display: flex;
  justify-content: space-around;
  font-size: 16px;
  animation: sparkle 0.8s ease-in-out infinite alternate;
}

@keyframes sparkle {
  0% { opacity: 0.5; transform: scale(0.8); }
  100% { opacity: 1; transform: scale(1.2); }
}

.sparkles span:nth-child(1) { animation-delay: 0s; }
.sparkles span:nth-child(2) { animation-delay: 0.2s; }
.sparkles span:nth-child(3) { animation-delay: 0.4s; }

/* 标题 */
.popup-title {
  margin: 8px 0;
}

.red-text {
  color: #ff0000;
  font-size: 20px;
  font-weight: bold;
  text-shadow: 2px 2px 4px rgba(255, 0, 0, 0.3);
  animation: pulse 1s ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}

/* 消息 */
.popup-message {
  margin: 10px 0;
}

.congrats-text {
  font-size: 15px;
  color: #333;
  margin: 5px 0;
}

.highlight {
  color: #ff0000;
  font-weight: bold;
  font-size: 18px;
  text-decoration: underline;
}

.sub-text {
  font-size: 12px;
  color: #666;
  margin: 5px 0;
}

.red-number {
  color: #ff0000;
  font-weight: bold;
  font-size: 16px;
}

/* 倒计时 */
.countdown-box {
  background: linear-gradient(135deg, #ff6b6b, #ee5a5a);
  color: white;
  padding: 8px;
  border-radius: 8px;
  margin: 10px auto;
  width: 160px;
  box-shadow: 0 4px 15px rgba(255, 0, 0, 0.3);
}

.countdown-label {
  font-size: 11px;
}

.countdown-timer {
  display: flex;
  justify-content: center;
  align-items: baseline;
  gap: 2px;
}

.time-digit {
  font-size: 28px;
  font-weight: bold;
  animation: countPulse 1s ease-in-out infinite;
}

@keyframes countPulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.1); color: #ffeb3b; }
}

.time-unit {
  font-size: 12px;
}

/* 手机图片 */
.prize-image {
  margin: 10px 0;
}

.phone-mockup {
  width: 60px;
  height: 110px;
  background: linear-gradient(135deg, #1a1a2e, #16213e);
  border-radius: 12px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow:
    0 10px 30px rgba(0, 0, 0, 0.3),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
  position: relative;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0) rotate(-2deg); }
  50% { transform: translateY(-10px) rotate(2deg); }
}

.phone-mockup::before {
  content: '';
  position: absolute;
  top: 4px;
  width: 20px;
  height: 2px;
  background: #333;
  border-radius: 2px;
}

.phone-screen {
  color: white;
  font-size: 10px;
  font-weight: bold;
  text-align: center;
}

/* CTA按钮 */
.cta-button {
  background: linear-gradient(135deg, #ff4444, #ff6600);
  color: white;
  border: none;
  padding: 12px 30px;
  font-size: 16px;
  font-weight: bold;
  border-radius: 50px;
  cursor: pointer;
  box-shadow:
    0 5px 20px rgba(255, 68, 68, 0.5),
    inset 0 2px 0 rgba(255, 255, 255, 0.3);
  transition: all 0.3s ease;
  animation: btnPulse 1.5s ease-in-out infinite;
  margin-top: 10px;
}

@keyframes btnPulse {
  0%, 100% { box-shadow: 0 5px 20px rgba(255, 68, 68, 0.5), inset 0 2px 0 rgba(255, 255, 255, 0.3); }
  50% { box-shadow: 0 5px 30px rgba(255, 68, 68, 0.8), inset 0 2px 0 rgba(255, 255, 255, 0.3); }
}

.cta-button:active {
  transform: scale(0.95);
  background: linear-gradient(135deg, #ff6666, #ff8833);
}

.btn-text {
  display: block;
  font-size: 15px;
}

.btn-sub {
  display: block;
  font-size: 11px;
  opacity: 0.9;
  margin-top: 2px;
}

/* 底部 */
.popup-footer {
  margin-top: 10px;
  font-size: 11px;
}

.verified-badge {
  color: #00aa00;
  font-weight: bold;
  margin: 5px 0;
}

.fake-stats {
  color: #666;
  margin: 5px 0;
}

.green {
  color: #00aa00;
  font-weight: bold;
}

/* 滚动文字 */
.scrolling-text {
  background: #1a1a2e;
  color: #ffd700;
  padding: 6px 0;
  overflow: hidden;
  white-space: nowrap;
}

.scrolling-text span {
  display: inline-block;
  animation: scroll 15s linear infinite;
  font-size: 11px;
}

@keyframes scroll {
  0% { transform: translateX(100%); }
  100% { transform: translateX(-100%); }
}

/* 角落广告 */
.corner-ad {
  position: fixed;
  bottom: 15px;
  right: 15px;
  background: linear-gradient(135deg, #ff4444, #ff6600);
  color: white;
  padding: 10px 15px;
  border-radius: 25px;
  cursor: pointer;
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.3);
  z-index: 9998;
  animation: slideIn 0.5s ease-out, bounce 1s ease-in-out infinite;
}

.corner-close {
  position: absolute;
  top: -8px;
  right: -8px;
  width: 18px;
  height: 18px;
  background: #333;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 10px;
  cursor: pointer;
}

.corner-content {
  display: flex;
  align-items: center;
  gap: 6px;
}

.corner-icon {
  font-size: 16px;
}

.corner-text {
  font-size: 11px;
  font-weight: bold;
}

@keyframes slideIn {
  0% { transform: translateX(100%); opacity: 0; }
  100% { transform: translateX(0); opacity: 1; }
}

/* ========== 警示弹窗样式 ========== */
.warning-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 99999;
  padding: 20px;
}

.warning-modal {
  background: white;
  border-radius: 20px;
  padding: 30px 25px;
  max-width: 340px;
  width: 100%;
  text-align: center;
  animation: modalPop 0.4s ease-out;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
}

@keyframes modalPop {
  0% { transform: scale(0.8); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}

.warning-icon {
  margin-bottom: 15px;
}

.shield {
  font-size: 60px;
  display: inline-block;
  animation: shieldPulse 2s ease-in-out infinite;
}

@keyframes shieldPulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.1); }
}

.warning-title {
  color: #1a73e8;
  font-size: 22px;
  margin: 0 0 15px 0;
  font-weight: bold;
}

.warning-text {
  font-size: 18px;
  color: #ff4444;
  font-weight: bold;
  margin: 0 0 10px 0;
}

.warning-subtext {
  font-size: 14px;
  color: #666;
  line-height: 1.6;
  margin: 0 0 20px 0;
}

.warning-tips {
  background: #fff3cd;
  border-radius: 12px;
  padding: 15px;
  margin-bottom: 20px;
  text-align: left;
}

.tip-item {
  font-size: 13px;
  color: #856404;
  margin: 8px 0;
  font-weight: 500;
}

.warning-btn {
  background: linear-gradient(135deg, #1a73e8, #4285f4);
  color: white;
  border: none;
  padding: 14px 40px;
  font-size: 16px;
  font-weight: bold;
  border-radius: 30px;
  cursor: pointer;
  width: 100%;
  transition: all 0.3s ease;
}

.warning-btn:active {
  transform: scale(0.98);
  background: linear-gradient(135deg, #1557b0, #1a73e8);
}

/* 过渡动画 */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.slide-in-enter-active,
.slide-in-leave-active {
  transition: all 0.5s ease;
}

.slide-in-enter-from,
.slide-in-leave-to {
  transform: translateX(100%);
  opacity: 0;
}
</style>