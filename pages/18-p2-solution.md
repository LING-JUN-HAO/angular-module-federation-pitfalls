---
layout: none
---

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">處理方式：關閉 <code>concatenateModules</code></h1>
  <div class="slide-subtitle">Production build 發生 shared module 初始化順序問題時，可用此方式降低風險</div>
</div>

<div class="block-stack">

<div class="info-block">
  <div class="info-label label-bad">問題</div>
  <div class="info-text">Webpack 可能因為 module 合併，讓 shared module 的初始化執行順序較難預期，可能出現：</div>

```text
Class extends value undefined
```

</div>

<v-click>
<div class="info-block">
  <div class="info-label label-fix">處理方式</div>

```ts {2}
optimization: {
  concatenateModules: false,
}
```

</div>
</v-click>

<v-click>
<div class="tl-ruling">
  取捨：穩定性提高，但 bundle 可能稍微變大
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
.slide-header { margin-bottom: 0.9rem; }
.slide-title {
  font-size: 2rem;
  font-weight: 800;
  color: #f1f5f9;
  line-height: 1.25;
  margin: 0 0 0.4rem 0;
}
.slide-title code {
  font-family: 'Fira Code', monospace;
  font-size: 1.5rem;
  color: #86efac;
}
.slide-subtitle {
  font-size: 0.7rem;
  color: #cbd5e1;
  line-height: 1.6;
}
.block-stack {
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
  flex: 1;
  min-height: 0;
  justify-content: flex-start;
}
.info-block {
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 10px;
  padding: 0.7rem 1.1rem;
  flex-shrink: 0;
}
.info-label {
  font-size: 0.62rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  margin-bottom: 0.5rem;
}
.label-bad { color: #f87171; }
.label-fix { color: #4ade80; }
.info-text {
  font-size: 0.78rem;
  color: #cbd5e1;
  line-height: 1.6;
  margin-bottom: 0.5rem;
}
.info-block pre {
  font-size: 0.7rem !important;
  line-height: 1.5 !important;
  margin: 0 !important;
  border-radius: 6px !important;
}
.tl-ruling {
  font-size: 0.78rem;
  font-weight: 600;
  color: #e2e8f0;
  line-height: 1.65;
  padding: 0.55rem 1rem;
  background: #0f172a;
  border-left: 3px solid #7dd3fc;
  border-radius: 0 8px 8px 0;
  flex-shrink: 0;
}
</style>
