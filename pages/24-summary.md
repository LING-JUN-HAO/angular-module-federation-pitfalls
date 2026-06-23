---
layout: center
class: text-center
---

<div class="mb-6">
  <span class="part-badge">總結</span>
</div>

# Module Federation 的三個邊界

<div class="mt-6 flex justify-center gap-4">

<v-click>
<div class="summary-card">
  <div class="summary-problem accent-red">踩坑一</div>
  <div class="summary-title">Secondary entry point 沒進 shared scope</div>
  <div class="summary-rule">
    <span class="rule-label">原則</span>
在 <code>singleton</code> + <code>shared</code> 前提下，有狀態相依的子模組必須跟主模組從同一個來源生成，不能各自產生實例
  </div>
</div>
</v-click>

<v-click>
<div class="summary-card">
  <div class="summary-problem accent-red">踩坑二</div>
  <div class="summary-title">Production optimization 打亂載入順序</div>
  <div class="summary-rule">
    <span class="rule-label">原則</span>
Webpack optimize 階段會改變模組打包時機，有機會導致 shared dependency 的執行順序跑掉
  </div>
</div>
</v-click>

<v-click>
<div class="summary-card">
  <div class="summary-problem accent-red">踩坑三</div>
  <div class="summary-title">Tailwind class 沒進 Module Federation 的載入鏈</div>
  <div class="summary-rule">
    <span class="rule-label">取捨</span>
    用 <code>ViewEncapsulation.None</code> 讓樣式跟著 JS 一起載入，換來的代價是失去 Angular 的 scope 隔離
  </div>
</div>
</v-click>

</div>

<v-click>
<div class="mt-8 final-note">
<span class="accent">微前端開發最大的難處，其實是開發用的 runtime 跟部署到 host 之後的 runtime 不一樣。</span>
</div>
</v-click>

<style>
.summary-card {
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 12px;
  padding: 1.5rem;
  max-width: 260px;
  text-align: left;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}
.summary-problem { font-size: 0.7rem; font-weight: 700; letter-spacing: 0.08em; text-transform: uppercase; }
.summary-title { font-weight: 700; color: #e2e8f0; font-size: 0.9rem; }
.summary-rule {
  font-size: 0.75rem;
  color: #64748b;
  line-height: 1.6;
  border-top: 1px solid #1e293b;
  padding-top: 0.5rem;
  margin-top: 0.2rem;
}
.summary-rule code {
  font-family: 'Fira Code', monospace;
  font-size: 0.68rem;
  color: #a5b4fc;
}
.rule-label {
  display: inline-block;
  background: #1e293b;
  color: #7dd3fc;
  padding: 0.1em 0.5em;
  border-radius: 4px;
  font-size: 0.62rem;
  margin-right: 0.3rem;
  margin-bottom: 0.3rem;
}
.final-note {
  font-size: 0.85rem;
  color: #475569;
  line-height: 1.8;
}
</style>
