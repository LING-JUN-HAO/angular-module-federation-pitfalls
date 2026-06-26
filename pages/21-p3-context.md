---
layout: none
---

<PageNum />

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">Remote 的 CSS 沒有被 Host 載入</h1>
  <div class="slide-subtitle">Tailwind CSS 已成功產生，但沒有被 remoteEntry.js 引用，因此 Host 動態載入 Remote 時拿不到樣式</div>
</div>

<div class="content-row">

<div class="left-col">

  <div class="section-card">
    <div class="mini-label">CSS 產生成功，不代表 Host 會自動載入</div>
    <div class="layer-row">
      <div class="layer-block">
        <div class="layer-label layer-ok">Build 層 — 沒問題</div>
        <div class="flow-chain stack">
          <div class="flow-step">Remote Source</div>
          <div class="flow-arrow">↓</div>
          <div class="flow-step">Tailwind 掃描 class</div>
          <div class="flow-arrow">↓</div>
          <div class="flow-step ok">styles.css 產生成功</div>
        </div>
      </div>
      <div class="layer-block">
        <div class="layer-label layer-warn">Federation 載入層 — 問題所在</div>
        <div class="flow-chain stack">
          <div class="flow-step">remoteEntry.js 只暴露 JS</div>
          <div class="flow-arrow">↓</div>
          <div class="flow-step broken">沒有引用 styles.css</div>
          <div class="flow-arrow">↓</div>
          <div class="flow-step broken">Host 載入 Remote 時<br>CSS 不會一起進來</div>
        </div>
      </div>
    </div>
  </div>

  <div class="conclusion-block">
    <span class="conclusion-text">styles.css 已經成功產生，但目前沒有被 remoteEntry.js 引用。因此 Host 動態載入 Remote JS 時，不會自動載入這份 CSS。</span>
  </div>

</div>

<div class="right-col">

  <v-click>
  <div class="section-card">
    <div class="mini-label">目標</div>
    <div class="goal-core">Remote 載入後，樣式也要跟著生效</div>
    <div class="goal-reasons">
      <div class="goal-reason">CSS 要跟著 Remote 一起被載入</div>
      <div class="goal-reason">Host 不需要手動維護 Remote 的 CSS</div>
    </div>
  </div>

  <div class="section-card mt-3">
    <div class="mini-label">解法方向：讓 Remote 入口模組主動帶上 CSS</div>
    <div class="flow-chain stack">
      <div class="flow-step">Host 動態載入 Remote JS</div>
      <div class="flow-arrow">↓</div>
      <div class="flow-step">Remote expose 的入口模組 import / 注入自己的 CSS</div>
      <div class="flow-arrow">↓</div>
      <div class="flow-step">CSS link 或 style tag 被加入 Host document</div>
      <div class="flow-arrow">↓</div>
      <div class="flow-step ok">Remote 樣式在 Host 中生效</div>
    </div>
  </div>

  </v-click>

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
  color: #94a3b8;
  margin-bottom: 0.6rem;
}
.warn-label { color: #f87171; }
.mt-3 { margin-top: 1.1rem; }
.layer-label {
  font-size: 0.56rem;
  font-weight: 700;
  letter-spacing: 0.04em;
  margin-bottom: 0.5rem;
  text-align: center;
}
.layer-ok { color: #4ade80; }
.layer-warn { color: #f87171; }
.layer-row {
  display: flex;
  gap: 1rem;
  margin-top: 0.6rem;
}
.layer-row .layer-block {
  flex: 1;
  min-width: 0;
}

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
.flow-chain.horizontal.even {
  flex-wrap: nowrap;
}
.flow-chain.horizontal.even .flow-step {
  flex: 1;
  min-width: 0;
  text-align: center;
}
.flow-chain.horizontal.even .flow-arrow {
  flex-shrink: 0;
  padding-left: 0;
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
  font-size: 0.7rem;
  font-weight: 700;
  color: #7dd3fc;
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
  color: #cbd5e1;
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
  font-size: 0.72rem;
  font-weight: 600;
  color: #fca5a5;
  line-height: 1.6;
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
  position: relative;
  font-size: 0.64rem;
  color: #94a3b8;
  line-height: 1.55;
  padding-left: 0.9rem;
}
.goal-reason::before {
  content: '•';
  position: absolute;
  left: 0;
  color: #7dd3fc;
  font-weight: 700;
}

/* 左欄放大（限定 left-col，避免影響右欄共用的 class） */
.left-col .mini-label { font-size: 0.7rem; }
.left-col .layer-label { font-size: 0.68rem; }
.left-col .flow-step { font-size: 0.8rem; }
.left-col .flow-arrow { font-size: 0.82rem; }
.conclusion-text { font-size: 0.84rem; }

</style>
