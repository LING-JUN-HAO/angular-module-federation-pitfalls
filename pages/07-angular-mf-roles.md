---
layout: default
---

<PageNum />

# Module Federation 如何載入 Remote？

<div class="body-wrap">
  <div class="roles-section">
    <div class="role-card host-card">
      <div class="role-badge host-badge">主應用（Host）</div>
      <div class="role-title">應用入口與整合層</div>
      <div class="role-desc">根據路由，在 Runtime 動態載入 Remote 公開的模組</div>
    </div>
    <div class="relation-arrow">
      <div class="arrow-label top-label">指定載入 Remote 模組</div>
      <div class="arrow-body">→</div>
    </div>
    <div class="role-card remote-card">
      <div class="role-badge remote-badge">子應用 (Remote)</div>
      <div class="role-title">獨立開發、獨立部署的子應用</div>
      <div class="role-desc">透過 exposes 決定哪些功能可以提供給 Host 載入</div>
    </div>
  </div>
  <div class="section-label">設定參數</div>
  <div class="params-section">
    <div class="params-col">
      <div class="params-col-title host-c">Host Router — loadRemoteModule()</div>
      <div class="param-row">
        <span class="param-key">type</span>
        <span class="param-desc">指定 remoteEntry 的載入格式，例如 <span class="accent">module</span> 表示以 ES Module 方式載入</span>
      </div>
      <div class="param-row">
        <span class="param-key">remoteEntry</span>
        <span class="param-desc">Remote 建置後產生的 federation 入口檔 URL，Host 透過它取得載入公開模組所需的資訊</span>
      </div>
      <div class="param-row">
        <span class="param-key">exposedModule</span>
        <span class="param-desc">指定要載入的公開模組名稱，需對應 Remote <span class="accent">exposes</span> 裡的 key</span>
      </div>
    </div>
    <div class="params-divider"></div>
    <div class="params-col">
      <div class="params-col-title remote-c">Remote — webpack.config.js</div>
      <div class="param-row">
        <span class="param-key">name</span>
        <span class="param-desc">Remote 的識別名稱，Host 載入時用來識別這個 remote container</span>
      </div>
      <div class="param-row">
        <span class="param-key">filename</span>
        <span class="param-desc">Remote 對外輸出的 federation 入口檔名稱，常見為 <span class="accent">remoteEntry.js</span></span>
      </div>
      <div class="param-row">
        <span class="param-key">exposes</span>
        <span class="param-desc">定義哪些模組可以被 Host 載入，未列在這裡的模組，Host 不能直接取得</span>
      </div>
    </div>
  </div>
</div>

<style>
.body-wrap {
  height: calc(100% - 4.5rem);
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
  margin-top: 0.7rem;
}
.roles-section {
  display: flex;
  align-items: center;
  gap: 1.2rem;
  flex-shrink: 0;
}
.role-card {
  flex: 1;
  border-radius: 10px;
  padding: 1rem 1.3rem;
  border: 1px solid;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}
.remote-card { background: #0a1f12; border-color: #14532d; }
.host-card   { background: #071e38; border-color: #1e4a6e; }
.role-badge {
  display: inline-block;
  font-family: 'Fira Code', monospace;
  font-weight: 700;
  font-size: 0.85rem;
  padding: 0.15em 0.6em;
  border-radius: 5px;
  align-self: flex-start;
}
.remote-badge { background: #052e16; color: #86efac; border: 1px solid #14532d; }
.host-badge   { background: #0c1a30; color: #7dd3fc; border: 1px solid #1e4a6e; }
.role-title {
  font-size: 0.78rem;
  font-weight: 600;
  color: #cbd5e1;
}
.role-desc {
  font-size: 0.75rem;
  color: #94a3b8;
  line-height: 1.6;
  font-weight: 500;
}
.relation-arrow {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.25rem;
  flex-shrink: 0;
}
.top-label, .bottom-label {
  font-size: 0.8rem;
  color: #94a3b8;
  font-weight: 500;
  white-space: nowrap;
}
.arrow-body {
  font-size: 1.7rem;
  color: #94a3b8;
  line-height: 1;
}
.section-label {
  font-size: 0.66rem;
  font-weight: 700;
  color: #94a3b8;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  flex-shrink: 0;
}
.params-section {
  flex: 1;
  display: flex;
  gap: 0;
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 10px;
  overflow: hidden;
  min-height: 0;
}
.params-col {
  flex: 1;
  display: flex;
  flex-direction: column;
  padding: 0.7rem 1rem;
  gap: 0;
}
.params-col-title {
  font-size: 0.72rem;
  font-weight: 700;
  font-family: 'Fira Code', monospace;
  letter-spacing: 0.04em;
  margin-bottom: 0.5rem;
  padding-bottom: 0.4rem;
  border-bottom: 1px solid #1e293b;
  flex-shrink: 0;
}
.host-c   { color: #7dd3fc; }
.remote-c { color: #86efac; }
.accent   { color: #7dd3fc; font-weight: 600; }
.param-row {
  flex: 1;
  display: flex;
  align-items: baseline;
  gap: 0.8rem;
  padding: 0.45rem 0;
  border-bottom: 1px solid #1a2234;
}
.param-row:last-child { border-bottom: none; }
.param-key {
  font-family: 'Fira Code', monospace;
  font-weight: 700;
  font-size: 0.76rem;
  color: #e2e8f0;
  white-space: nowrap;
  min-width: 7rem;
  flex-shrink: 0;
}
.param-desc {
  font-size: 0.75rem;
  color: #cbd5e1;
  line-height: 1.5;
}
.params-divider {
  width: 1px;
  background: #1e293b;
  flex-shrink: 0;
}
</style>
