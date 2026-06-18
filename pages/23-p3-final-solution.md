---
layout: none
---

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">解決方案：加上 <code>ViewEncapsulation.None</code></h1>
  <div class="slide-subtitle">讓 scoped css 本身變成全域樣式，巢狀的內層組件也能吃到外層的 Tailwind class</div>
</div>

<div class="content-row">

<div class="left-col">

  <div class="section-card">
    <div class="mini-label">機制</div>

<div class="mechanism-text">

在最外層 component 加上 <code>encapsulation: ViewEncapsulation.None</code>，Angular 編譯時就不會再幫這份樣式加上 <code>_ngcontent-*</code> scope，原本被 scope 住的 CSS 規則會變成單純的全域 CSS。

</div>

```ts {5}
@Component({
  selector: 'app-main-layout',
  imports: [...],
  styleUrl: './tailwind.css',
  encapsulation: ViewEncapsulation.None,
  templateUrl: './main-layout.component.html',
})
```

  </div>

  <div class="section-card mt-sm">
    <div class="mini-label warn-label">代價</div>
    <div class="tradeoff-row">
      <div class="tradeoff-col">
        <div class="tradeoff-head ok-text">得到</div>
        <div class="tradeoff-item ok-item">✓ 巢狀元件可直接使用 Tailwind</div>
      </div>
      <div class="tradeoff-col">
        <div class="tradeoff-head warn-label">失去</div>
        <div class="tradeoff-item warn-item">✗ CSS 變成 global CSS</div>
        <div class="tradeoff-item warn-item">✗ 無法再依靠 Angular scope 隔離</div>
        <div class="tradeoff-item warn-item">✗ 不同 remote 可能互相影響</div>
      </div>
    </div>
  </div>

</div>

<div class="right-col">

  <div class="section-card">
    <div class="mini-label">為什麼有效</div>
    <div class="effect-flow">
      <div class="effect-node">MainLayoutComponent<br><span class="effect-sub">(ViewEncapsulation.None)</span></div>
      <div class="effect-arrow">↓</div>
      <div class="effect-node global"><code>.flex</code> <code>.mt-4</code> <code>.bg-primary</code><br><span class="effect-sub">變成 global CSS</span></div>
      <div class="effect-arrow">↓</div>
      <div class="effect-node ok">HomeComponent / PlaceComponent / DialogComponent ...<br><span class="effect-sub">全部可存取</span></div>
    </div>
    <div class="result-text ok-text">Tailwind utilities 不再被 <code>_ngcontent-*</code> 限制。</div>
  </div>

</div>

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
.slide-header { margin-bottom: 0.8rem; }
.slide-title {
  font-size: 1.75rem;
  font-weight: 800;
  color: #f1f5f9;
  line-height: 1.25;
  margin: 0 0 0.4rem 0;
}
.slide-title code {
  font-family: 'Fira Code', monospace;
  font-size: 1.3rem;
  color: #86efac;
}
.slide-subtitle {
  font-size: 0.65rem;
  color: #64748b;
  line-height: 1.6;
}
.content-row {
  display: flex;
  flex-direction: row;
  align-items: stretch;
  gap: 2rem;
  flex: 1;
  min-height: 0;
}
.left-col {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
  min-height: 0;
  min-width: 0;
  overflow: hidden;
}
.right-col {
  flex: 1;
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
  font-size: 0.58rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: #475569;
  margin-bottom: 0.5rem;
}
.warn-label { color: #f87171; }
.mt-sm { margin-top: 0.7rem; }

.mechanism-text {
  font-size: 0.62rem;
  color: #94a3b8;
  line-height: 1.6;
  margin-bottom: 0.5rem;
}
.section-card pre {
  font-size: 0.5rem !important;
  line-height: 1.4 !important;
  margin: 0 !important;
  border-radius: 6px !important;
  max-width: 100% !important;
}
.mechanism-text code {
  font-family: 'Fira Code', monospace;
  font-size: 0.56rem;
  color: #a5b4fc;
}


.effect-flow {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  margin-bottom: 0.6rem;
}
.effect-node {
  width: 100%;
  box-sizing: border-box;
  text-align: center;
  font-size: 0.62rem;
  color: #cbd5e1;
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 6px;
  padding: 0.5rem 0.7rem;
  line-height: 1.5;
}
.effect-node code {
  font-family: 'Fira Code', monospace;
  font-size: 0.58rem;
  color: #a5b4fc;
}
.effect-node.global { border-color: #b45309; background: #1c1206; }
.effect-node.global code { color: #fde68a; }
.effect-node.ok { border-color: #166534; background: #052e16; color: #4ade80; }
.effect-sub {
  font-size: 0.5rem;
  color: #64748b;
}
.effect-arrow {
  font-size: 0.65rem;
  color: #475569;
  line-height: 1.4;
}

.result-text {
  font-size: 0.62rem;
  color: #fca5a5;
  line-height: 1.6;
}
.result-text code {
  font-family: 'Fira Code', monospace;
  font-size: 0.56rem;
  color: #f87171;
}
.result-text.ok-text {
  color: #94a3b8;
}
.result-text.ok-text code {
  color: #a5b4fc;
}

.tradeoff-row {
  display: flex;
  gap: 1rem;
}
.tradeoff-col {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}
.tradeoff-head {
  font-size: 0.56rem;
  font-weight: 700;
  letter-spacing: 0.05em;
}
.ok-text { color: #4ade80; }
.tradeoff-item {
  font-size: 0.6rem;
  line-height: 1.55;
}
.ok-item { color: #94a3b8; }
.warn-item { color: #fca5a5; }
</style>
