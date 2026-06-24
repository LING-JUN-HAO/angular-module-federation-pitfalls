---
layout: center
class: text-center
---

<div class="mb-3">
  <span class="part-badge">延伸思考</span>
</div>

# 載入 Remote 只是第一步

<div class="think-subtitle">Module Federation 幫你把 Remote 載進來，但載進來之後，還有很多「誰來管」的問題要決定</div>

<div class="mt-6 think-grid">

<div class="think-card">
  <div class="think-num">思考一</div>
  <div class="think-q">shared 套件怎麼管？</div>
  <div class="think-body">不是每個套件都適合 shared，也不是加上 singleton 就一定安全。</div>
  <div class="think-point"><span class="point-label">重點是：</span>版本、來源、secondary entry point 都要有規則。</div>
</div>

<div class="think-card">
  <div class="think-num">思考二</div>
  <div class="think-q">子應用之間怎麼溝通？</div>
  <div class="think-body">可以用 Custom Event、Event Bus，也可以讓 Host 當中介。</div>
  <div class="think-point"><span class="point-label">重點是：</span>事件名稱、資料格式、誰負責解除監聽，都要先約好。</div>
</div>

<div class="think-card">
  <div class="think-num">思考三</div>
  <div class="think-q">跨 Remote 的狀態由誰管理？</div>
  <div class="think-body">可以由 Host 提供 shared service，或共用 library 負責。</div>
  <div class="think-point"><span class="point-label">重點是：</span>誰管理狀態，其他人就會依賴它的契約。</div>
</div>

<div class="think-card">
  <div class="think-num">思考四</div>
  <div class="think-q">Host 要不要當入口守門員？</div>
  <div class="think-body">使用者要進入某個 Remote 前，是 Host 先判斷能不能進，還是交給 Remote 自己處理？</div>
  <div class="think-point"><span class="point-label">重點是：</span>Host 統一把關，規則比較一致；Remote 自己處理，自主性比較高。</div>
</div>

</div>

<style>
.think-subtitle {
  font-size: 0.8rem;
  color: #cbd5e1;
  line-height: 1.7;
  max-width: 640px;
  margin: 1rem auto 0;
  margin-bottom: 8px;
}
.think-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  align-items: stretch;
  gap: 1rem;
  max-width: 760px;
  margin: 0 auto;
}
.think-card {
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 12px;
  padding: 1.2rem 1.4rem;
  text-align: left;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}
.think-num {
  font-size: 0.62rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #7dd3fc;
}
.think-q {
  font-weight: 700;
  color: #e2e8f0;
  font-size: 0.95rem;
}
.think-body {
  font-size: 0.72rem;
  color: #94a3b8;
  line-height: 1.6;
}
.think-point {
  font-size: 0.72rem;
  color: #cbd5e1;
  line-height: 1.6;
  border-top: 1px solid #1e293b;
  padding-top: 0.5rem;
  margin-top: auto;
}
.point-label {
  color: #7dd3fc;
  font-weight: 700;
}
.final-note {
  font-size: 0.85rem;
  color: #94a3b8;
  line-height: 1.8;
}
</style>
