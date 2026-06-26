---
layout: default
---

<PageNum />

# 單體架構的痛點

<div class="mt-3 text-base" style="color: #cbd5e1;">
  當所有功能都集中在同一個前端專案中，團隊協作、部署與技術演進都會受到限制
</div>

<div class="pain-list">

  <div class="pain-item">
    <span class="pain-num">01</span>
    <div>
      <div class="pain-title">功能模組邊界不清楚</div>
      <div class="pain-desc">隨著專案成長，程式碼之間的相依關係越來越複雜，改動任何一處都需要花大量時間確認影響範圍</div>
    </div>
  </div>

  <div class="pain-item">
    <span class="pain-num">02</span>
    <div>
      <div class="pain-title">互相等待、阻塞部署</div>
      <div class="pain-desc">各團隊功能須等所有人完成後才能一併上版</div>
    </div>
  </div>

  <div class="pain-item">
    <span class="pain-num">03</span>
    <div>
      <div class="pain-title">任何異動都要重新打包整個應用</div>
      <div class="pain-desc">即使只改了一個小功能，也需要 build 並部署整個應用</div>
    </div>
  </div>

  <div class="pain-item">
    <span class="pain-num">04</span>
    <div>
      <div class="pain-title">Build 時間持續拉長</div>
      <div class="pain-desc">隨著專案規模擴大，每次 build 的時間也不斷增加，開發效率持續下降</div>
    </div>
  </div>

</div>

<style>
.pain-list {
  height: calc(100% - 8.5rem);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-top: 1rem;
}
.pain-item {
  display: flex;
  align-items: center;
  gap: 1.3rem;
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 10px;
  padding: 0.35rem 1.6rem;
}
.pain-num {
  font-size: 1.05rem;
  font-weight: 700;
  color: #f87171;
  letter-spacing: 0.05em;
  min-width: 32px;
}
.pain-title {
  font-weight: 600;
  font-size: 1.2rem;
  color: #e2e8f0;
  margin-bottom: 0.35rem;
}
.pain-desc {
  font-size: 0.95rem;
  color: #cbd5e1;
  line-height: 1.55;
}
</style>
