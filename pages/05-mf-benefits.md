---
layout: default
---

# 微前端解決了什麼

<div class="mt-8 grid grid-cols-3 gap-5">
  <v-clicks>

  <div class="benefit-card">
    <div class="benefit-label">開發彈性</div>
    <div class="benefit-body">
      各業務團隊可自主規劃開發方式，不需要等待或配合其他團隊的節奏
    </div>
    <div class="benefit-example">
      掛號團隊可獨立迭代功能，不受門診或批價的進度影響
    </div>
    <div class="benefit-fix">
      <span class="fix-tag">解決</span> 互相等待、阻塞部署
    </div>
  </div>

  <div class="benefit-card">
    <div class="benefit-label">獨立部署</div>
    <div class="benefit-body">
      子應用可依自身需求設定 CI/CD，獨立完成發布，無需等待其他應用
    </div>
    <div class="benefit-example">
      批價系統修了一個費用計算錯誤 bug，直接上版，不用等門診或掛號
    </div>
    <div class="benefit-fix">
      <span class="fix-tag">解決</span> 每次異動都要重新打包整個應用
    </div>
  </div>

  <div class="benefit-card">
    <div class="benefit-label">故障隔離</div>
    <div class="benefit-body">
      某個子應用異常時，影響範圍控制在該子應用內，不會拖垮整個系統
    </div>
    <div class="benefit-example">
      掛號系統發生問題，門診與批價照常運作，不互相影響
    </div>
    <div class="benefit-fix">
      <span class="fix-tag">解決</span> 功能模組邊界不清楚
    </div>
  </div>

  </v-clicks>
</div>

<v-click>
<div class="mt-6 bottom-row">
  <div class="bottom-item">
    <div class="bottom-num">↑</div>
    <div class="bottom-text">發布頻率提升</div>
  </div>
  <div class="bottom-divider"></div>
  <div class="bottom-item">
    <div class="bottom-num">↓</div>
    <div class="bottom-text">上版風險降低</div>
  </div>
  <div class="bottom-divider"></div>
  <div class="bottom-item">
    <div class="bottom-num">↑</div>
    <div class="bottom-text">團隊自主性提高</div>
  </div>
  <div class="bottom-divider"></div>
  <div class="bottom-principle">
    核心思路：讓技術架構真實反映團隊的分工方式
  </div>
</div>
</v-click>

<style>
.benefit-card {
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 10px;
  padding: 1.3rem;
  display: flex;
  flex-direction: column;
  gap: 0.65rem;
}
.benefit-label {
  font-weight: 700;
  color: #86efac;
  font-size: 1rem;
  padding-bottom: 0.4rem;
  border-bottom: 1px solid #1e293b;
}
.benefit-body {
  font-size: 0.8rem;
  color: #94a3b8;
  line-height: 1.6;
  flex: 1;
}
.benefit-example {
  font-size: 0.75rem;
  color: #475569;
  background: #0a0f1a;
  border-left: 2px solid #1e293b;
  padding: 0.4rem 0.6rem;
  border-radius: 0 4px 4px 0;
  line-height: 1.5;
}
.benefit-fix {
  font-size: 0.72rem;
  color: #475569;
}
.fix-tag {
  display: inline-block;
  background: #1e293b;
  color: #7dd3fc;
  padding: 0.1em 0.5em;
  border-radius: 4px;
  font-size: 0.65rem;
  margin-right: 0.3rem;
}

/* 概念示意圖 */
.concept-diagram {
  display: flex;
  align-items: center;
  gap: 1rem;
  background: #0a0f1a;
  border: 1px solid #1e293b;
  border-radius: 10px;
  padding: 0.8rem 1.4rem;
}
.cd-side {
  flex: 1;
  display: flex;
  align-items: center;
  gap: 0.7rem;
}
.cd-label {
  font-size: 0.65rem;
  font-weight: 700;
  white-space: nowrap;
  letter-spacing: 0.02em;
}
.cd-mono-box {
  background: #1a0f0f;
  border: 1px solid #7f1d1d;
  border-radius: 6px;
  padding: 0.35rem 0.7rem;
}
.cd-mono-inner {
  font-size: 0.72rem;
  font-weight: 600;
  color: #fca5a5;
  display: flex;
  align-items: center;
  gap: 0.3rem;
}
.cd-sep { color: #7f1d1d; }
.cd-mf-box {
  background: #071410;
  border: 1px solid #1e4a2e;
  border-radius: 6px;
  padding: 0.35rem 0.7rem;
  display: flex;
  gap: 0.5rem;
  align-items: flex-start;
}
.cd-shell {
  font-size: 0.72rem;
  font-weight: 700;
  color: #86efac;
  padding-top: 0.05rem;
}
.cd-apps { display: flex; flex-direction: column; }
.cd-app {
  font-size: 0.68rem;
  color: #4ade80;
  font-family: 'Fira Code', monospace;
  line-height: 1.55;
}
.cd-arrow {
  font-size: 0.9rem;
  color: #334155;
  flex-shrink: 0;
}
.cd-deploy {
  font-size: 0.68rem;
  font-weight: 700;
  white-space: nowrap;
}
.cd-vs {
  font-size: 0.7rem;
  color: #334155;
  font-weight: 700;
  flex-shrink: 0;
  padding: 0 0.2rem;
}
.label-bad  { color: #f87171; }
.label-good { color: #86efac; }

.bottom-row {
  display: flex;
  align-items: center;
  gap: 0;
  background: #0a0f1a;
  border: 1px solid #1e293b;
  border-radius: 10px;
  padding: 0.7rem 1.2rem;
}
.bottom-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  flex: 1;
  justify-content: center;
}
.bottom-num {
  font-size: 0.9rem;
  color: #86efac;
  font-weight: 700;
}
.bottom-text {
  font-size: 0.78rem;
  color: #64748b;
}
.bottom-divider {
  width: 1px;
  height: 1.2rem;
  background: #1e293b;
  flex-shrink: 0;
}
.bottom-principle {
  flex: 2;
  text-align: right;
  font-size: 0.78rem;
  color: #7dd3fc;
  font-style: italic;
  padding-left: 1rem;
  border-left: 1px solid #1e293b;
}
</style>
