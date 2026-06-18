---
layout: default
---

# Shared 如何讓所有子應用共用依賴？

<div class="body-wrap">
  <div class="intro-wrap">
    <div class="intro-line">所有載入到同一頁面的應用，都會在 Runtime 自動協商套件版本；版本相容則共用同一份實例，不相容時依設定拋出錯誤或各自載入。</div>
  </div>
  <div class="configs-section">
    <div class="config-block remote-block">
      <div class="config-title remote-c">Remote — webpack.config.js</div>
      <div class="code-body">
        <div class="code-line"><span class="ck">shared</span>: <span class="cf">share</span>({</div>
        <div class="code-line"><span class="ci"></span><span class="cs">'@angular/core'</span>: {</div>
        <div class="code-line"><span class="ci2"></span><span class="ck">singleton</span>: <span class="cb">true</span>,</div>
        <div class="code-line"><span class="ci2"></span><span class="ck">strictVersion</span>: <span class="cb">true</span>,</div>
        <div class="code-line"><span class="ci2"></span><span class="ck">requiredVersion</span>: <span class="cs">'20.1.0'</span>,</div>
        <div class="code-line"><span class="ci"></span>},</div>
        <div class="code-line"><span class="ci"></span><span class="cs">'rxjs'</span>: { <span class="ck">singleton</span>: <span class="cb">true</span> },</div>
        <div class="code-line">})</div>
      </div>
    </div>
    <div class="runtime-col">
      <div class="runtime-title">Runtime 協商</div>
      <div class="scenario good-scenario">
        <div class="s-row"><span class="s-role host-c">Host</span><span class="s-ver">20.1.0</span></div>
        <div class="s-arrow">↕</div>
        <div class="s-row"><span class="s-role remote-c">Remote</span><span class="s-ver">20.1.0</span></div>
        <div class="s-result good">✓ 共用同一份</div>
      </div>
      <div class="scenario-sep"></div>
      <div class="scenario bad-scenario">
        <div class="s-row"><span class="s-role host-c">Host</span><span class="s-ver">20.1.0</span></div>
        <div class="s-arrow">↕</div>
        <div class="s-row"><span class="s-role remote-c">Remote</span><span class="s-ver">19.2.0</span></div>
        <div class="s-result bad">✗ 各自載入</div>
      </div>
    </div>
    <div class="config-block host-block">
      <div class="config-title host-c">Host — webpack.config.js</div>
      <div class="code-body">
        <div class="code-line"><span class="ck">shared</span>: <span class="cf">share</span>({</div>
        <div class="code-line"><span class="ci"></span><span class="cs">'@angular/core'</span>: {</div>
        <div class="code-line"><span class="ci2"></span><span class="ck">singleton</span>: <span class="cb">true</span>,</div>
        <div class="code-line"><span class="ci2"></span><span class="ck">strictVersion</span>: <span class="cb">true</span>,</div>
        <div class="code-line"><span class="ci2"></span><span class="ck">requiredVersion</span>: <span class="cs">'20.1.0'</span>,</div>
        <div class="code-line"><span class="ci"></span>},</div>
        <div class="code-line"><span class="ci"></span><span class="cs">'rxjs'</span>: { <span class="ck">singleton</span>: <span class="cb">true</span> },</div>
        <div class="code-line">})</div>
      </div>
    </div>
  </div>
  <div class="options-section">
    <div class="option-row">
      <span class="option-key">singleton</span>
      <span class="option-desc">優先使用單一套件實例，避免同一套件被重複初始化</span>
    </div>
    <div class="option-row">
      <span class="option-key">strictVersion</span>
      <span class="option-desc">版本不符時直接拋出錯誤，而非自動降級或使用不相容版本</span>
    </div>
    <div class="option-row">
      <span class="option-key">requiredVersion</span>
      <span class="option-desc">指定可接受的版本範圍，Runtime 依此判斷與對方版本是否可共用</span>
    </div>
  </div>
</div>

<style>
.body-wrap {
  height: calc(100% - 4.5rem);
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
  margin-top: 0.7rem;
}
.intro-wrap {
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}
.intro-line {
  font-size: 0.78rem;
  color: #64748b;
  line-height: 1.6;
}
.intro-note {
  font-size: 0.73rem;
  color: #7dd3fc;
  line-height: 1.5;
  padding-left: 0.8rem;
  border-left: 2px solid #1e4a6e;
}
.configs-section {
  display: flex;
  align-items: stretch;
  gap: 0.8rem;
  flex: 1;
  min-height: 0;
}
.config-block {
  flex: 1;
  border-radius: 10px;
  border: 1px solid;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}
.remote-block { background: #0a1f12; border-color: #14532d; }
.host-block   { background: #071e38; border-color: #1e4a6e; }
.config-title {
  font-size: 0.6rem;
  font-weight: 700;
  font-family: 'Fira Code', monospace;
  letter-spacing: 0.04em;
  padding: 0.5rem 0.9rem;
  border-bottom: 1px solid;
  flex-shrink: 0;
}
.remote-block .config-title { border-color: #14532d; }
.host-block   .config-title { border-color: #1e4a6e; }
.remote-c { color: #86efac; }
.host-c   { color: #7dd3fc; }
.accent   { color: #7dd3fc; font-weight: 600; }
.code-body {
  padding: 0.6rem 0.9rem;
  display: flex;
  flex-direction: column;
  gap: 0.1rem;
  flex: 1;
}
.code-line {
  font-family: 'Fira Code', monospace;
  font-size: 0.62rem;
  line-height: 1.65;
}
.ck  { color: #7dd3fc; }
.cf  { color: #c084fc; }
.cs  { color: #86efac; }
.cb  { color: #fb923c; }
.ci  { display: inline-block; width: 1.2em; }
.ci2 { display: inline-block; width: 2.4em; }
.runtime-col {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.4rem;
  flex-shrink: 0;
  width: 9rem;
  justify-content: center;
}
.runtime-title {
  font-size: 0.55rem;
  font-weight: 700;
  color: #334155;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  flex-shrink: 0;
}
.scenario {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.2rem;
  padding: 0.4rem 0.6rem;
  border-radius: 6px;
  border: 1px solid;
  width: 100%;
}
.good-scenario { background: #071410; border-color: #14532d; }
.bad-scenario  { background: #150808; border-color: #7f1d1d; }
.s-row {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  width: 100%;
}
.s-role {
  font-size: 0.55rem;
  font-weight: 700;
  font-family: 'Fira Code', monospace;
  min-width: 3.2rem;
}
.s-ver {
  font-size: 0.55rem;
  color: #94a3b8;
  font-family: 'Fira Code', monospace;
}
.s-arrow {
  font-size: 0.85rem;
  color: #334155;
  line-height: 1;
}
.s-result {
  font-size: 0.6rem;
  font-weight: 700;
  padding-top: 0.25rem;
  border-top: 1px solid #1e293b;
  width: 100%;
  text-align: center;
}
.s-result.good { color: #86efac; }
.s-result.bad  { color: #f87171; }
.scenario-sep {
  width: 100%;
  height: 1px;
  background: #1e293b;
  flex-shrink: 0;
}
.options-section {
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 10px;
  overflow: hidden;
  flex-shrink: 0;
}
.option-row {
  display: flex;
  align-items: baseline;
  gap: 0.8rem;
  padding: 0.45rem 1rem;
  border-bottom: 1px solid #1a2234;
}
.option-row:last-child { border-bottom: none; }
.option-key {
  font-family: 'Fira Code', monospace;
  font-weight: 700;
  font-size: 0.68rem;
  color: #e2e8f0;
  white-space: nowrap;
  min-width: 8rem;
  flex-shrink: 0;
}
.option-desc {
  font-size: 0.67rem;
  color: #64748b;
  line-height: 1.55;
}
</style>
