---
layout: default
---

# 微前端是什麼？

<div class="mt-4 text-lg leading-relaxed" style="color: #cbd5e1; max-width: 720px;">
  將單體前端應用拆分為多個可<span class="accent">獨立開發、部署與運行</span>的子應用，
  對使用者而言是一個完整的網站，對開發者而言是各自解耦、自主掌控的架構。
</div>

<div class="mt-5 grid grid-cols-3 gap-4">
  <v-click>
  <div class="concept-card">
    <div class="concept-title">概念來源</div>
    <div class="concept-desc">借鑑微服務（Microservices）架構，以功能（vertical slice）為單位劃分責任</div>
  </div>
  </v-click>
  <v-click>
  <div class="concept-card">
    <div class="concept-title">組織對齊</div>
    <div class="concept-desc">讓門診團隊、批價團隊、掛號團隊各自擁有端到端的開發與部署能力</div>
  </div>
  </v-click>
  <v-click>
  <div class="concept-card">
    <div class="concept-title">獨立部署</div>
    <div class="concept-desc">批價模組更新不影響門診，掛號上版不需等其他功能，各自獨立發布</div>
  </div>
  </v-click>
</div>

<v-click>
<div class="mt-5 compare-wrap">

  <!-- 左：單體 -->
  <div class="compare-side">
    <div class="compare-label label-bad">單體前端(before)</div>
    <div class="mono-box">
      <div class="mono-solid">
        <span>掛號系統</span>
        <span>門診系統</span>
        <span>批價系統</span>
      </div>
      <div class="mono-note">⚠ 緊耦合，共同 Repo、共同 build、共同部署</div>
    </div>
  </div>

  <!-- 中：箭頭 -->
  <div class="compare-arrow">
    <div class="arrow-icon">→</div>
    <div class="arrow-label">拆分</div>
  </div>

  <!-- 右：微前端 -->
  <div class="compare-side">
    <div class="compare-label label-good">微前端(after)</div>
    <div class="container-box">
      <div class="container-label">
        主應用（容器）
        <span class="container-sub">負責組合與導覽</span>
      </div>
      <div class="mf-box">
        <div class="mf-block block-a">
          <div class="mf-name">掛號系統</div>
          <div class="mf-badges">
            <span class="mf-team-tag">掛號團隊</span>
            <span class="deploy-badge">獨立 Repo</span>
            <span class="deploy-badge">獨立部署</span>
          </div>
        </div>
        <div class="mf-block block-b">
          <div class="mf-name">門診系統</div>
          <div class="mf-badges">
            <span class="mf-team-tag">門診團隊</span>
            <span class="deploy-badge">獨立 Repo</span>
            <span class="deploy-badge">獨立部署</span>
          </div>
        </div>
        <div class="mf-block block-c">
          <div class="mf-name">批價系統</div>
          <div class="mf-badges">
            <span class="mf-team-tag">批價團隊</span>
            <span class="deploy-badge">獨立 Repo</span>
            <span class="deploy-badge">獨立部署</span>
          </div>
        </div>
      </div>
    </div>
  </div>

</div>
</v-click>

<style>
.concept-card {
  background: #1e293b;
  border: 1px solid #334155;
  border-radius: 10px;
  padding: 1rem 1.2rem;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}
.concept-title { font-weight: 600; color: #7dd3fc; font-size: 0.95rem; }
.concept-desc { font-size: 0.78rem; color: #94a3b8; line-height: 1.5; }

.compare-wrap {
  display: flex;
  align-items: center;
  gap: 1.2rem;
}
.compare-side { flex: 1; }
.compare-label {
  font-size: 0.68rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  margin-bottom: 0.5rem;
}
.label-bad  { color: #f87171; }
.label-good { color: #86efac; }

.compare-arrow {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.2rem;
  flex-shrink: 0;
}
.arrow-icon {
  font-size: 1.8rem;
  color: #475569;
  line-height: 1;
}
.arrow-label {
  font-size: 0.62rem;
  font-weight: 700;
  color: #334155;
  letter-spacing: 0.06em;
}

/* 單體：一整塊色塊 */
.mono-box {
  background: #1a0f0f;
  border: 1px solid #7f1d1d;
  border-radius: 10px;
  padding: 0.8rem;
}
.mono-solid {
  background: #4b1717;
  border-radius: 7px;
  padding: 0.7rem 1rem;
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin-bottom: 0.55rem;
  font-size: 0.78rem;
  font-weight: 600;
  color: #fca5a5;
}
.mono-note {
  font-size: 0.65rem;
  color: #f87171;
  text-align: center;
  opacity: 0.8;
}

/* 微前端容器外框 */
.container-box {
  border: 1px solid #1e4a2e;
  border-radius: 10px;
  padding: 0.6rem 0.8rem 0.8rem;
  background: #071410;
}
.container-label {
  display: flex;
  align-items: baseline;
  gap: 0.5rem;
  font-size: 0.68rem;
  font-weight: 700;
  color: #86efac;
  letter-spacing: 0.04em;
  margin-bottom: 0.5rem;
}
.container-sub {
  font-size: 0.6rem;
  font-weight: 400;
  color: #374151;
  letter-spacing: 0;
}

/* 微前端子應用 */
.mf-box {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}
.mf-block {
  border-radius: 7px;
  padding: 0.4rem 0.7rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid;
}
.mf-name { font-size: 0.78rem; font-weight: 600; }
.mf-badges {
  display: flex;
  align-items: center;
  gap: 0.3rem;
}
.mf-team-tag {
  font-size: 0.6rem;
  opacity: 0.55;
  white-space: nowrap;
}
.deploy-badge {
  font-size: 0.58rem;
  font-weight: 700;
  color: #86efac;
  background: #052e16;
  border: 1px solid #14532d;
  border-radius: 4px;
  padding: 0.15em 0.45em;
  white-space: nowrap;
}

.block-a { background: #0c2a3e; border-color: #1e4a6e; color: #7dd3fc; }
.block-b { background: #0a2a17; border-color: #14532d; color: #86efac; }
.block-c { background: #1c1a00; border-color: #3d3200; color: #fde68a; }
</style>
