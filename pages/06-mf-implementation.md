---
layout: default
---

# 微前端怎麼實現？

<div class="body-wrap">
  <div class="intro-line">
    常見的實現方式有四種，核心都在處理同一件事：<span class="accent">如何讓多個子應用整合在同一個頁面，同時維持彼此的隔離性</span>
  </div>
  <div class="other-methods">
    <v-clicks>
    <div class="method-card">
      <div class="method-name">iframe</div>
      <div class="method-pro"><span class="label-pro">優</span>完全隔離、技術棧不限</div>
      <div class="method-con"><span class="label-con">缺</span>路由同步困難</div>
      <div class="method-con"><span class="label-con">缺</span>跨框架溝通成本高</div>
    </div>
    <div class="method-card">
      <div class="method-name">Web Components</div>
      <div class="method-pro"><span class="label-pro">優</span>原生瀏覽器標準</div>
      <div class="method-con"><span class="label-con">缺</span>封裝與整合成本較高</div>
      <div class="method-con"><span class="label-con">缺</span>共用依賴需自行設計</div>
    </div>
    <div class="method-card">
      <div class="method-name">Single SPA</div>
      <div class="method-pro"><span class="label-pro">優</span>完整微前端框架</div>
      <div class="method-con"><span class="label-con">缺</span>需遵守生命週期規範</div>
      <div class="method-con"><span class="label-con">缺</span>架構侵入性較高</div>
    </div>
    <div class="method-card">
      <div class="method-name">Runtime Dynamic Loading</div>
      <div class="method-pro"><span class="label-pro">優</span>載入邏輯完全自訂</div>
      <div class="method-con"><span class="label-con">缺</span>依賴協商需自行實作</div>
      <div class="method-con"><span class="label-con">缺</span>Runtime 問題難以除錯</div>
    </div>
    </v-clicks>
  </div>

  <v-click>
  <div class="intro-line">
    而 <span class="accent">Webpack Module Federation</span> 更進一步，能在 <span class="accent">Runtime</span> 階段協商依賴版本，讓多個子應用共享同一套件實例，避免重複載入。
  </div>
  <div class="wmf-card">
    <div class="wmf-header">
      <span class="wmf-name">Webpack Module Federation</span>
      <span class="wmf-tag">本次主角</span>
    </div>
    <div class="wmf-body">
      <div class="dep-col">
        <div class="dep-col-label label-bad">傳統方式</div>
        <div class="dep-apps">
          <div class="dep-app">
            <div class="dep-app-name">主應用</div>
            <div class="dep-pkg pkg-red">Angular core</div>
            <div class="dep-pkg pkg-red">RxJS</div>
            <div class="dep-pkg pkg-red">PrimeNG</div>
          </div>
          <div class="dep-app">
            <div class="dep-app-name">子應用 A</div>
            <div class="dep-pkg pkg-red">Angular core</div>
            <div class="dep-pkg pkg-red">RxJS</div>
            <div class="dep-pkg pkg-red">PrimeNG</div>
          </div>
          <div class="dep-app">
            <div class="dep-app-name">子應用 B</div>
            <div class="dep-pkg pkg-red">Angular core</div>
            <div class="dep-pkg pkg-red">RxJS</div>
            <div class="dep-pkg pkg-red">PrimeNG</div>
          </div>
        </div>
        <div class="dep-result dep-bad">重複打包、重複下載</div>
      </div>
      <div class="wmf-divider"></div>
      <div class="dep-col">
        <div class="dep-col-label label-good">Module Federation</div>
        <div class="dep-mf">
          <div class="dep-app mf-host">
            <div class="dep-app-name">主應用</div>
            <div class="dep-pkg pkg-green">Angular core</div>
            <div class="dep-pkg pkg-green">RxJS</div>
            <div class="dep-pkg pkg-green">PrimeNG</div>
          </div>
          <div class="shared-row">
            <div class="shared-line"></div>
            <div class="shared-badge">Shared</div>
            <div class="shared-line"></div>
          </div>
          <div class="mf-remotes">
            <div class="dep-app mf-remote">
              <div class="dep-app-name">子應用 A</div>
            </div>
            <div class="dep-app mf-remote">
              <div class="dep-app-name">子應用 B</div>
            </div>
          </div>
        </div>
        <div class="dep-result dep-good">共用依賴，只載入一次</div>
      </div>
    </div>
  </div>
  </v-click>
</div>

<style>
.body-wrap {
  height: calc(100% - 4.5rem);
  display: flex;
  flex-direction: column;
  gap: 0.45rem;
  margin-top: 0.5rem;
}
.intro-line {
  font-size: 0.78rem;
  color: #64748b;
  line-height: 1.5;
  flex-shrink: 0;
}
.wmf-bridge {
  font-size: 0.75rem;
  color: #64748b;
  line-height: 1.5;
  flex-shrink: 0;
}
.bridge-hl {
  color: #7dd3fc;
  font-weight: 600;
}
.other-methods {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0.6rem;
  flex-shrink: 0;
}
.method-card {
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 8px;
  padding: 0.5rem 0.7rem;
  display: flex;
  flex-direction: column;
  gap: 0.28rem;
}
.method-name {
  font-weight: 700;
  font-size: 0.72rem;
  color: #94a3b8;
  padding-bottom: 0.25rem;
  border-bottom: 1px solid #1e293b;
}
.method-pro {
  font-size: 0.65rem;
  color: #86efac;
  line-height: 1.4;
  display: flex;
  align-items: baseline;
  gap: 0.3rem;
}
.method-con {
  font-size: 0.65rem;
  color: #f87171;
  line-height: 1.4;
  display: flex;
  align-items: baseline;
  gap: 0.3rem;
}
.label-pro {
  font-size: 0.52rem;
  font-weight: 700;
  background: #052e16;
  color: #86efac;
  border: 1px solid #14532d;
  border-radius: 3px;
  padding: 0.05em 0.35em;
  flex-shrink: 0;
}
.label-con {
  font-size: 0.52rem;
  font-weight: 700;
  background: #2d0f0f;
  color: #f87171;
  border: 1px solid #7f1d1d;
  border-radius: 3px;
  padding: 0.05em 0.35em;
  flex-shrink: 0;
}
.wmf-card {
  flex: 1;
  min-height: 0;
  overflow: hidden;
  background: #071e38;
  border: 1px solid #1e4a6e;
  border-radius: 10px;
  padding: 0.6rem 1.2rem;
  display: flex;
  flex-direction: column;
}
.wmf-header {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  margin-bottom: 0.4rem;
  flex-shrink: 0;
}
.wmf-name {
  font-weight: 700;
  font-size: 0.9rem;
  color: #e2e8f0;
}
.wmf-tag {
  font-size: 0.58rem;
  font-weight: 700;
  background: #1e3a5f;
  color: #7dd3fc;
  padding: 0.1em 0.55em;
  border-radius: 4px;
  letter-spacing: 0.04em;
}
.wmf-body {
  display: flex;
  gap: 1.5rem;
  flex: 1;
  min-height: 0;
}
.wmf-divider {
  width: 1px;
  background: #1e3a5f;
  flex-shrink: 0;
}
.dep-col {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
  min-height: 0;
}
.dep-col-label {
  font-size: 0.62rem;
  font-weight: 700;
  letter-spacing: 0.07em;
  text-transform: uppercase;
  flex-shrink: 0;
}
.label-bad  { color: #f87171; }
.label-good { color: #86efac; }
.dep-apps {
  display: flex;
  gap: 0.5rem;
  flex: 1;
}
.dep-app {
  flex: 1;
  border-radius: 6px;
  padding: 0.45rem 0.5rem;
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  border: 1px solid #1e293b;
  background: #0f172a;
}
.dep-app-name {
  font-size: 0.63rem;
  font-weight: 700;
  color: #94a3b8;
  padding-bottom: 0.2rem;
  border-bottom: 1px solid #1e293b;
  margin-bottom: 0.1rem;
}
.dep-pkg {
  font-size: 0.58rem;
  border-radius: 3px;
  padding: 0.1em 0.4em;
  text-align: center;
}
.pkg-red   { background: #2d0f0f; color: #f87171; border: 1px solid #7f1d1d; }
.pkg-green { background: #052e16; color: #86efac; border: 1px solid #14532d; }
.dep-result {
  font-size: 0.65rem;
  font-weight: 600;
  text-align: center;
  padding: 0.3em 0;
  flex-shrink: 0;
}
.dep-bad  { color: #f87171; }
.dep-good { color: #86efac; }
.dep-mf {
  flex: 1;
  min-height: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.25rem;
}
.mf-host {
  width: 100%;
}
.shared-row {
  display: flex;
  align-items: center;
  width: 65%;
  gap: 0.3rem;
  flex-shrink: 0;
}
.shared-line {
  flex: 1;
  height: 1px;
  background: #3b82f6;
}
.shared-badge {
  font-size: 0.55rem;
  font-weight: 700;
  color: #7dd3fc;
  background: #1e3a5f;
  border: 1px solid #3b82f6;
  border-radius: 4px;
  padding: 0.15em 0.5em;
  white-space: nowrap;
  letter-spacing: 0.04em;
}
.mf-remotes {
  display: flex;
  gap: 0.5rem;
  width: 100%;
}
.mf-remote {
  flex: 1;
}
.mf-remote.dep-app {
  padding: 0.25rem 0.4rem;
}
.mf-remote .dep-app-name {
  border-bottom: none;
  text-align: center;
  padding-bottom: 0;
  margin-bottom: 0;
  font-size: 0.58rem;
}
</style>
