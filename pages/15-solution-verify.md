---
layout: none
---

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">處理方式：Host 加入 side effect import</h1>
  <div class="slide-subtitle">
    在 Host 加入 side effect import 後，可以透過 Host 的 console 直接觀察 shared scope 的註冊及載入情形
  </div>
</div>

<div class="content-row">

<div class="left-col">

<div class="code-block">
  <div class="code-header">
    <span class="file-label">host/src/app/app.routes.ts</span>
  </div>

```ts {2}
import { loadRemoteModule } from "@angular-architects/module-federation";
import "@angular/core/rxjs-interop"; // side effect import

loadRemoteModule({ ... }).then((m) => m.routes);
```

</div>

<div class="timing-note">Host 先載入 <code>@angular/core/rxjs-interop</code>，確保 shared scope 有註冊。</div>

<div class="code-block">
  <div class="code-header">
    <span class="file-label">host/src/main.ts</span>
  </div>

```ts
declare const __webpack_share_scopes__: any;

import("./bootstrap").then(() => {
  // bootstrap 完成後，shared scope 已初始化
  console.log(
    "@angular/core =>",
    __webpack_share_scopes__?.default?.["@angular/core"],
  );
  console.log(
    "@angular/core/rxjs-interop =>",
    __webpack_share_scopes__?.default?.["@angular/core/rxjs-interop"],
  );
});
```

</div>

</div>

<v-click>
<div class="right-col">
  <div class="img-label">Host console 輸出</div>
  <div class="img-note"><code>@angular/core/rxjs-interop</code> 成功出現在 shared scope 清單中</div>
  <div class="field-legend">
    <div class="field-item">· <code>from</code>：由哪個 app 提供</div>
    <div class="field-item">· <code>loaded: 1</code>：已實際載入，版本協商勝出</div>
  </div>
  <img src="/shared-scope-console.png" class="slide-img" />
</div>
</v-click>

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
.slide-header {
  margin-bottom: 0.75rem;
}
.slide-title {
  font-size: 2rem;
  font-weight: 800;
  color: #f1f5f9;
  line-height: 1.25;
  margin: 0 0 0.4rem 0;
}
.slide-subtitle {
  font-size: 0.65rem;
  color: #cbd5e1;
  line-height: 1.6;
}
.content-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  flex: 1;
  min-height: 0;
}
.left-col {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
  overflow: hidden;
}
.right-col {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  overflow: hidden;
  margin-top: 0.6rem;
}
.code-block {
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 8px;
  overflow: hidden;
}
.code-block :deep(pre) {
  font-size: 0.54rem !important;
  line-height: 1.4 !important;
  margin: 0 !important;
}
.code-header {
  display: flex;
  align-items: center;
  padding: 0.25rem 0.8rem;
  background: #0b1222;
  border-bottom: 1px solid #1e293b;
}
.file-label {
  font-size: 0.52rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #94a3b8;
}
.img-label {
  font-size: 0.52rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #94a3b8;
  border-bottom: 1px solid #1e293b;
  padding-bottom: 0.35rem;
}
.img-note {
  font-size: 0.62rem;
  color: #4ade80;
  line-height: 1.55;
}
.img-note code {
  font-family: 'Fira Code', monospace;
  font-size: 0.57rem;
  color: #86efac;
  background: #052e16;
  border: 1px solid #14532d;
  padding: 0.1em 0.4em;
  border-radius: 3px;
}
.slide-img {
  width: 100%;
  height: auto;
  border-radius: 6px;
  display: block;
}
.timing-note {
  font-size: 0.6rem;
  color: #cbd5e1;
  line-height: 1.55;
  background: #0b1628;
  border: 1px solid #1e3a5f;
  border-left: 3px solid #3b82f6;
  border-radius: 6px;
  padding: 0.25rem 0.7rem;
  margin: 0.3rem 0;
}
.field-legend {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
  margin: 0;
  font-size: 0.6rem;
  color: #94a3b8;
  line-height: 1.6;
}
.field-item {
  display: flex;
  align-items: baseline;
  gap: 0.4rem;
}
.field-legend code {
  font-family: 'Fira Code', monospace;
  font-size: 0.57rem;
  color: #7dd3fc;
  background: #0c1a30;
  padding: 0.05em 0.4em;
  border-radius: 3px;
}
.timing-note code {
  font-family: 'Fira Code', monospace;
  font-size: 0.55rem;
  color: #93c5fd;
}
</style>
