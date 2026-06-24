---
layout: default
---

# Remote 需要它，shared scope 卻沒有

<div class="body-wrap">

<div class="two-col">

<div class="left-col">

<div class="code-block">
  <div class="code-header">
    <span class="file-label">Remote — component.ts</span>
  </div>

```ts
import { toSignal } from '@angular/core/rxjs-interop';
import { interval } from 'rxjs';

@Component({ ... })
export class TimerComponent {
  // ...略
  timer = toSignal(interval(1000), { initialValue: 0 });
}
```

</div>

<div class="config-block">
  <div class="config-header">
    <span class="file-label">Remote — webpack.config.js</span>
    <span class="config-note">rxjs-interop 有列出</span>
  </div>

```ts
shared: share({
  "@angular/core": {
    singleton: true,
    requiredVersion: "auto",
  },
  "@angular/core/rxjs-interop": {
    singleton: true,
    requiredVersion: "auto",
  },
});
```

</div>

</div>

<div class="right-col">

<div class="causal-chain">
  <v-click>
  <div class="chain-label">問題發生流程</div>
  <div class="chain-node cause">
    Host 有列出 <code>@angular/core/rxjs-interop</code>，但 Host 未實際 import，未進入 Host bundle
  </div>
  </v-click>

  <v-click>
  <div class="chain-arrow">↓</div>
  <div class="chain-node effect">
    shared scope 裡不存在此套件
  </div>
  </v-click>

  <v-click>
  <div class="chain-arrow">↓</div>
  <div class="chain-node neutral">
    Remote 載入時去 shared scope 找，但找不到
  </div>
  </v-click>

  <v-click>
  <div class="chain-arrow">↓</div>
  <div class="chain-node fallback">
    Module Federation fallback 觸發
  </div>
  </v-click>

  <v-click>
  <div class="chain-arrow">↓</div>
  <div class="chain-node result">
    使用 Remote bundle 內的版本
  </div>
  </v-click>
</div>

<v-click>
<div class="compare-label">預期 vs 實際</div>
<div class="compare-table">
  <div class="compare-row success">
    <span class="compare-cond">原本預期</span>
    <span class="compare-sep">→</span>
    <span class="compare-result">從 Host shared scope 載入</span>
  </div>
  <div class="compare-divider"></div>
  <div class="compare-row fail">
    <span class="compare-cond">實際發生</span>
    <span class="compare-sep">→</span>
    <span class="compare-result">shared scope 沒有 → fallback → 使用 Remote bundle 內版本</span>
  </div>
</div>
</v-click>

</div>
</div>

</div>

<style>
.body-wrap {
  height: calc(100% - 4.5rem);
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  margin-top: 0.8rem;
}
.two-col {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.8rem;
  align-items: stretch;
}
.left-col {
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
}
.right-col {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}
.code-block,
.config-block {
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 8px;
  overflow: hidden;
}
.code-block :deep(pre),
.config-block :deep(pre) {
  font-size: 0.62rem !important;
  line-height: 1.4 !important;
  margin: 0 !important;
}
.code-header,
.config-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 0.9rem;
  background: #0b1222;
  border-bottom: 1px solid #1e293b;
}
.file-label {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #94a3b8;
}
.config-note {
  font-size: 0.52rem;
  color: #86efac;
  background: #052e16;
  border: 1px solid #14532d;
  border-radius: 3px;
  padding: 0.1em 0.5em;
  font-weight: 600;
}
.causal-chain {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  gap: 0;
}
.chain-node {
  font-size: 0.7rem;
  line-height: 1.5;
  padding: 0.45rem 0.85rem;
  border-radius: 6px;
}
.chain-node code {
  font-family: 'Fira Code', monospace;
  font-size: 0.66rem;
  color: #a5b4fc;
}
.chain-node.cause    { background: #0f172a; border: 1px solid #334155; border-left: 3px solid #475569; color: #94a3b8; }
.chain-node.neutral  { background: #0f172a; border: 1px solid #334155; border-left: 3px solid #475569; color: #94a3b8; }
.chain-node.effect   { background: #0b1628; border: 1px solid #1e3a5f; border-left: 3px solid #3b82f6; color: #7dd3fc; }
.chain-node.fallback { background: #1a1000; border: 1px solid #92400e; border-left: 3px solid #f59e0b; color: #fbbf24; font-weight: 600; }
.chain-node.result   { background: #1a0808; border: 1px solid #991b1b; border-left: 3px solid #ef4444; color: #fca5a5; font-weight: 600; }
.chain-label {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #94a3b8;
  margin-bottom: 0.4rem;
}
.chain-arrow {
  font-size: 1rem;
  color: #7dd3fc;
  text-align: center;
  line-height: 1.3;
  padding: 0.05rem 0;
}
.compare-label {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #94a3b8;
  margin-top: 0.75rem;
  margin-bottom: 0.4rem;
}
.compare-table {
  border: 1px solid #1e293b;
  border-radius: 7px;
  overflow: hidden;
}
.compare-row {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  padding: 0.5rem 0.9rem;
  font-size: 0.7rem;
}
.compare-row.success { background: #030f07; }
.compare-row.fail    { background: #150808; }
.compare-divider { height: 1px; background: #1e293b; }
.compare-cond {
  font-family: 'Fira Code', monospace;
  font-size: 0.65rem;
  width: 82px;
  flex-shrink: 0;
}
.compare-row.success .compare-cond { color: #86efac; }
.compare-row.fail    .compare-cond { color: #f87171; }
.compare-sep { color: #7dd3fc; flex-shrink: 0; }
.compare-result { color: #94a3b8; }
.compare-row.success .compare-result { color: #86efac; }
.compare-row.fail    .compare-result { color: #f87171; }
</style>
