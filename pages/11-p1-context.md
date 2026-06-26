---
layout: default
---

<PageNum />

# 前情提要：Secondary Entry Points

<div class="mt-2 mb-5 text-sm" style="color: #cbd5e1;">
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
    <span class="dep-note"><code>toSignal()</code> 會建立 Observable 訂閱，因此需要在 <strong>Injection Context</strong> 中執行，讓 Angular 能透過 <code>DestroyRef</code> 在對應的元件、指令或服務銷毀時自動清理。</span>
    <span class="inject-note"><strong>Injection Context</strong> 是 Angular 用來判斷「目前要從哪個 Injector 解析依賴」的執行上下文；只有在這個上下文存在時，<code>inject()</code> 才能正確取得依賴，例如 constructor、field initializer 或 <code>runInInjectionContext()</code> 的同步範圍。</span>
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
  color: #94a3b8;
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
  color: #94a3b8;
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
  color: #94a3b8;
  font-size: 0.8rem;
}
.arrow-label {
  font-size: 0.55rem;
  font-weight: 700;
  color: #7dd3fc;
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
  color: #94a3b8;
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
  font-size: 0.8rem;
  font-weight: 700;
  color: #7dd3fc;
  margin-bottom: 0.4rem;
}
.focus-body {
  font-size: 0.8rem;
  color: #94a3b8;
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
  color: #cbd5e1;
  font-size: 0.78rem;
}
.dep-note {
  display: block;
  margin-top: 0.3rem;
  font-size: 0.72rem;
  color: #94a3b8;
}
.dep-note strong {
  color: #cbd5e1;
}
.dep-note code {
  font-family: 'Fira Code', monospace;
  color: #cbd5e1;
  font-size: 0.7rem;
}
.inject-note {
  display: block;
  margin-top: 0.5rem;
  font-size: 0.68rem;
  color: #cbd5e1;
  line-height: 2;
  border-top: 1px solid #1e293b;
  padding-top: 0.5rem;
}
.inject-note code {
  font-family: 'Fira Code', monospace;
  color: #94a3b8;
  font-size: 0.66rem;
}
</style>
