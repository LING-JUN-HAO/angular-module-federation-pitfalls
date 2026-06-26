---
layout: none
---

<PageNum />

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">風險：Tailwind 進入 Component Scoped CSS</h1>
  <div class="slide-subtitle">CSS 可以跟著 component JS 載入，但也會被 Angular View Encapsulation 限制</div>
</div>

<div class="content-row">

<div class="left-col">

<div class="section-card">
<div class="mini-label">機制</div>
<div class="flow-chain stack">
  <div class="flow-step">tailwind.css 放進最外層 component 的 styleUrl</div>
  <div class="flow-arrow">↓</div>
  <div class="flow-step">Angular 編進該 component 的 lazy chunk</div>
  <div class="flow-arrow">↓</div>
  <div class="flow-step note">CSS 成功進 runtime，但不是全域 CSS</div>
</div>

```ts {2}
@Component({
  styleUrl: './tailwind.css',
})
```

</div>

</div>

<div class="right-col">

  <div v-click class="section-card">
    <div class="compare-stack">
      <div class="compare-item">
        <div class="mini-label">期望</div>
        <div class="compare-text">CSS 透過 styleUrl 跟著 JS chunk 一起載入</div>
      </div>
      <div class="compare-item">
        <div class="mini-label warn-label">實際</div>
        <ul class="compare-list">
          <li>Angular 會加上 <code>_ngcontent-*</code> scope</li>
          <li>Tailwind utilities 被限制在該 component 內</li>
        </ul>
      </div>
    </div>
    <img src="/devtools-ngcontent.png" class="devtools-img devtools-img-top" />
    <div class="consequence-text">→ 子元件 / 動態內容 / 投影內容可能吃不到 Tailwind 樣式</div>
  </div>

</div>

</div>

</div>

<style>
.slide-wrap {
  display: flex;
  flex-direction: column;
  height: 100vh;
  padding: 1.8rem 2.5rem 1.3rem 2.5rem;
  box-sizing: border-box;
  overflow: hidden;
}
.slide-header { margin-bottom: 0.8rem; }
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
  grid-template-columns: minmax(0, 1fr) minmax(0, 1fr);
  gap: 2rem;
  flex: 1;
  min-height: 0;
}
.left-col {
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
  min-height: 0;
  min-width: 0;
  overflow: hidden;
}
.right-col {
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
  min-height: 0;
  min-width: 0;
  overflow: hidden;
}
.section-card {
  background: #0b1222;
  border: 1px solid #1e293b;
  border-radius: 10px;
  padding: 0.85rem 1.05rem;
}
.mini-label {
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: #94a3b8;
  margin-bottom: 0.5rem;
}
.warn-label { color: #f87171; }

.flow-chain.stack {
  display: flex;
  flex-direction: column;
  gap: 0;
  margin-bottom: 0.6rem;
}
.flow-step {
  font-size: 0.78rem;
  color: #cbd5e1;
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 6px;
  padding: 0.55rem 0.7rem;
  text-align: center;
  width: 100%;
  box-sizing: border-box;
}
.flow-step.note {
  color: #93c5fd;
  border-color: #1e3a5f;
  background: #0c1a30;
}
.flow-arrow {
  font-size: 0.8rem;
  font-weight: 700;
  color: #7dd3fc;
  text-align: center;
  padding: 0.2rem 0;
}

.section-card pre {
  font-size: 0.7rem !important;
  line-height: 1.4 !important;
  margin: 0 !important;
  border-radius: 6px !important;
  max-width: 100% !important;
}

.compare-stack {
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
  margin-bottom: 0.7rem;
}
.compare-item {
  flex: 1;
  min-width: 0;
}
.compare-text {
  font-size: 0.78rem;
  color: #cbd5e1;
  line-height: 1.55;
}
.compare-text code {
  font-family: 'Fira Code', monospace;
  font-size: 0.72rem;
  color: #a5b4fc;
}
.compare-list {
  margin: 0;
  padding: 0;
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}
.compare-list li {
  position: relative;
  padding-left: 0.85rem;
  font-size: 0.78rem;
  color: #cbd5e1;
  line-height: 1.55;
}
.compare-list li::before {
  content: "";
  position: absolute;
  left: 0;
  top: 0.45em;
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background: #64748b;
}
.compare-list code {
  font-family: 'Fira Code', monospace;
  font-size: 0.72rem;
  color: #a5b4fc;
}
.devtools-img-top {
  margin-top: 0.2rem;
}
.consequence-text {
  font-size: 0.78rem;
  font-weight: 600;
  color: #f87171;
  line-height: 1.5;
  margin-top: 0.6rem;
}
.consequence-text code {
  font-family: 'Fira Code', monospace;
  font-size: 0.72rem;
  color: #fca5a5;
}
.devtools-img {
  width: 100%;
  height: auto;
  border-radius: 6px;
  border: 1px solid #1e293b;
  display: block;
}

.warning-block {
  margin-top: 0.9rem;
  background: linear-gradient(135deg, #1e0a0a 0%, #0b1222 100%);
  border: 1px solid #7f1d1d;
  border-radius: 10px;
  padding: 0.8rem 1.1rem;
  flex-shrink: 0;
}
.result-text {
  font-size: 0.68rem;
  color: #fca5a5;
  line-height: 1.6;
}
.risk-hl {
  font-weight: 700;
  color: #f87171;
}
</style>
