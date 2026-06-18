---
layout: none
---

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">原本以為的解決方式：把樣式變成 component scoped css</h1>
  <div class="slide-subtitle">改變 Tailwind 的撰寫方式，讓樣式跟著 component 的 JS 一起被 Module Federation 打包</div>
</div>

<div class="content-row">

<div class="left-col">

  <div class="section-card">
    <div class="mini-label">機制</div>

<div class="mechanism-text">

透過最外層 component 的 <code>styleUrl</code> 引用 <code>tailwind.css</code>，Angular 會把它編譯進該 component 的 JS chunk；Module Federation 透過 <code>remoteEntry.js</code> 解析後，再於 runtime 載入該 chunk，並套用 component-scoped 樣式。

</div>

```ts {4}
@Component({
  selector: 'app-main-layout',
  imports: [...],
  styleUrl: './tailwind.css',
  templateUrl: './main-layout.component.html',
})
```

<div class="mechanism-text mt-sm">

MainLayoutComponent 被 lazy load 進路由時，連同它的 <code>styleUrl</code> 一起被打包：

</div>

```ts {2-15}
export const routes: Routes = [
  {
    path: "main",
    loadComponent: () =>
      import("./main-layout/main-layout.component").then(
        (m) => m.MainLayoutComponent,
      ),
    children: [
      {
        path: "",
        loadComponent: () =>
          import("./home/home.component").then((m) => m.HomeComponent),
      },
    ],
  },
];
```

  </div>

</div>

<div class="right-col">

  <div class="section-card">
    <div class="mini-label">期望：CSS 能跟著 component 的 JS 一起被載入</div>
    <ul class="bullet-list">
      <li>Module Federation 只看得到 JS，樣式已經藏在 JS 裡面，不需要額外 expose 或 import 任何 style module</li>
      <li>走 Angular 自己的 ViewEncapsulation，每條規則自動帶 <code>_ngcontent-*</code> scope，樣式隔離是內建的，不用額外設計機制</li>
    </ul>
    <img src="/devtools-ngcontent.png" class="devtools-img devtools-img-top" />
  </div>

  <div class="section-card mt-sm">
    <div class="mini-label warn-label">結果</div>
    <div class="result-text">外層的 scope css 沒辦法套用到嵌套的內層組件 css，<code>_ngcontent-*</code> scope 把每個 component 的樣式各自隔開，巢狀子元件用到的 Tailwind class 還是吃不到。</div>
  </div>

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
.slide-header { margin-bottom: 0.8rem; }
.slide-title {
  font-size: 1.75rem;
  font-weight: 800;
  color: #f1f5f9;
  line-height: 1.25;
  margin: 0 0 0.4rem 0;
}
.slide-subtitle {
  font-size: 0.65rem;
  color: #64748b;
  line-height: 1.6;
}
.content-row {
  display: grid;
  grid-template-columns: minmax(0, 1fr) minmax(0, 1fr);
  gap: 2rem;
  flex: 1;
  min-height: 0;
}
.left-col {
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
  min-height: 0;
  min-width: 0;
  overflow: hidden;
}
.right-col {
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
  min-height: 0;
  min-width: 0;
  overflow: hidden;
}
.section-card {
  background: #0b1222;
  border: 1px solid #1e293b;
  border-radius: 10px;
  padding: 0.85rem 1.05rem;
}
.mini-label {
  font-size: 0.58rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: #475569;
  margin-bottom: 0.5rem;
}
.warn-label { color: #f87171; }
.mt-sm { margin-top: 0.7rem; }
.result-text {
  font-size: 0.62rem;
  color: #fca5a5;
  line-height: 1.6;
}
.result-text code {
  font-family: 'Fira Code', monospace;
  font-size: 0.56rem;
  color: #f87171;
}

.mechanism-text {
  font-size: 0.62rem;
  color: #94a3b8;
  line-height: 1.6;
  margin-bottom: 0.5rem;
}
.mechanism-text.mt-sm {
  margin-top: 0.6rem;
}
.section-card pre {
  font-size: 0.46rem !important;
  line-height: 1.3 !important;
  margin: 0 !important;
  border-radius: 6px !important;
  max-width: 100% !important;
  max-height: 12rem !important;
  overflow: auto !important;
}
.mechanism-text code {
  font-family: 'Fira Code', monospace;
  font-size: 0.56rem;
  color: #a5b4fc;
}

.bullet-list {
  margin: 0;
  padding: 0;
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}
.bullet-list li {
  position: relative;
  padding-left: 0.9rem;
  font-size: 0.6rem;
  color: #94a3b8;
  line-height: 1.55;
}
.bullet-list li::before {
  content: "";
  position: absolute;
  left: 0;
  top: 0.5em;
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background: #475569;
}
.bullet-list code {
  font-family: 'Fira Code', monospace;
  font-size: 0.54rem;
  color: #a5b4fc;
}
.devtools-img-top {
  margin-top: 0.7rem;
}
.devtools-img {
  width: 100%;
  height: auto;
  border-radius: 6px;
  border: 1px solid #1e293b;
  display: block;
}
</style>
