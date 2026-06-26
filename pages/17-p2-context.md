---
layout: none
---

<PageNum />

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">Webpack 降版修補漏洞後，Production Build 發生 Runtime Error</h1>
  <div class="slide-subtitle">Remote 的 webpack 從 5.105.0 降至 5.104.1 後，Development mode 啟動正常，但 Production mode 在 runtime 出錯</div>
</div>

<div class="content-row">

<div class="left-col">

  <div class="tl-item">
    <div class="tl-step">
      <div class="tl-num">1</div>
      <div class="tl-line"></div>
    </div>
    <div class="tl-content">
      <div class="tl-label">背景</div>
      <div class="tl-body">OSV Scanner 掃到 webpack <code>5.105.0</code> 有已知漏洞，透過 <code>pnpm overrides</code> 將 webpack 強制鎖版至 <code>5.104.1</code>。</div>
    </div>
  </div>

  <div class="tl-item">
    <div class="tl-step">
      <div class="tl-num">2</div>
      <div class="tl-line"></div>
    </div>
    <div class="tl-content">
      <div class="tl-label">現象</div>
      <div class="tl-body">降版後驗證 Remote Application：</div>
      <div class="tl-env-row">
        <div class="env-item ok">
          <span class="env-mode">Development mode</span>
          <span class="env-result ok-text">啟動正常</span>
        </div>
        <div class="env-item fail">
          <span class="env-mode">Production mode</span>
          <span class="env-result fail-text">Runtime error</span>
        </div>
      </div>
    </div>
  </div>

  <div class="tl-item">
    <div class="tl-step">
      <div class="tl-num">3</div>
      <div class="tl-line"></div>
    </div>
    <div class="tl-content">
      <div class="tl-label">核心問題</div>
      <div class="tl-body">同樣只差一個 patch 版本，為什麼只有 production build 會失敗？</div>
    </div>
  </div>

  <div class="tl-item last">
    <div class="tl-step">
      <div class="tl-num">4</div>
    </div>
    <div class="tl-content">
      <div class="tl-label">推測方向</div>
      <div class="tl-body">可能與 production mode 的最佳化流程有關<br>
        <span class="tl-tags">
          <span class="tl-tag">minify</span>
          <span class="tl-tag">tree-shaking</span>
          <span class="tl-tag">module concatenation</span>
        </span>
      </div>
    </div>
  </div>

</div>

<v-click>
<div class="right-col">
  <div class="img-label">錯誤只出現在 production bundle 執行階段</div>
  <img src="/p2-runtime-error.png" class="error-img" />
  <div class="error-caption">
    <code>TypeError: Class extends value undefined is not a constructor or null</code>
  </div>
  <div class="tl-ruling">排除多個疑似相關依賴後，問題仍然存在，初步判斷根因不在單一套件，而是更底層的打包或共享機制。</div>
</div>
</v-click>

</div>
</div>

<style>
.slide-wrap {
  display: flex;
  flex-direction: column;
  height: 100vh;
  padding: 2rem 2.5rem 1.5rem 2.5rem;
  box-sizing: border-box;
  overflow: hidden;
}
.slide-header {
  margin-bottom: 1.1rem;
}
.slide-title {
  font-size: 1.75rem;
  font-weight: 800;
  color: #f1f5f9;
  line-height: 1.25;
  margin: 0 0 0.4rem 0;
}
.slide-subtitle {
  font-size: 0.8rem;
  color: #cbd5e1;
  line-height: 1.6;
}
.content-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  flex: 1;
  min-height: 0;
}
.left-col {
  display: flex;
  flex-direction: column;
}
.right-col {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  overflow: hidden;
}
.tl-item {
  display: flex;
  gap: 1rem;
  align-items: stretch;
}
.tl-step {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex-shrink: 0;
  width: 1.8rem;
}
.tl-num {
  width: 1.8rem;
  height: 1.8rem;
  border-radius: 50%;
  background: #1e293b;
  border: 1px solid #334155;
  color: #94a3b8;
  font-size: 0.72rem;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
.tl-line {
  flex: 1;
  width: 1px;
  background: #1e293b;
  margin: 0.25rem 0;
}
.tl-content {
  padding-bottom: 1.2rem;
  flex: 1;
}
.tl-item.last .tl-content {
  padding-bottom: 0;
}
.tl-label {
  font-size: 0.64rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #94a3b8;
  margin-bottom: 0.3rem;
  margin-top: 0.15rem;
}
.tl-body {
  font-size: 0.88rem;
  color: #94a3b8;
  line-height: 1.7;
}
.tl-ruling {
  font-size: 0.78rem;
  color: #94a3b8;
  line-height: 1.7;
  padding: 0.4rem 0.7rem;
  background: #0f172a;
  border-left: 2px solid #475569;
  border-radius: 0 4px 4px 0;
}
.tl-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.35rem;
  margin-top: 0.4rem;
}
.tl-tag {
  font-family: 'Fira Code', monospace;
  font-size: 0.7rem;
  color: #94a3b8;
  background: #1e293b;
  border: 1px solid #334155;
  border-radius: 4px;
  padding: 0.15em 0.6em;
}
.tl-body code {
  font-family: 'Fira Code', monospace;
  font-size: 0.82rem;
  color: #a5b4fc;
}
.tl-env-row {
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
  margin-top: 0.4rem;
}
.env-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 0.9rem;
  border-radius: 5px;
  width: 19rem;
}
.env-item.ok   { background: #030f07; border: 1px solid #14532d; }
.env-item.fail { background: #150808; border: 1px solid #7f1d1d; }
.env-mode {
  font-family: 'Fira Code', monospace;
  font-size: 0.78rem;
  color: #cbd5e1;
  width: 11rem;
}
.ok-text   { font-size: 0.8rem; color: #4ade80; font-weight: 600; }
.fail-text { font-size: 0.8rem; color: #f87171; font-weight: 600; }
.img-label {
  font-size: 0.64rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #94a3b8;
  border-bottom: 1px solid #1e293b;
  padding-bottom: 0.35rem;
}
.error-img {
  width: 100%;
  height: auto;
  border-radius: 6px;
  display: block;
}
.error-caption {
  font-size: 0.72rem;
  color: #cbd5e1;
  line-height: 1.6;
}
.error-caption code {
  font-family: 'Fira Code', monospace;
  font-size: 0.7rem;
  color: #f87171;
}
</style>
