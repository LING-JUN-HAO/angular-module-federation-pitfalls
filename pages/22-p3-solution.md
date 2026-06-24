---
layout: none
---

<div class="slide-wrap">

<div class="slide-header">
  <h1 class="slide-title">風險方案：把 Tailwind 塞進 Component Scoped CSS</h1>
  <div class="slide-subtitle">能讓 CSS 跟著 component JS 一起被 Module Federation 載入，但會受 Angular View Encapsulation 限制，存在樣式失效風險</div>
</div>

<div class="content-row">

<div class="left-col">

  <div class="section-card">
    <div class="mini-label">機制</div>

<div class="mechanism-text">

把 <code>tailwind.css</code> 放進最外層 component 的 <code>styleUrl</code>，Angular 會把這份 CSS 編進該 component 的 lazy chunk；當 Host 透過 <code>remoteEntry.js</code> 載入這個 component chunk 時，CSS 也會跟著 component 一起進入 runtime。

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

Lazy load 這個 component 時，<code>styleUrl</code> 會跟著 component 一起被打包：

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

  <div v-click class="section-card">
    <div class="mini-label">期望：CSS 跟著 component JS 一起被載入</div>
    <ul class="bullet-list">
      <li>Module Federation 仍以載入 JS chunk 為主，Tailwind CSS 透過 component <code>styleUrl</code> 進入該 chunk，不需要額外 expose 或 import style module</li>
      <li>交由 Angular View Encapsulation 處理，每條規則自動帶 <code>_ngcontent-*</code> scope，樣式隔離是內建的，不用額外設計機制</li>
    </ul>
    <img src="/devtools-ngcontent.png" class="devtools-img devtools-img-top" />
  </div>

  <div v-click class="section-card mt-sm">
    <div class="mini-label warn-label">風險</div>
    <div class="result-text">Tailwind 放進 component <code>styleUrl</code> 後，會變成受 component 邊界限制的 scoped CSS，外層 component 本身能吃到，但子元件、動態內容、投影內容會落在邊界之外，吃不到 Tailwind 樣式。</div>
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
