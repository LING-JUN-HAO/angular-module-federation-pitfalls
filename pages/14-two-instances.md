---
layout: none
---

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">結果：@angular/core 模組實例不一致</h1>
  <div class="slide-subtitle">
    Host 的 shared scope 沒有登記 @angular/core/rxjs-interop，導致 remote 使用各自的 instance
  </div>
</div>

<div class="content-row">

<div class="left-col">

<v-click>
<div class="instance-diagram">
  <div class="inst-label">執行時的 module 狀態</div>
  <div class="inst-cols">
    <div class="inst-zone shared-zone">
      <div class="zone-title">shared scope</div>
      <div class="zone-pkg">
        <span class="pkg-dot green-dot"></span>
        <code>@angular/core</code>
        <span class="inst-tag shared-tag">instance A</span>
      </div>
    </div>
    <div class="inst-zone bundle-zone">
      <div class="zone-title">Remote bundle（本地）</div>
      <div class="zone-pkg">
        <span class="pkg-dot yellow-dot"></span>
        <code class="pkg-long">@angular/core/rxjs-interop</code>
        <span class="inst-tag fallback-tag">fallback</span>
      </div>
      <div class="zone-arrow">↓ 依賴</div>
      <div class="zone-pkg danger">
        <span class="pkg-dot red-dot"></span>
        <code>@angular/core</code>
        <span class="inst-tag danger-tag">instance B</span>
      </div>
    </div>
  </div>
</div>
</v-click>

<v-click>
<div class="problem-box">
  <div class="problem-title">問題點</div>
  <div class="problem-text">
    <code>toSignal()</code> 透過 B 執行 → B 沒有作用中的 injector → <span class="err">NG0203</span>
  </div>
</div>
</v-click>

</div>

<v-click>
<div class="right-col">
  <div class="img-label">Host build 完，觀察 Module Federation shared scope 註冊情形</div>
  <div class="img-note">注意：清單中沒有 <code>@angular/core/rxjs-interop</code></div>
  <img src="/webpack-shared-register.png" class="slide-img" />
</div>
</v-click>

</div>
</div>

<style>
.slide-wrap {
  display: flex;
  flex-direction: column;
  height: 100vh;
  padding: 2.5rem 2.5rem 1.5rem 2.5rem;
  box-sizing: border-box;
  overflow: hidden;
}
.slide-header {
  margin-bottom: 1rem;
}
.slide-title {
  font-size: 2rem;
  font-weight: 800;
  color: #f1f5f9;
  line-height: 1.25;
  margin: 0 0 0.5rem 0;
}
.slide-subtitle {
  font-size: 0.68rem;
  color: #cbd5e1;
  line-height: 1.7;
}
.content-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  flex: 1;
  min-height: 0;
  margin-top: 0.8rem;
}
.left-col {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  align-items: flex-start;
  overflow: hidden;
}
.right-col {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  overflow: hidden;
}
.img-label {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #94a3b8;
}
.img-note {
  font-size: 0.62rem;
  color: #f87171;
}
.img-note code {
  font-family: 'Fira Code', monospace;
  font-size: 0.60rem;
  color: #f1f5f9;
  background: #7f1d1d;
  padding: 0.1em 0.4em;
  border-radius: 3px;
}
.slide-img {
  width: 100%;
  height: auto;
  border-radius: 6px;
  display: block;
}
.instance-diagram {
  background: #0f172a;
  border: 1px solid #1e293b;
  border-radius: 8px;
  overflow: hidden;
}
.inst-label {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #94a3b8;
  padding: 0.5rem 0.9rem;
  background: #0b1222;
  border-bottom: 1px solid #1e293b;
}
.inst-cols {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.6rem;
}
.inst-zone {
  padding: 0.75rem 0.85rem;
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
}
.shared-zone { border-right: 1px solid #1e293b; }
.bundle-zone {}
.zone-title {
  font-size: 0.52rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #94a3b8;
  margin-bottom: 0.15rem;
}
.zone-pkg {
  display: flex;
  align-items: center;
  gap: 0.45rem;
  font-size: 0.65rem;
  font-family: 'Fira Code', monospace;
  color: #a5b4fc;
}
.zone-pkg.danger { color: #f87171; }
.pkg-long { font-size: 0.48rem; }
.zone-arrow {
  font-size: 0.6rem;
  color: #94a3b8;
  padding-left: 1rem;
  line-height: 1.2;
}
.zone-note {
  font-size: 0.55rem;
  color: #f87171;
  line-height: 1.5;
  border-top: 1px solid #2d0f0f;
  padding-top: 0.35rem;
  margin-top: 0.1rem;
}
.pkg-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  flex-shrink: 0;
}
.green-dot  { background: #22c55e; }
.yellow-dot { background: #f59e0b; }
.red-dot    { background: #ef4444; }
.inst-tag {
  font-size: 0.48rem;
  font-weight: 700;
  letter-spacing: 0.06em;
  border-radius: 3px;
  padding: 0.1em 0.45em;
  font-family: 'Noto Sans TC', sans-serif;
  flex-shrink: 0;
}
.shared-tag   { background: #052e16; color: #86efac; border: 1px solid #14532d; }
.fallback-tag { background: #2d0f0f; color: #f87171; border: 1px solid #7f1d1d; }
.danger-tag   { background: #450a0a; color: #f87171; border: 1px solid #991b1b; font-weight: 900; }
.problem-box {
  background: #150808;
  border: 1px solid #7f1d1d;
  border-left: 3px solid #ef4444;
  border-radius: 7px;
  padding: 0.75rem 0.9rem;
}
.problem-title {
  font-size: 0.55rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #f87171;
  margin-bottom: 0.45rem;
}
.problem-text {
  font-size: 0.67rem;
  color: #94a3b8;
  line-height: 1.8;
}
.problem-text code {
  font-family: 'Fira Code', monospace;
  color: #a5b4fc;
  font-size: 0.63rem;
}
.err { color: #f87171; font-family: 'Fira Code', monospace; font-size: 0.67rem; }
</style>
