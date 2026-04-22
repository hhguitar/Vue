<template>
  <div class="page">
    <aside class="sidebar">
      <div class="brand">
        <div class="brand-icon">语</div>
        <div class="brand-text">播音教学平台</div>
      </div>

      <nav class="menu">
        <div class="menu-item">
          <span class="mi-icon">⌂</span>
          <span>首页</span>
        </div>

        <div class="menu-group">
          <div class="menu-label">数字人资产</div>
          <div class="menu-item">
            <span class="mi-icon">👤</span>
            <span>数字人资产</span>
          </div>
          <div class="menu-item active">
            <span class="mi-icon">🎞</span>
            <span>数字人克隆</span>
          </div>
          <div class="menu-item">
            <span class="mi-icon">📹</span>
            <span>视频制作</span>
          </div>
        </div>

        <div class="menu-group">
          <div class="menu-label">声音资产</div>
          <div class="menu-item">
            <span class="mi-icon">🔊</span>
            <span>文本配音</span>
          </div>
          <div class="menu-item">
            <span class="mi-icon">🎙</span>
            <span>声音复刻</span>
          </div>
        </div>
      </nav>

      <div class="sidebar-footer">
        <div class="menu-item small">
          <span class="mi-icon">🗂</span>
          <span>数据管理</span>
        </div>
        <div class="menu-item small">
          <span class="mi-icon">⚙</span>
          <span>我的</span>
        </div>
      </div>
    </aside>

    <main class="content">
      <div class="page-title">试题</div>

      <section class="workspace">
        <div class="panel form-panel">
          <div class="field-block">
            <label class="field-label">数字人形象名称</label>
            <div class="input-wrap">
              <input
                v-model="form.name"
                class="text-input"
                type="text"
                maxlength="8"
                placeholder="请输入数字人形象名称"
              />
              <span class="count">{{ form.name.length }}/8</span>
            </div>
          </div>

          <div class="field-block">
            <label class="field-label">视频上传</label>
            <div
              class="upload-box"
              :class="{ dragging: isDragging, uploaded: !!fileName }"
              @click="openFileDialog"
              @dragover.prevent="isDragging = true"
              @dragleave.prevent="isDragging = false"
              @drop.prevent="handleDrop"
            >
              <input
                ref="fileInput"
                class="hidden-input"
                type="file"
                accept="video/mp4"
                @change="handleFileChange"
              />

              <div class="upload-icon">
                <div class="arrow-up"></div>
              </div>
              <div class="upload-title">上传视频制作您的数字人分身</div>
              <div class="upload-subtitle">支持 mp4 格式，视频比例为 9:16</div>
              <button class="primary-btn" type="button" @click.stop="openFileDialog">
                上传视频
              </button>
              <div v-if="fileName" class="file-name">已选择：{{ fileName }}</div>
            </div>
          </div>

          <div class="field-block">
            <label class="field-label">注意事项</label>
            <div class="tips-grid">
              <div class="tip-card" v-for="tip in tips" :key="tip.title">
                <div class="tip-illus">{{ tip.icon }}</div>
                <div class="tip-text">{{ tip.title }}</div>
              </div>
            </div>
          </div>

          <div class="field-block">
            <label class="field-label">音色选择</label>
            <div class="voice-grid">
              <button
                v-for="voice in voices"
                :key="voice.id"
                class="voice-card"
                :class="{ selected: selectedVoice === voice.id }"
                type="button"
                @click="selectedVoice = voice.id"
              >
                <div class="voice-avatar">{{ voice.avatar }}</div>
                <div class="voice-meta">
                  <div class="voice-name">{{ voice.name }}</div>
                  <div class="voice-desc">{{ voice.desc }}</div>
                </div>
                <span class="radio-dot"></span>
              </button>
            </div>
          </div>

          <div class="submit-wrap">
            <button class="submit-btn" type="button">生成数字人</button>
          </div>
        </div>

        <div class="panel preview-panel">
          <div class="preview-title">案例视频</div>
          <div class="preview-frame">
            <div class="portrait-card">
              <div class="portrait-outline"></div>
              <div class="portrait-figure">
                <div class="hair"></div>
                <div class="head"></div>
                <div class="body"></div>
                <div class="arm left"></div>
                <div class="arm right"></div>
                <div class="leg left"></div>
                <div class="leg right"></div>
              </div>
            </div>
          </div>

          <ul class="check-list">
            <li v-for="item in checklist" :key="item">{{ item }}</li>
          </ul>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const fileInput = ref(null)
const fileName = ref('')
const isDragging = ref(false)
const selectedVoice = ref('xiaomei')

const form = ref({
  name: ''
})

const tips = [
  { icon: '☀', title: '光线充足且不过曝' },
  { icon: '🔉', title: '周围安静无噪音' },
  { icon: '4K', title: '1080P/25fps/音话同步' }
]

const voices = [
  { id: 'wanwan', avatar: '👩', name: '晓晓', desc: '新闻女声' },
  { id: 'xiaoyi', avatar: '👩‍💼', name: '小易', desc: '甜美女孩' },
  { id: 'xiaomei', avatar: '👩', name: '小媛', desc: '知性女声' },
  { id: 'yunbai', avatar: '👨', name: '云白', desc: '新闻男声' },
  { id: 'yunxi', avatar: '👨‍🎓', name: '云溪', desc: '青年学生' },
  { id: 'ziyang', avatar: '👱‍♂️', name: '子扬', desc: '精英青年' }
]

const checklist = [
  '不说话、不张嘴、不露牙',
  '表情简单，动作自然',
  '穿低领衣服，露出脖子',
  '光线充足，脸部整体光线均匀',
  '保持镜头、光线、景别、演绎位置不变'
]

function openFileDialog() {
  fileInput.value?.click()
}

function setFile(file) {
  if (!file) return
  fileName.value = file.name
}

function handleFileChange(event) {
  const [file] = event.target.files || []
  setFile(file)
}

function handleDrop(event) {
  isDragging.value = false
  const [file] = event.dataTransfer?.files || []
  setFile(file)
}
</script>

<style scoped>
:global(*) {
  box-sizing: border-box;
}

:global(body) {
  margin: 0;
  font-family: Inter, -apple-system, BlinkMacSystemFont, 'PingFang SC', 'Microsoft YaHei', sans-serif;
  background: #f4f6fb;
  color: #2a3557;
}

:global(button),
:global(input) {
  font: inherit;
}

.page {
  min-height: 100vh;
  display: flex;
  background:
    linear-gradient(90deg, #eff4fc 0%, #eef3fb 14%, #f3f5fd 14%, #f3f5fd 100%);
}

.sidebar {
  width: 130px;
  background: linear-gradient(180deg, #eaf3ff 0%, #d7ebff 100%);
  padding: 16px 10px;
  display: flex;
  flex-direction: column;
  border-right: 1px solid rgba(117, 144, 197, 0.12);
}

.brand {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 18px;
  padding: 6px 6px 12px;
}

.brand-icon {
  width: 24px;
  height: 24px;
  border-radius: 6px;
  background: linear-gradient(135deg, #506cff 0%, #7d56ff 100%);
  color: #fff;
  display: grid;
  place-items: center;
  font-size: 12px;
  font-weight: 700;
  box-shadow: 0 8px 18px rgba(74, 109, 255, 0.2);
}

.brand-text {
  font-size: 12px;
  font-weight: 700;
  color: #4160b7;
  white-space: nowrap;
}

.menu {
  display: flex;
  flex-direction: column;
  gap: 8px;
  flex: 1;
}

.menu-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.menu-label {
  color: #7b90c4;
  font-size: 12px;
  padding: 8px 10px 2px;
}

.menu-item {
  height: 36px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 0 12px;
  color: #52627f;
  font-size: 13px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.menu-item:hover {
  background: rgba(84, 120, 255, 0.09);
  color: #3e58a8;
}

.menu-item.active {
  color: #fff;
  background: linear-gradient(90deg, #4f8fff 0%, #704dff 100%);
  box-shadow: 0 10px 18px rgba(103, 103, 255, 0.22);
}

.menu-item.small {
  height: 34px;
  font-size: 12px;
}

.mi-icon {
  width: 16px;
  text-align: center;
  opacity: 0.9;
}

.sidebar-footer {
  padding-top: 12px;
}

.content {
  flex: 1;
  padding: 26px 26px 28px;
}

.page-title {
  font-size: 12px;
  color: #8d97b7;
  margin-bottom: 18px;
}

.workspace {
  display: grid;
  grid-template-columns: minmax(640px, 1fr) 335px;
  gap: 24px;
  align-items: start;
}

.panel {
  background: #fff;
  border-radius: 24px;
  box-shadow: 0 14px 36px rgba(100, 119, 160, 0.12);
}

.form-panel {
  padding: 22px 22px 16px;
}

.preview-panel {
  padding: 22px 24px 24px;
}

.field-block + .field-block {
  margin-top: 20px;
}

.field-label,
.preview-title {
  display: block;
  font-size: 15px;
  font-weight: 700;
  color: #3e4a68;
  margin-bottom: 12px;
}

.input-wrap {
  position: relative;
}

.text-input {
  width: 100%;
  height: 46px;
  background: #f6f7fb;
  border: 1px solid #edf0f6;
  border-radius: 12px;
  padding: 0 64px 0 14px;
  outline: none;
  color: #2d3555;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.text-input:focus {
  border-color: #8eabff;
  box-shadow: 0 0 0 4px rgba(95, 123, 255, 0.08);
}

.text-input::placeholder {
  color: #b1b7c8;
}

.count {
  position: absolute;
  right: 14px;
  top: 50%;
  transform: translateY(-50%);
  color: #aeb5c5;
  font-size: 12px;
}

.upload-box {
  min-height: 286px;
  border: 2px dashed #bcd0ff;
  border-radius: 22px;
  background: linear-gradient(180deg, #f9fbff 0%, #f5f8ff 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 28px 20px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease;
}

.upload-box:hover {
  transform: translateY(-1px);
  box-shadow: inset 0 0 0 1px rgba(89, 122, 255, 0.06), 0 12px 24px rgba(104, 131, 212, 0.08);
}

.upload-box.dragging,
.upload-box.uploaded {
  border-color: #6b95ff;
  background: linear-gradient(180deg, #f4f8ff 0%, #eef4ff 100%);
}

.hidden-input {
  display: none;
}

.upload-icon {
  width: 110px;
  height: 110px;
  border: 2px dashed #c6cde2;
  border-radius: 2px;
  margin-bottom: 14px;
  position: relative;
  display: grid;
  place-items: center;
}

.arrow-up {
  position: relative;
  width: 18px;
  height: 44px;
  background: #c5dbff;
  border-radius: 10px;
}

.arrow-up::before {
  content: '';
  position: absolute;
  left: 50%;
  top: -22px;
  transform: translateX(-50%);
  width: 0;
  height: 0;
  border-left: 23px solid transparent;
  border-right: 23px solid transparent;
  border-bottom: 26px solid #c5dbff;
}

.upload-title {
  font-size: 18px;
  font-weight: 700;
  color: #27314e;
  margin-bottom: 8px;
}

.upload-subtitle {
  font-size: 12px;
  color: #96a0b8;
  margin-bottom: 16px;
}

.primary-btn,
.submit-btn {
  min-width: 116px;
  height: 42px;
  border: none;
  border-radius: 999px;
  background: linear-gradient(180deg, #4f85ff 0%, #345fe9 100%);
  color: #fff;
  font-weight: 700;
  box-shadow: 0 10px 20px rgba(56, 95, 233, 0.22);
  cursor: pointer;
  transition: transform 0.18s ease, box-shadow 0.18s ease, filter 0.18s ease;
}

.primary-btn:hover,
.submit-btn:hover {
  transform: translateY(-1px);
  filter: brightness(1.03);
}

.primary-btn:active,
.submit-btn:active {
  transform: translateY(0);
  box-shadow: 0 6px 14px rgba(56, 95, 233, 0.2);
}

.file-name {
  margin-top: 14px;
  font-size: 13px;
  color: #4d63a8;
  max-width: 100%;
  word-break: break-all;
}

.tips-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}

.tip-card {
  background: #f4f6fc;
  border-radius: 14px;
  padding: 16px 12px 14px;
  min-height: 84px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: #5d6888;
  text-align: center;
}

.tip-illus {
  width: 44px;
  height: 44px;
  border: 2px dashed #b9bfcd;
  display: grid;
  place-items: center;
  margin-bottom: 8px;
  color: #7e8598;
  font-weight: 700;
}

.tip-text {
  font-size: 12px;
}

.voice-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 14px 16px;
}

.voice-card {
  height: 58px;
  border: 1px solid transparent;
  background: #f3f5fb;
  border-radius: 14px;
  padding: 8px 12px;
  display: flex;
  align-items: center;
  gap: 10px;
  position: relative;
  cursor: pointer;
  transition: all 0.2s ease;
  text-align: left;
}

.voice-card:hover {
  border-color: #c6d5ff;
  background: #f7f9ff;
}

.voice-card.selected {
  border-color: #7ea1ff;
  background: #eef3ff;
  box-shadow: 0 8px 18px rgba(102, 130, 222, 0.12);
}

.voice-avatar {
  width: 38px;
  height: 38px;
  border-radius: 50%;
  background: linear-gradient(180deg, #ffddb8 0%, #ffc19a 100%);
  display: grid;
  place-items: center;
  font-size: 22px;
  flex: 0 0 auto;
}

.voice-meta {
  min-width: 0;
}

.voice-name {
  font-size: 14px;
  font-weight: 700;
  color: #3f4a69;
}

.voice-desc {
  margin-top: 2px;
  font-size: 12px;
  color: #8e97af;
}

.radio-dot {
  position: absolute;
  right: 12px;
  top: 12px;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #c1c9dd;
}

.voice-card.selected .radio-dot {
  background: #6d8fff;
  box-shadow: 0 0 0 4px rgba(109, 143, 255, 0.12);
}

.submit-wrap {
  display: flex;
  justify-content: center;
  padding: 18px 0 0;
}

.submit-btn {
  min-width: 128px;
}

.preview-frame {
  background: #edf1fb;
  border-radius: 22px;
  height: 232px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 18px;
}

.portrait-card {
  width: 132px;
  height: 168px;
  position: relative;
}

.portrait-outline {
  position: absolute;
  inset: 0;
  border: 2px dashed #848894;
}

.portrait-figure {
  position: absolute;
  inset: 10px 18px 0;
}

.hair {
  position: absolute;
  top: 8px;
  left: 50%;
  transform: translateX(-50%);
  width: 72px;
  height: 54px;
  background: #5b3c34;
  border-radius: 38px 38px 28px 28px;
}

.head {
  position: absolute;
  top: 18px;
  left: 50%;
  transform: translateX(-50%);
  width: 38px;
  height: 46px;
  background: #f7dcca;
  border-radius: 20px;
}

.body {
  position: absolute;
  top: 58px;
  left: 50%;
  transform: translateX(-50%);
  width: 64px;
  height: 78px;
  background: #d8ccb6;
  border-radius: 18px 18px 10px 10px;
}

.arm,
.leg {
  position: absolute;
  background: #dccfb8;
  border-radius: 10px;
}

.arm.left {
  width: 14px;
  height: 60px;
  top: 66px;
  left: 22px;
  transform: rotate(16deg);
}

.arm.right {
  width: 14px;
  height: 58px;
  top: 68px;
  right: 22px;
  transform: rotate(-10deg);
}

.leg.left {
  width: 14px;
  height: 46px;
  top: 118px;
  left: 46px;
  transform: rotate(6deg);
}

.leg.right {
  width: 14px;
  height: 46px;
  top: 118px;
  right: 46px;
  transform: rotate(-4deg);
}

.check-list {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.check-list li {
  position: relative;
  padding-left: 26px;
  color: #68738e;
  font-size: 13px;
  line-height: 1.5;
}

.check-list li::before {
  content: '✓';
  position: absolute;
  left: 0;
  top: 0;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: linear-gradient(180deg, #79a0ff 0%, #6f7eff 100%);
  color: #fff;
  font-size: 12px;
  display: grid;
  place-items: center;
  box-shadow: 0 6px 12px rgba(111, 126, 255, 0.24);
}

@media (max-width: 1180px) {
  .workspace {
    grid-template-columns: 1fr;
  }

  .preview-panel {
    order: -1;
  }
}

@media (max-width: 900px) {
  .page {
    flex-direction: column;
    background: #f4f6fb;
  }

  .sidebar {
    width: 100%;
    border-right: none;
    border-bottom: 1px solid rgba(117, 144, 197, 0.12);
  }

  .menu {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 8px;
  }

  .menu-group {
    grid-column: span 2;
  }

  .content {
    padding: 18px;
  }

  .form-panel,
  .preview-panel {
    padding: 18px;
    border-radius: 20px;
  }

  .tips-grid,
  .voice-grid {
    grid-template-columns: 1fr 1fr;
  }
}

@media (max-width: 640px) {
  .workspace {
    gap: 16px;
  }

  .tips-grid,
  .voice-grid {
    grid-template-columns: 1fr;
  }

  .upload-box {
    min-height: 248px;
  }

  .upload-title {
    font-size: 16px;
  }

  .page-title {
    margin-bottom: 12px;
  }
}
</style>
