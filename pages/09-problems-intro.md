---
layout: center
class: text-center transition-page
---

# 三個實戰踩坑

<div class="mt-10 flex flex-col items-center gap-4">
  <div class="problem-row">
    <div class="problem-row-num">踩坑一</div>
    <div class="problem-row-body">
      <div class="problem-row-title">Angular secondary entry point 的 shared 共用</div>
      <div class="problem-row-hint">→ inject context error</div>
    </div>
  </div>
  <div class="problem-row">
    <div class="problem-row-num">踩坑二</div>
    <div class="problem-row-body">
      <div class="problem-row-title">Webpack Production optimization 影響 Module Federation</div>
      <div class="problem-row-hint">→ Class extends value undefined</div>
    </div>
  </div>
  <div class="problem-row">
    <div class="problem-row-num">踩坑三</div>
    <div class="problem-row-body">
      <div class="problem-row-title">微前端架構下的樣式隔離與樣式污染問題</div>
      <div class="problem-row-hint">→ 樣式污染 / 覆蓋失效</div>
    </div>
  </div>
</div>

<style>
.problem-row {
  display: flex;
  align-items: center;
  gap: 1.2rem;
  background: #0f172a;
  border: 1px solid #2d0f0f;
  border-radius: 10px;
  padding: 0.9rem 1.4rem;
  width: 600px;
  text-align: left;
}
.problem-row-dim {
  border-color: #1e293b;
  background: #0a0f1a;
}
.problem-row-num {
  font-size: 0.68rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #f87171;
  min-width: 52px;
}
.problem-row-title {
  font-size: 0.88rem;
  font-weight: 600;
  color: #e2e8f0;
  margin-bottom: 0.2rem;
}
.problem-row-hint {
  font-size: 0.72rem;
  color: #94a3b8;
  font-family: 'Fira Code', monospace;
}
</style>
