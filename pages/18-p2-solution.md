---
layout: none
---

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">處理方式：關閉 webpack production <code>concatenateModules</code></h1>
  <div class="slide-subtitle">先用簡化模型說明機制，再回頭對照 dist 產物確認現象一致</div>
</div>

<div class="content-row">

<div class="left-col">

<div>
  <div class="mini-label">最小模型：兩個互相依賴的模組</div>

```ts
// A: rxjs base
export class Subscriber {}

// B: rxjs/operators
import { Subscriber } from "./A";
export class OperatorSubscriber extends Subscriber {}
```

</div>

<div v-click>
  <div class="mini-label mt-3 warn-label">啟用 concatenateModules 後的風險</div>

```ts
class OperatorSubscriber extends Subscriber {}
```

  <div class="explain-box explain-warn">scope hoisting 不會改變 ES module 的依賴執行順序，但會合併模組邊界；在 shared module 載入路徑上，原本不容易觀察到的初始化時序問題可能因此浮現，進而拋出 <code>Class extends value undefined</code>。</div>

  <div class="model-note">示意：簡化模型，非逐行對應實際 dist 程式碼</div>

</div>

<div v-click>
  <div class="mini-label mt-3 fix-label">解決方式：webpack 設定關閉 concatenateModules 優化設定</div>

```ts {2}
optimization: {
  concatenateModules: false,
}
```

</div>

</div>

<div v-click>
<div class="right-col">

  <div class="img-label">對回實際 dist 產物</div>
  <div class="trace-steps">
    <div class="trace-step">
      <div class="trace-num">1</div>
      <div class="trace-body">
        <div class="trace-title">Production 啟動流程不是同步執行</div>
        <div class="trace-detail"><code>main.*.js</code> 不是直接執行 app，而是先載入 bootstrap chunk，才進入後續初始化流程</div>
      </div>
    </div>
    <div class="trace-step">
      <div class="trace-num">2</div>
      <div class="trace-body">
        <div class="trace-title">Shared module 需要額外初始化</div>
        <div class="trace-detail"><code>remoteEntry.js</code> 內可見 <code>rxjs</code> / <code>rxjs/operators</code> 都要經過 shared 載入流程，不會跟主程式同步完成初始化</div>
      </div>
    </div>
    <div class="trace-step">
      <div class="trace-num">3</div>
      <div class="trace-body">
        <div class="trace-title">錯誤發生在 shared 載入路徑上</div>
        <div class="trace-detail">原本出錯的 production build 中，<code>rxjs/operators</code> 走一條獨立的 shared 初始化路徑；<code>Class extends value undefined</code> 就是沿著這條路徑發生的</div>
      </div>
    </div>
    <div class="trace-step">
      <div class="trace-num">4</div>
      <div class="trace-body">
        <div class="trace-title">對照 dist 確認：問題出在 shared module 初始化順序</div>
        <div class="trace-detail">比對兩份 dist 的執行順序後確認：關閉 <code>concatenateModules</code> 後，shared module 的切分回到可預期的邊界，原本提前被引用、尚未初始化的時序路徑不再出現</div>
      </div>
    </div>
  </div>

  <div class="tl-ruling">代價：scope hoisting 透過合併 closure 減少 module wrapper 與 runtime 開銷，關閉後 bundle 通常會略大；但能換回較可預測的 shared module 載入與初始化路徑。</div>

</div>
</div>

</div>
</div>

<style>
.slide-wrap {
  display: flex;
  flex-direction: column;
  height: 100vh;
  padding: 1.8rem 2.5rem 1.2rem 2.5rem;
  box-sizing: border-box;
  overflow: hidden;
}
.slide-header { margin-bottom: 0.9rem; }
.slide-title {
  font-size: 1.75rem;
  font-weight: 800;
  color: #f1f5f9;
  line-height: 1.25;
  margin: 0 0 0.4rem 0;
}
.slide-title code {
  font-family: 'Fira Code', monospace;
  font-size: 1.35rem;
  color: #86efac;
}
.slide-subtitle {
  font-size: 0.62rem;
  color: #64748b;
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
  gap: 0.7rem;
  min-height: 0;
}
.right-col {
  display: flex;
  flex-direction: column;
  gap: 0.55rem;
  min-height: 0;
  overflow: hidden;
}
.mini-label {
  font-size: 0.58rem;
  font-weight: 700;
  letter-spacing: 0.07em;
  text-transform: uppercase;
  color: #94a3b8;
  margin-bottom: 0.25rem;
}
.warn-label { color: #f87171; }
.fix-label { color: #4ade80; }
.explain-box {
  font-size: 0.56rem;
  color: #94a3b8;
  line-height: 1.8;
  padding: 0.45rem 0.65rem;
  border-radius: 0 4px 4px 0;
  margin-bottom: 0.35rem;
}
.explain-box code {
  font-family: 'Fira Code', monospace;
  font-size: 0.6rem;
  color: #a5b4fc;
}
.explain-warn {
  background: #1e0a0a;
  border-left: 2px solid #f87171;
  margin-top: 0.6rem;
  margin-bottom: 0;
}
.model-note {
  font-size: 0.56rem;
  color: #94a3b8;
  margin-top: 0.25rem;
  font-style: italic;
}
.mt-3 { margin-top: 0.55rem; }
.left-col pre {
  font-size: 0.62rem !important;
  line-height: 1.5 !important;
  margin: 0 !important;
  border-radius: 6px !important;
}
.left-col pre code {
  font-size: 0.62rem !important;
}
.img-label {
  font-size: 0.56rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #94a3b8;
  border-bottom: 1px solid #1e293b;
  padding-bottom: 0.3rem;
  margin-bottom: 0.4rem;
}
.trace-steps {
  display: flex;
  flex-direction: column;
  gap: 1.1rem;
}
.trace-step {
  display: flex;
  gap: 0.6rem;
  align-items: flex-start;
}
.trace-num {
  width: 1.2rem;
  height: 1.2rem;
  border-radius: 50%;
  background: #1e293b;
  border: 1px solid #334155;
  color: #94a3b8;
  font-size: 0.5rem;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  margin-top: 0.05rem;
}
.trace-body { flex: 1; }
.trace-title {
  font-size: 0.62rem;
  font-weight: 700;
  color: #cbd5e1;
  margin-bottom: 0.1rem;
}
.trace-detail {
  font-size: 0.58rem;
  color: #94a3b8;
  line-height: 1.55;
}
.trace-detail code {
  font-family: 'Fira Code', monospace;
  font-size: 0.54rem;
  color: #a5b4fc;
}
.tl-ruling {
  font-size: 0.58rem;
  font-weight: 600;
  color: #e2e8f0;
  line-height: 1.7;
  padding: 0.6rem 0.85rem;
  background: #0f172a;
  border-left: 3px solid #7dd3fc;
  border-radius: 0 6px 6px 0;
}
</style>
