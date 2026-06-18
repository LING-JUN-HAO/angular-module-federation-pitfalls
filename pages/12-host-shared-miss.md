---
layout: default
---

# Host 顯性列出，dist 卻沒有它

<div class="mt-2 mb-5 text-sm" style="color: #64748b;">
  把套件名稱寫進 shared 設定，不等於 Webpack 真的會把它共享出去
</div>

<div class="two-col">

<div class="left-col">

<div class="config-block">
  <div class="config-label">Host — webpack.config.js</div>

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

<v-click>
<div class="arrow-row">
  <div class="arrow-line-h"></div>
  <div class="arrow-label">build</div>
  <div class="arrow-line-h"></div>
</div>

<div class="dist-block">
  <div class="dist-label">Host dist — shared scope 實際內容</div>
  <div class="dist-list">
    <div class="dist-item present">
      <span class="dist-dot present-dot"></span>
      <code>@angular/core</code>
      <span class="dist-tag present-tag">已共享</span>
    </div>
    <div class="dist-item absent">
      <span class="dist-dot absent-dot"></span>
      <code>@angular/core/rxjs-interop</code>
      <span class="dist-tag absent-tag">不存在</span>
    </div>
  </div>
</div>
</v-click>

</div>

<div class="right-col">

<v-click>
<div class="reason-block">
  <div class="reason-title">為什麼列出來也沒用？</div>
  <div class="reason-body">
    <div class="reason-step">
      <span class="step-num">1</span>
      <div class="step-text">Webpack 從 entry point 出發，遞迴追蹤所有引用，建立 <code>dependency graph</code></div>
    </div>
    <div class="step-arrow"></div>
    <div class="reason-step">
      <span class="step-num">2</span>
      <div class="step-text"><code>@angular/core/rxjs-interop</code> 沒有被任何模組引用，不進入 graph</div>
    </div>
    <div class="step-arrow"></div>
    <div class="reason-step">
      <span class="step-num">3</span>
      <div class="step-text">Module Federation shared plugin 只對<strong>進入 graph 的模組</strong>套用共享規則，設定才會生效</div>
    </div>
  </div>
</div>
</v-click>

<v-click>
<div class="rule-box mt-4">
  <div class="rule-title">Module Federation 的共享前提</div>
  <div class="rule-text">
    shared 設定只是<strong>宣告意圖</strong>：<br>
    「如果這個套件被用到，就用共享版本」<br><br>
    套件本身沒有被 import，就不會進 bundle，<br>
    也就不會出現在 shared scope 裡。
  </div>
</div>
</v-click>

</div>
</div>

<style>
.two-col {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  align-items: start;
}
.config-block {
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 8px;
  overflow: hidden;
}
.config-label {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #475569;
  padding: 0.5rem 0.9rem;
  border-bottom: 1px solid #1e293b;
  background: #0b1222;
}
.arrow-row {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.6rem 0.5rem;
}
.arrow-line-h {
  flex: 1;
  height: 1px;
  background: #334155;
}
.arrow-label {
  font-size: 0.52rem;
  color: #64748b;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}
.dist-block {
  background: #0b1222;
  border: 1px solid #1e293b;
  border-radius: 8px;
  overflow: hidden;
}
.dist-label {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #475569;
  padding: 0.5rem 0.9rem;
  border-bottom: 1px solid #1e293b;
}
.dist-list {
  display: flex;
  flex-direction: column;
  gap: 0;
}
.dist-item {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  padding: 0.45rem 0.9rem;
  border-bottom: 1px solid #0f172a;
}
.dist-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  flex-shrink: 0;
}
.present-dot { background: #22c55e; }
.absent-dot  { background: #334155; border: 1px solid #475569; }
.dist-item code {
  font-size: 0.7rem;
  font-family: 'Fira Code', monospace;
  flex: 1;
}
.dist-item.present code { color: #94a3b8; }
.dist-item.absent  code { color: #334155; }
.dist-tag {
  font-size: 0.5rem;
  font-weight: 700;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  border-radius: 3px;
  padding: 0.1em 0.45em;
  flex-shrink: 0;
}
.present-tag { background: #052e16; color: #86efac; border: 1px solid #14532d; }
.absent-tag  { background: #1e293b; color: #475569; border: 1px solid #334155; }
.reason-block {
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 8px;
  padding: 0.9rem 1rem;
}
.reason-title {
  font-size: 0.58rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #475569;
  margin-bottom: 0.7rem;
}
.reason-body {
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
}
.reason-step {
  display: flex;
  align-items: flex-start;
  gap: 0.6rem;
}
.step-num {
  font-size: 0.58rem;
  font-weight: 700;
  color: #334155;
  background: #1e293b;
  border-radius: 50%;
  width: 18px;
  height: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  margin-top: 0.1rem;
}
.step-text {
  font-size: 0.7rem;
  color: #64748b;
  line-height: 1.5;
}
.step-text code {
  font-family: 'Fira Code', monospace;
  color: #a5b4fc;
  font-size: 0.67rem;
}
.step-arrow {
  width: 1px;
  height: 0.8rem;
  background: #1e293b;
  margin-left: 8px;
}
.rule-box {
  background: #0b1628;
  border: 1px solid #1e3a5f;
  border-left: 3px solid #f59e0b;
  border-radius: 6px;
  padding: 0.8rem 1rem;
}
.rule-title {
  font-size: 0.58rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #f59e0b;
  margin-bottom: 0.45rem;
}
.rule-text {
  font-size: 0.7rem;
  color: #64748b;
  line-height: 1.8;
}
.rule-text strong {
  color: #94a3b8;
}
</style>
