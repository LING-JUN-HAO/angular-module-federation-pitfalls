---
layout: center
class: text-center
---

<div class="pit-wrap">
  <div class="pit-badge">踩坑一</div>
  <h1 class="pit-title">Angular secondary entry point<br>的 shared 共用</h1>
  <div class="pit-error">RuntimeError: NG0203: toSignal() can only be used within an injection context</div>
  <div class="pit-hint">shared 設定正確，但錯誤依然發生</div>
</div>

<style>
.pit-wrap {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.2rem;
}
.pit-badge {
  font-size: 0.68rem;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: #f87171;
  background: #2d0f0f;
  border: 1px solid #7f1d1d;
  border-radius: 6px;
  padding: 0.25em 0.9em;
}
.pit-title {
  font-size: 2rem;
  font-weight: 800;
  color: #e2e8f0;
  line-height: 1.3;
  margin: 0;
}
.pit-error {
  font-family: 'Fira Code', monospace;
  font-size: 0.72rem;
  color: #f87171;
  background: #150808;
  border: 1px solid #7f1d1d;
  border-radius: 6px;
  padding: 0.5em 1.2em;
  max-width: 600px;
}
.pit-hint {
  font-size: 0.75rem;
  color: #475569;
}
</style>
