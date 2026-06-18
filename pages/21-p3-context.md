---
layout: none
---

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">Tailwind CSS 已產生，但未隨 Module Federation 載入</h1>
  <div class="slide-subtitle">Remote 的 Tailwind CSS 確實存在，只是這份 CSS 沒有被綁進 Module Federation 的載入鏈</div>
</div>

<div class="content-row">

<div class="left-col">

  <div class="section-card">
    <div class="mini-label warn-label">問題點：Remote styles.css 有產生，但沒有被匯出</div>
    <div class="compare-row">
      <div class="compare-col">
        <div class="compare-head">Remote Build</div>
        <div class="flow-step">Remote Source</div>
        <div class="flow-step">Tailwind Scan</div>
        <div class="flow-step broken">✕ Remote styles.css</div>
      </div>
      <div class="compare-divider"></div>
      <div class="compare-col">
        <div class="compare-head">Module Federation</div>
        <div class="flow-step">Expose Route</div>
        <div class="flow-step">remoteEntry.js</div>
        <div class="flow-step ok">Host 載入</div>
      </div>
    </div>
  </div>

  <div class="conclusion-block">
    <span class="conclusion-text">Remote styles.css 為獨立產物，未被納入 Module Federation 的載入流程</span>
  </div>

</div>

<div class="right-col">

  <div class="section-card">
    <div class="mini-label">目標</div>
    <div class="goal-core">Remote 載入時樣式自動生效，並維持樣式隔離</div>
    <div class="goal-reasons">
      <div class="goal-reason">— Remote 是動態載入的，各 remote 之間應保持獨立</div>
      <div class="goal-reason">— Host 不需要、也不該掃描 remote 的 source 來補產 CSS</div>
    </div>
  </div>

  <div class="section-card mt-3">
    <div class="mini-label">思考方向：讓 CSS 跟著 remoteEntry.js 一起動態插入 host</div>
    <div class="flow-chain stack">
      <div class="flow-step">Remote 動態載入</div>
      <div class="flow-arrow">↓</div>
      <div class="flow-step">CSS 一併打包進 remoteEntry.js</div>
      <div class="flow-arrow">↓</div>
      <div class="flow-step ok">remoteEntry.js 把樣式動態插入 host</div>
    </div>
  </div>

</div>

</div>
</div>

<style>
.slide-wrap {
  display: flex;
  flex-direction: column;
  height: 100vh;
  padding: 1.6rem 2.5rem 1.1rem 2.5rem;
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
  font-size: 0.65rem;
  color: #64748b;
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
  gap: 1.1rem;
  min-height: 0;
}
.right-col {
  display: flex;
  flex-direction: column;
  min-height: 0;
  overflow: hidden;
}
.section-card {
  background: #0b1222;
  border: 1px solid #1e293b;
  border-radius: 10px;
  padding: 1.1rem 1.3rem;
}
.mini-label {
  font-size: 0.58rem;
  font-weight: 700;
  letter-spacing: 0.07em;
  text-transform: uppercase;
  color: #475569;
  margin-bottom: 0.6rem;
}
.warn-label { color: #f87171; }
.mt-3 { margin-top: 1.1rem; }

.flow-chain {
  display: flex;
  flex-direction: column;
  gap: 0;
}
.flow-chain.horizontal {
  flex-direction: row;
  align-items: center;
  flex-wrap: wrap;
  gap: 0.45rem;
}
.flow-chain.compact .flow-step { font-size: 0.6rem; }
.flow-step {
  font-size: 0.68rem;
  color: #cbd5e1;
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 6px;
  padding: 0.45rem 0.75rem;
  display: inline-block;
  align-self: flex-start;
}
.flow-step.warn { color: #f87171; border-color: #7f1d1d; background: #1e0a0a; }
.flow-step.ok { color: #4ade80; border-color: #166534; background: #052e16; }
.flow-step.broken { color: #f87171; border-color: #7f1d1d; background: #1e0a0a; }
.flow-arrow {
  font-size: 0.62rem;
  color: #475569;
  line-height: 1.3;
  padding: 0.25rem 0 0.25rem 0.85rem;
}
.flow-chain.stack .flow-step {
  width: 100%;
  box-sizing: border-box;
  align-self: stretch;
  text-align: center;
}
.flow-chain.stack .flow-arrow {
  padding-left: 0;
  text-align: center;
}

.compare-row {
  display: flex;
  gap: 1.2rem;
}
.compare-col {
  flex: 1;
  display: flex;
  flex-direction: column;
}
.compare-col .flow-step {
  margin-bottom: 0.55rem;
  width: 100%;
  box-sizing: border-box;
}
.compare-head {
  font-size: 0.6rem;
  font-weight: 700;
  color: #64748b;
  margin-bottom: 0.5rem;
}
.compare-divider {
  width: 1px;
  background: repeating-linear-gradient(180deg, #334155 0, #334155 4px, transparent 4px, transparent 8px);
}
.statement-box {
  font-size: 0.66rem;
  line-height: 1.6;
  padding: 0.55rem 0.75rem;
  border-radius: 0 4px 4px 0;
  margin-top: 0.6rem;
}
.warn-statement {
  color: #fca5a5;
  background: #1e0a0a;
  border-left: 2px solid #f87171;
}
.ok-statement {
  color: #cbd5e1;
  background: #0f172a;
  border-left: 2px solid #7dd3fc;
}

.conclusion-block {
  display: flex;
  align-items: baseline;
  flex-wrap: wrap;
  gap: 0.6rem;
  padding: 0.9rem 1.2rem;
  background: linear-gradient(135deg, #1e0a0a 0%, #0b1222 100%);
  border: 1px solid #7f1d1d;
  border-radius: 10px;
}
.conclusion-tag {
  flex-shrink: 0;
  font-size: 0.56rem;
  font-weight: 700;
  letter-spacing: 0.05em;
  color: #f87171;
  background: #2d0f0f;
  border: 1px solid #7f1d1d;
  border-radius: 4px;
  padding: 0.25em 0.6em;
}
.conclusion-text {
  font-size: 0.78rem;
  font-weight: 600;
  color: #fca5a5;
  white-space: nowrap;
  line-height: 1.55;
}

.goal-core {
  font-size: 0.8rem;
  font-weight: 700;
  color: #f1f5f9;
  line-height: 1.55;
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 8px;
  padding: 0.6rem 0.85rem;
}
.goal-reasons {
  display: flex;
  flex-direction: column;
  gap: 0.45rem;
  margin-top: 0.7rem;
  padding-left: 0.3rem;
}
.goal-reason {
  font-size: 0.64rem;
  color: #94a3b8;
  line-height: 1.55;
}


</style>
