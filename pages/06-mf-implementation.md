---
layout: default
---

<PageNum />

# 微前端怎麼實現？

<div class="body-wrap">
  <div class="intro-line">
    <div>常見的整合方式有 iframe、Web Components、Single SPA、Runtime Dynamic Loading 等，核心都在處理同一件事：<span class="accent">如何將多個獨立前端應用組合成一個完整系統，同時維持各自的開發與部署邊界</span>。</div>
    <div class="intro-line-gap">而 <span class="accent">Webpack Module Federation</span> 更進一步，能在 <span class="accent">Runtime</span> 階段協商依賴版本，讓多個應用在版本相容時共享同一份模組實例，避免重複初始化與執行。</div>
  </div>
  <div class="wmf-card">
    <div class="wmf-header">
      <span class="wmf-name">Webpack Module Federation</span>
      <span class="wmf-tag">本次主角</span>
    </div>
    <div class="wmf-body">
      <div class="dep-col">
        <div class="dep-col-label label-bad">Share Nothing</div>
        <div class="dep-apps">
          <div class="dep-app">
            <div class="dep-app-name">主應用(Host)</div>
            <div class="dep-pkg pkg-red">Angular core</div>
            <div class="dep-pkg pkg-red">RxJS</div>
            <div class="dep-pkg pkg-red">PrimeNG</div>
          </div>
          <div class="dep-app">
            <div class="dep-app-name">子應用 A(Remote)</div>
            <div class="dep-pkg pkg-red">Angular core</div>
            <div class="dep-pkg pkg-red">RxJS</div>
            <div class="dep-pkg pkg-red">PrimeNG</div>
          </div>
          <div class="dep-app">
            <div class="dep-app-name">子應用 B(Remote)</div>
            <div class="dep-pkg pkg-red">Angular core</div>
            <div class="dep-pkg pkg-red">RxJS</div>
            <div class="dep-pkg pkg-red">PrimeNG</div>
          </div>
        </div>
        <div class="dep-result dep-bad">重複打包、重複載入</div>
      </div>
      <div class="wmf-divider"></div>
      <div class="dep-col">
        <div class="dep-col-label label-good">Shared Dependencies</div>
        <div class="dep-mf">
          <div class="dep-app mf-host">
            <div class="dep-app-name">主應用(Host)</div>
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
              <div class="dep-app-name">子應用 A(Remote)</div>
            </div>
            <div class="dep-app mf-remote">
              <div class="dep-app-name">子應用 B(Remote)</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
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
  font-size: 1rem;
  color: #cbd5e1;
  line-height: 1.6;
  flex-shrink: 0;
}
.intro-line-gap {
  margin-top: 0.5rem;
}
.wmf-bridge {
  font-size: 0.75rem;
  color: #cbd5e1;
  line-height: 1.5;
  flex-shrink: 0;
}
.bridge-hl {
  color: #7dd3fc;
  font-weight: 600;
}
.wmf-card {
  flex: 0 0 auto;
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
  font-size: 1rem;
  color: #e2e8f0;
}
.wmf-tag {
  font-size: 0.68rem;
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
  align-items: flex-start;
}
.wmf-divider {
  width: 1px;
  background: #1e3a5f;
  flex-shrink: 0;
  align-self: stretch;
}
.dep-col {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
  min-height: 0;
}
.dep-col-label {
  font-size: 0.75rem;
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
  align-items: flex-start;
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
  font-size: 0.78rem;
  font-weight: 700;
  color: #94a3b8;
  padding-bottom: 0.2rem;
  border-bottom: 1px solid #1e293b;
  margin-bottom: 0.1rem;
}
.dep-pkg {
  font-size: 0.72rem;
  border-radius: 3px;
  padding: 0.1em 0.4em;
  text-align: center;
}
.pkg-red   { background: #2d0f0f; color: #f87171; border: 1px solid #7f1d1d; }
.pkg-green { background: #052e16; color: #86efac; border: 1px solid #14532d; }
.dep-result {
  font-size: 0.8rem;
  font-weight: 600;
  text-align: center;
  padding: 0.3em 0;
  flex-shrink: 0;
}
.dep-bad  { color: #f87171; }
.dep-good { color: #86efac; }
.dep-mf {
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
  font-size: 0.68rem;
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
  font-size: 0.72rem;
}
</style>
