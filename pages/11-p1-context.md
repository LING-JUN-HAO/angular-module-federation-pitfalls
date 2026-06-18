---
layout: default
---

# 前情提要：Secondary Entry Points

<div class="mt-2 mb-5 text-sm" style="color: #64748b;">
  大型套件常透過多個子路徑提供功能，開發者可按需匯入特定模組
</div>

<div class="grid-wrap">

<div class="pkg-card">
  <div class="pkg-label">Primary Entry Point</div>
  <code class="pkg-name">@angular/core</code>
  <div class="pkg-desc">負責整個 Angular 的 runtime：DI 系統、Component 渲染生命週期、裝飾器的運行時支援</div>
</div>

<div class="arrow-col">
  <div class="arrow-line"></div>
  <div class="arrow-label">sub-path</div>
</div>

<div class="sep-col">
  <div class="sep-entry sep-highlight">
    <code>@angular/core/rxjs-interop</code>
    <span class="sep-tag highlight">Secondary Entry Point</span>
  </div>
  <div class="sep-entry sep-highlight">
    <code>@angular/core/testing</code>
    <span class="sep-tag highlight">Secondary Entry Point</span>
  </div>
</div>

</div>

<v-click>
<div class="focus-box mt-6">
  <div class="focus-title">本次踩坑主角：<code>@angular/core/rxjs-interop</code></div>
  <div class="focus-body">
    <div class="body-line">提供 <strong>RxJS ↔ Angular Signals</strong> 的橋接函數：</div>
    <div class="body-line"><code>toSignal(observable)</code> — Observable 轉 Signal</div>
    <div class="body-line"><code>toObservable(signal)</code> — Signal 轉 Observable</div>
    <span class="dep-note"><code>toSignal()</code> 內部呼叫 <code>inject()</code>，<strong>必須在 Injection Context 下執行</strong>，且依賴 <code>@angular/core</code> 的 DI 實例。</span>
    <span class="inject-note">Injection Context 是 Angular 執行時追蹤「目前在哪個 Injector 節點」的狀態。<code>inject()</code> 被呼叫時讀取這個狀態來解析依賴，有效的位置只有：<code>constructor()</code>、field initializer、以及 <code>runInInjectionContext()</code> 包起來的範圍。</span>
  </div>
</div>
</v-click>

<style>
.grid-wrap {
  display: flex;
  align-items: center;
  gap: 0;
}
.pkg-card {
  background: #0f172a;
  border: 1px solid #334155;
  border-radius: 8px;
  padding: 1rem 1.2rem;
  min-width: 180px;
  flex-shrink: 0;
}
.pkg-label {
  font-size: 0.52rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #475569;
  margin-bottom: 0.4rem;
}
.pkg-name {
  display: block;
  font-size: 0.82rem;
  color: #7dd3fc;
  font-family: 'Fira Code', monospace;
  margin-bottom: 0.4rem;
}
.pkg-desc {
  font-size: 0.6rem;
  color: #475569;
  line-height: 1.5;
}
.arrow-col {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 0.8rem;
  gap: 0.2rem;
}
.arrow-line {
  width: 48px;
  height: 1px;
  background: #334155;
  position: relative;
}
.arrow-line::after {
  content: '›';
  position: absolute;
  right: -6px;
  top: -10px;
  color: #475569;
  font-size: 0.8rem;
}
.arrow-label {
  font-size: 0.5rem;
  color: #334155;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}
.sep-col {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  flex: 1;
}
.sep-entry {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 6px;
  padding: 0.4rem 0.8rem;
}
.sep-entry code {
  font-size: 0.72rem;
  color: #a5b4fc;
  font-family: 'Fira Code', monospace;
  flex: 1;
}
.sep-entry.sep-highlight {
  border-color: #1e3a5f;
  background: #0b1628;
}
.sep-tag {
  font-size: 0.48rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #475569;
  background: #1e293b;
  border-radius: 3px;
  padding: 0.1em 0.45em;
  flex-shrink: 0;
}
.sep-tag.highlight {
  color: #7dd3fc;
  background: #0c1a30;
  border: 1px solid #1e4a6e;
}
.focus-box {
  background: #0b1628;
  border: 1px solid #1e3a5f;
  border-left: 3px solid #7dd3fc;
  border-radius: 6px;
  padding: 0.8rem 1.1rem;
}
.focus-title {
  font-size: 0.72rem;
  font-weight: 700;
  color: #7dd3fc;
  margin-bottom: 0.4rem;
}
.focus-body {
  font-size: 0.72rem;
  color: #64748b;
  line-height: 1.8;
}
.body-line {
  margin-bottom: 0.5rem;
}
.focus-body strong {
  color: #94a3b8;
}
.focus-body code {
  font-family: 'Fira Code', monospace;
  color: #a5b4fc;
  font-size: 0.7rem;
}
.dep-note {
  display: block;
  margin-top: 0.3rem;
  font-size: 0.66rem;
  color: #7dd3fc;
}
.dep-note code {
  color: #7dd3fc;
}
.inject-note {
  display: block;
  margin-top: 0.5rem;
  font-size: 0.62rem;
  color: #475569;
  line-height: 1.7;
  border-top: 1px solid #1e293b;
  padding-top: 0.5rem;
}
.inject-note code {
  font-family: 'Fira Code', monospace;
  color: #64748b;
  font-size: 0.6rem;
}
</style>
