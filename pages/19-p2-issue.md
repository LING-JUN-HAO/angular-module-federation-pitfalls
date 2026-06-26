---
layout: none
---

<PageNum />

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">其實，第一步應該是先翻 Issue</h1>
  <div class="slide-subtitle">AI 陪我們跑了一輪又一輪的 build，最後線索卻收斂到一篇早就存在的 Issue。</div>
</div>

<div class="content-row">

<div class="left-col">

  <div class="tl-item">
    <div class="tl-step">
      <div class="tl-num">1</div>
      <div class="tl-line"></div>
    </div>
    <div class="tl-content">
      <div class="tl-label">先和 AI 一起比對 build 產物</div>
      <div class="tl-body">一開始沒有明確線索，我們先反覆 build、比對 dist，從 chunk id 追到 remoteEntry.js，逐步收斂可疑範圍。</div>
    </div>
  </div>

  <div class="tl-item">
    <div class="tl-step">
      <div class="tl-num">2</div>
      <div class="tl-line"></div>
    </div>
    <div class="tl-content">
      <div class="tl-label">線索逐漸收斂到 scope hoisting</div>
      <div class="tl-body">中間嘗試過多個方向，但最後從 dist 差異與實驗結果確認，問題確實和 concatenateModules 造成的 scope hoisting 有關。</div>
    </div>
  </div>

  <div class="tl-item">
    <div class="tl-step">
      <div class="tl-num">3</div>
      <div class="tl-line"></div>
    </div>
    <div class="tl-content">
      <div class="tl-label">後來找到一篇高度相似的 Issue</div>
      <div class="tl-body">後續查到 <code>angular-architects/module-federation-plugin#1053</code>，其中的錯誤訊息與版本組合都和我們的情境高度吻合，讓問題方向更加明確。</div>
    </div>
  </div>

  <div class="tl-item last">
    <div class="tl-step">
      <div class="tl-num">4</div>
    </div>
    <div class="tl-content">
      <div class="tl-label">下次可以更早檢查相關 Issue</div>
      <div class="tl-body">build 驗證當然有價值，但若一開始就找到這類高度相似的案例，應該可以少繞幾輪、也更快鎖定方向。</div>
    </div>
  </div>

</div>

<div class="right-col">
  <img src="/issue-1053.png" class="issue-img" />
  <a class="issue-link" href="https://github.com/angular-architects/module-federation-plugin/issues/1053" target="_blank">@angular-architects/module-federation Issue #1053</a>
  <div class="ai-note">AI 或許足夠強大，但也不要忘記我們還有社群的經驗可以借鏡。</div>
</div>

</div>
</div>

<style>
.slide-wrap {
  display: flex;
  flex-direction: column;
  height: 100vh;
  padding: 2rem 2.5rem 1.5rem 2.5rem;
  box-sizing: border-box;
  overflow: hidden;
}
.slide-header { margin-bottom: 1.1rem; }
.slide-title {
  font-size: 1.7rem;
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
}
.right-col {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
}
.tl-item {
  display: flex;
  gap: 1rem;
  align-items: stretch;
}
.tl-step {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex-shrink: 0;
  width: 1.8rem;
}
.tl-num {
  width: 1.8rem;
  height: 1.8rem;
  border-radius: 50%;
  background: #1e293b;
  border: 1px solid #334155;
  color: #94a3b8;
  font-size: 0.72rem;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
.tl-line {
  flex: 1;
  width: 1px;
  background: #1e293b;
  margin: 0.25rem 0;
}
.tl-content {
  padding-bottom: 1.3rem;
  flex: 1;
}
.tl-item.last .tl-content { padding-bottom: 0; }
.tl-label {
  font-size: 0.64rem;
  font-weight: 800;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #cbd5e1;
  margin-bottom: 0.3rem;
  margin-top: 0.15rem;
}
.tl-body {
  font-size: 0.88rem;
  color: #94a3b8;
  line-height: 1.7;
}
.tl-body code {
  font-family: 'Fira Code', monospace;
  font-size: 0.82rem;
  color: #a5b4fc;
}
.issue-img {
  width: 100%;
  height: auto;
  border-radius: 6px;
  border: 1px solid #30363d;
  display: block;
}
.issue-link {
  font-family: 'Fira Code', monospace;
  font-size: 0.78rem;
  color: #7dd3fc;
  text-decoration: underline;
}
.ai-note {
  font-size: 0.82rem;
  color: #cbd5e1;
  line-height: 1.75;
  padding: 0.7rem 0.95rem;
  background: #0f172a;
  border-left: 3px solid #7dd3fc;
  border-radius: 0 6px 6px 0;
}
</style>
