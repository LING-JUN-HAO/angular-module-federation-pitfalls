---
layout: none
---

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">處理方式：關閉 <code>concatenateModules</code>，穩定 shared module 初始化順序</h1>
  <div class="slide-subtitle">左邊用最小模型重現錯誤型態，右邊對照 dist 確認錯誤發生在 shared module 初始化階段</div>
</div>

<div class="content-row">

<div class="left-col">

<div>
  <div class="mini-label">最小模型：class 繼承依賴跨 module</div>

```ts
// A: rxjs base
export class Subscriber {}
```

```ts
// B: rxjs/operators
import { Subscriber } from "./A";
export class OperatorSubscriber extends Subscriber {}
```

</div>

<div v-click>
  <div class="mini-label mt-3 warn-label">webpack runtime 模擬：module wrapper 合併後的 exports 時序</div>
  <div class="bridge-note">上面的 import/export，webpack 編譯後實際變成這樣：</div>

```ts
// 正常順序：A 先把 Subscriber 放進 exports 箱子，B 再拿
var moduleA = { exports: {} };
class Subscriber {}
moduleA.exports.Subscriber = Subscriber;

class OperatorSubscriber extends moduleA.exports.Subscriber {}
```

```ts
// 錯誤順序（賦值動作被排到後面）：B 太早執行，箱子還是空的
var moduleA = { exports: {} };

class OperatorSubscriber extends moduleA.exports.Subscriber {}
// moduleA.exports.Subscriber === undefined → Class extends value undefined

// class Subscriber {} 跟 moduleA.exports.Subscriber = Subscriber
// 被排到下面才執行，這裡已經來不及了
```

</div>

</div>

<div class="right-col">

  <div class="img-label">對照 dist：shared module 是在哪個階段初始化的</div>

<div class="trace-steps">
  <div v-click class="trace-step">
    <div class="trace-num">1</div>
    <div class="trace-body">
      <div class="trace-title">main.js 啟動後，先完成 bootstrap → shared scope 註冊 → remoteEntry.js 宣告</div>
      <div class="trace-detail">才輪到 app code；<code>rxjs</code> / <code>rxjs/operators</code> 都走這條 shared 載入流程</div>
    </div>
  </div>
  <div v-click class="trace-step">
    <div class="trace-num">2</div>
    <div class="trace-body">
      <div class="trace-title">shared module 初始化時，class 繼承依賴必須已就緒</div>
      <div class="trace-detail"><code>OperatorSubscriber extends Subscriber</code> 需要 <code>Subscriber</code> 已完成初始化</div>
    </div>
  </div>
  <div v-click class="trace-step">
    <div class="trace-num">3</div>
    <div class="trace-body">
      <div class="trace-title">concatenateModules 讓 module wrapper 被合併</div>
      <div class="trace-detail">scope hoisting 後，原本分開的初始化邊界變得更敏感</div>
    </div>
  </div>
  <div v-click class="trace-step">
    <div class="trace-num">4</div>
    <div class="trace-body trace-body-err">
      <div class="trace-title trace-title-err">因此錯誤發生在 shared module 初始化階段：<code>Class extends value undefined</code></div>
      <div class="trace-detail">對照兩份 dist 確認：關閉 concatenateModules 後，這條時序路徑不再出現</div>
    </div>
  </div>
</div>

<v-click>
<div class="fix-block">
  <div class="fix-block-label">解法</div>

```ts {2}
optimization: {
  concatenateModules: false,
}
```

</div>
</v-click>

<v-click>
<div class="tl-ruling">結論：這不是修正原始碼依賴，而是讓 production dist 的 shared module 初始化流程回到較可預測的狀態。<span class="tl-cost">代價：bundle 可能略大；換回較可預測的 shared module 載入路徑。</span></div>
</v-click>

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
  color: #cbd5e1;
  line-height: 1.6;
}
.content-row {
  display: grid;
  grid-template-columns: 0.9fr 1.1fr;
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
  gap: 0.6rem;
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
.warn-label { color: #f59e0b; }
.fix-label { color: #4ade80; }
.mt-3 { margin-top: 0.55rem; }
.bridge-note {
  font-size: 0.58rem;
  color: #94a3b8;
  font-style: italic;
  margin-top: 0.2rem;
  margin-bottom: 0.35rem;
}
.left-col pre {
  font-size: 0.6rem !important;
  line-height: 1.5 !important;
  margin: 0 !important;
  border-radius: 6px !important;
}
.left-col pre code {
  font-size: 0.6rem !important;
}
.left-col pre + pre {
  margin-top: 0.4rem !important;
}
.img-label {
  font-size: 0.56rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #94a3b8;
  border-bottom: 1px solid #1e293b;
  padding-bottom: 0.3rem;
  margin-bottom: 0.2rem;
  flex-shrink: 0;
}
.trace-steps {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
.trace-step {
  display: flex;
  gap: 0.6rem;
  align-items: flex-start;
}
.trace-num {
  width: 1.1rem;
  height: 1.1rem;
  border-radius: 50%;
  background: #1e293b;
  border: 1px solid #334155;
  color: #94a3b8;
  font-size: 0.48rem;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  margin-top: 0.05rem;
}
.trace-body { flex: 1; }
.trace-title {
  font-size: 0.6rem;
  font-weight: 700;
  color: #cbd5e1;
  margin-bottom: 0.1rem;
}
.trace-title code {
  font-family: 'Fira Code', monospace;
  font-size: 0.58rem;
  color: #a5b4fc;
}
.trace-title-err { color: #f87171; }
.trace-detail {
  font-size: 0.55rem;
  color: #94a3b8;
  line-height: 1.5;
}
.trace-detail code {
  font-family: 'Fira Code', monospace;
  font-size: 0.53rem;
  color: #a5b4fc;
}
.fix-block {
  background: #0a1f12;
  border: 1px solid #14532d;
  border-radius: 8px;
  padding: 0.5rem 0.7rem;
  flex-shrink: 0;
}
.fix-block-label {
  font-size: 0.52rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #4ade80;
  margin-bottom: 0.3rem;
}
.fix-block pre {
  font-size: 0.58rem !important;
  line-height: 1.4 !important;
  margin: 0 !important;
  border-radius: 6px !important;
}
.tl-ruling {
  font-size: 0.56rem;
  font-weight: 600;
  color: #e2e8f0;
  line-height: 1.65;
  padding: 0.55rem 0.8rem;
  background: #0f172a;
  border-left: 3px solid #7dd3fc;
  border-radius: 0 6px 6px 0;
  flex-shrink: 0;
}
.tl-ruling code {
  font-family: 'Fira Code', monospace;
  font-size: 0.54rem;
  color: #7dd3fc;
}
.tl-cost {
  display: block;
  margin-top: 0.3rem;
  font-size: 0.54rem;
  font-weight: 500;
  color: #94a3b8;
}
</style>
