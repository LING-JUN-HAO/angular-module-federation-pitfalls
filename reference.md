[TOC]

:::spoiler 常用指令

> - ng build mfe-app --configuration development
> - ng build mfe-app --configuration production
>   可以 `-c`
> - ng serve mfe-app -c development
> - ng serve host-app -c production
>   :::

## 微前端是什麼？

![ChatGPT Image Jun 3, 2026, 12_38_44 AM (1)](https://hackmd.io/_uploads/HyPBPt3xGe.png)

微前端（Micro Frontends）是一種前端架構風格，透過將單體前端應用拆分為多個可獨立開發、部署與運行的子應用，從而提升系統的可維護性與團隊自治能力。

其概念源自微服務架構（Microservices），強調以功能（vertical slice）為單位劃分責任，使各團隊能夠從前端 UI 到後端服務進行端到端（end-to-end）的開發與管理。

對使用者而言，整體體驗仍是一個完整的網站；而對開發者而言，這種架構的目的在於解耦前端應用，提升開發效率與團隊協作彈性。

**借鏡相關的設計概念：**

- 領域驅動設計 (domain-driven design)

## 常見案例

- Amazon MicroFrontend

## 實踐方式

- Webpack Module Federation：Host App 和 Remote Apps 都 import 同一個 shared library 裡的 Angular Service，並且透過 Module Federation 設成 singleton，讓大家共用同一個 service instance。
  ======> CustomEvent 也是一種方式
- Web Components
- iframe：不同源會透過 postMessage 的方式溝通
- Single SPA
- Client Side 利用 JavaScript 載入模組

## 微前端想解決什麼問題？

> 微前端想解決的核心問題，不只是技術層面，更多是組織層面的挑戰。

隨著公司規模成長，前端應用往往由多個團隊共同維護。當所有人都在同一個 codebase 上工作，問題就會開始浮現：

**單體架構的痛點：**

- 共享元件一旦異動，所有引用頁面都可能連帶崩潰，影響範圍難以預測
- 各團隊功能須等所有人完成後才能一併上版，互相等待、阻塞部署流程
- 任何功能異動都需要重新打包並部署整個應用，即使只是一個小功能的修改
- 隨著專案規模擴大，每次 build 的時間也不斷拉長，開發效率持續下降

這些問題的根源在於：架構與組織結構沒有對齊。訂單團隊、用戶團隊、金流團隊，各自負責不同的業務領域，卻被迫共用同一套程式碼與部署流程，導致彼此高度耦合、互相牽制。

微前端的目標，是讓每個團隊能夠真正獨立地開發、測試與部署自己負責的功能，不需要等待其他團隊，也不會因為別人的異動而受到影響，讓技術架構真實反映團隊的分工方式。

**微前端優點：**

- 各業務領域團隊的前端開發彈性提升：團隊可自主選擇使用的框架（雖然實務上通常仍會維持一致），並可依據模組需求自行規劃業務邏輯的開發方式。
- 部署可獨立運行：當子應用需要更新時，可依團隊自身需求設定 CI/CD 檢查流程，並獨立完成發布，無需等待其他應用完成後才能一併上版。
- 故障隔離：當某個子應用發生異常時，影響範圍通常可控制在該子應用內，不會直接導致其他子應用或整個系統無法運作，提升系統整體穩定性。

## Webpack Module Federation 運作機制

webpack 以 Node.js 的 CommonJS 模組機制為設計基礎，在瀏覽器環境中實作出一套等效的模組系統。所有模組的載入、快取與執行，以及 chunk 的動態切割與非同步載入，都圍繞著 `__webpack_require__ `這個核心物件運作。

**Webpack Module Federation 的運作機制可以歸納為四個層次：**

- **Module Registry（模組快取中心）**
  無論是本地模組或遠端模組，只要曾經載入過，結果就會被快取在 `__webpack_module_cache__` 中，後續存取直接回傳快取，不會重複執行。
- **Async Chunk Loader（非同步 chunk 載入器）**
  負責動態載入遠端模組。`__webpack_require__.e()` 作為協調層，統一等待所有相關工作完成；`__webpack_require__.l()` 作為執行層，透過動態插入 `<script>` tag 發起網路請求，待 chunk 程式碼執行完畢後通知 `__webpack_require__.e()` resolve，整個載入流程才算完成。
- **Container API 橋接（init / get）**
  Host 載入 Remote 的 remoteEntry.js 後，會呼叫 init() 並將自身的 shared scope 物件傳入，讓 Remote 得以比對雙方的套件版本。get() 則將指定模組封裝成工廠函式（factory function）回傳給 Host，Host 呼叫後才真正執行模組程式碼。
- **Shared Scope 版本協商（singleton 管理）**
  `__webpack_require__.S` 是一個跨應用共享的物件，記錄每個套件的版本號、提供方與是否已被使用。Host 和 Remote 雙方都會將自己擁有的套件版本註冊進來，已被載入使用的版本具有最高優先權，不會被後續註冊的版本覆蓋。

## @angular-architects/module-federation/webpack 介紹

主要是讓 Angular CLI 專案可以使用 Webpack Module Federation。它會產生 federation 設定檔，並安裝 custom builder，讓 Angular 可以支援 Host / Remote 架構

### 存在哪些角色？

| 角色           | 說明                                         |
| -------------- | -------------------------------------------- |
| Host / Shell   | 主應用，負責載入 Remote                      |
| Remote         | 被載入的子應用或模組                         |
| exposed module | Remote 對外提供的 Feature Module / Component |
| remoteEntry.js | Remote 的入口清單，Host 透過它知道要載入什麼 |

### 基礎使用情境

**Remote 端：公開自己的 routes**

- name：Remote 的識別名稱，也就是 Module Federation Container 的名稱，用來讓 Host 識別並載入該 Remote 所暴露的模組
- exposes：定義 Remote 對外公開的模組，讓 Host 可以透過指定的公開名稱載入並使用。

```ts=
// remote/webpack.config.js

const { withModuleFederationPlugin } = require('@angular-architects/module-federation/webpack');

module.exports = withModuleFederationPlugin({
  name: 'mfe1',

  exposes: {
    './routes': './src/app/app.routes.ts',
  },
});
```

**代表的意思**

```
Host 可使用的名稱      Remote 內部實際檔案
./routes          →    ./src/app/app.routes.ts
```

**app.routes.ts 匯出內容：Remote 實際對外提供的檔案內容。Host 載入 exposedModule 後，可透過 .then(m => m.routes) 取得這裡匯出的 routes。**

```ts=
// remote/src/app/app.routes.ts

import { Routes } from '@angular/router';
import { ProductListComponent } from './product-list/product-list.component';

export const routes: Routes = [
  {
    path: '',
    component: ProductListComponent,
  },
];
```

**Host 端：載入 Remote 的 routes**

- type：指定 Remote 的載入格式，例如 module 表示以 ES Module 方式載入
- remoteEntry：Remote 對外提供的入口檔位置，Host 會透過它取得 Remote 暴露出的模組資訊
- exposedModule：指定要載入 Remote 中哪一個被公開的模組名稱，需對應 Remote exposes 裡定義的 key

```ts=
// host/src/app/app.routes.ts

import { Routes } from '@angular/router';
import { loadRemoteModule } from '@angular-architects/module-federation';

export const routes: Routes = [
  {
    path: 'products',
    loadChildren: () =>
      loadRemoteModule({
        type: 'module',
        remoteEntry: 'http://localhost:4201/remoteEntry.js',
        exposedModule: './routes',
      }).then(m => m.routes), // 取得該模組中實際匯出的 routes
  },
];
```

## 微前端問題 1 - Host 未使用套件子路徑，導致 Remote 無法共用該模組

在大型套件中，通常會透過多個子路徑提供不同功能，讓開發者可以依照需求匯入特定模組。以 Angular 20 為例，若需要將 RxJS 與 Angular Signals 進行轉換，很可能會使用到橋接函數 toSignal。

```
// 從 @angular/core 的子路徑 rxjs-interop 匯入 toSignal 函數
import { toSignal } from "@angular/core/rxjs-interop";
```

**host 結構**

webpack 參數：

- singleton：確保共用模組在整個應用中只會有一個實例
- strictVersion：是否嚴格檢查版本（false 表示只要版本相容即可，不強制完全符合）
- requiredVersion：自動從 package.json 讀取該套件的版本作為 requiredVersion

這些設定的主要目的，是在遇到不同版本的模組實例時，能夠妥善處理版本相容性問題。

一般情況下，host 會優先載入，並將其 shared 的套件統一註冊到 `shared scope` 中供其他模組共用。當 remote 端也設定 `singleton: true` 時，會到 `shared module` 中尋找 host 已提供的版本，確認是否可共用。

---

**webpack config：**

shareAll：shareAll 會將 package.json 中的 dependencies 自動加入 shared 設定，表示這些套件可被 host 與 remote 共用，不需要逐一手動列出。

```ts=
const { withModuleFederationPlugin, shareAll } = require('@angular-architects/module-federation/webpack');

// 依據 package.json 中定義的版本，自動推斷並設定 shared module 的相容版本
module.exports = withModuleFederationPlugin({
  shared: {
    ...shareAll({ singleton: true, strictVersion: false, requiredVersion: 'auto' }),
  }
});
```

即使已經安裝該套件，也不代表它一定會被 Webpack 打包進 bundle。實際是否會被打包，仍取決於該模組是否有被 import 或實際使用。

因此，在 Module Federation 的情境下，即使 host 本身沒有直接使用到該模組，仍可能需要主動 import，確保它被納入 bundle 並註冊到 shared scope 中。否則當 remote 嘗試從 shared scope 取得共用套件時，可能會找不到對應的 shared module，進而導致 runtime 錯誤。

:::info
**衍生問題**

- Host 的 bundle 變大
- 程式碼語意不清楚，看不出這個 import 是業務需要還是為了共用
  :::

---

remote webpack 設定檔：

```ts=
const { withModuleFederationPlugin, share } = require('@angular-architects/module-federation/webpack');

module.exports = withModuleFederationPlugin({

  name: 'mfeApp',

  exposes: {
    './routes': './src/app/app.routes.ts',
  },

  shared: share({
    '@angular/animations': { singleton: true, strictVersion: false, requiredVersion: 'auto' },
    '@angular/common': { singleton: true, strictVersion: false, requiredVersion: 'auto' },
    '@angular/common/http': { singleton: true, strictVersion: false, requiredVersion: 'auto' },
    '@angular/core': { singleton: true, strictVersion: false, requiredVersion: 'auto' },
    '@angular/router': { singleton: true, strictVersion: false, requiredVersion: 'auto' },
    'primeng': { singleton: true, strictVersion: false, requiredVersion: 'auto' },
    'primeicons': { singleton: true, strictVersion: false, requiredVersion: 'auto' },
    rxjs: { singleton: true, strictVersion: false, requiredVersion: 'auto' },
    tslib: { singleton: true, strictVersion: false, requiredVersion: 'auto' },
  })

});
```

remote 需要使用 @angular/core/rxjs-interop 套件時，發生 inject context error：

![image](https://hackmd.io/_uploads/rk7IxmnRWl.png)

**觀察打包後的 host/main.js 可以發現：**

Host 有成功註冊 @angular/core，但沒有註冊 @angular/core/rxjs-interop。

![image](https://hackmd.io/_uploads/HkIGemnAZx.png)

這會導致 Remote 在 runtime 查詢 shared scope 時，找得到 @angular/core 的共用實例，卻找不到 @angular/core/rxjs-interop 對應的 shared module。此時 @angular/core 可能來自 shared scope，而 @angular/core/rxjs-interop 則可能 fallback 到 Remote 自行打包的版本，造成來源不一致。

由於 @angular/core/rxjs-interop 是 @angular/core 套件底下的 secondary entry point，且執行時會依賴 Angular Core 的 runtime / DI context，因此若兩者沒有透過一致的 shared scope 解析，就可能在 runtime 產生錯誤或非預期行為。

---

原本預期的結果：angular/core & angular/core/rxjs-interop 共用同一個 DI context，但結果卻不是這樣。

解決方式：在 host 端加入 side-effect import，強制讓 webpack 看見並打包 `@angular/core/rxjs-interop`，讓它能進入 Module Federation shared scope。

(host)app.routes.ts

```ts=
import { Routes } from "@angular/router";
import { loadRemoteModule } from "@angular-architects/module-federation";
import "@angular/core/rxjs-interop"; // 為了被 webpack 打包而載入

export const routes: Routes = [
  {
    path: "",
    loadChildren: () =>
      loadRemoteModule({
        type: "module",
        remoteEntry: "http://localhost:4201/remoteEntry.js",
        exposedModule: "./routes",
      })
        .then((m) => m.routes)
        .catch((err) => {
          console.log("error loading remote routes", err);
          return [];
        }),
  },
];
```

再次觀察 host 打包後的結果：

可以確認 @angular/core 與 @angular/core/rxjs-interop 都已成功註冊到 shared scope 中。

![image](https://hackmd.io/_uploads/Syl7bQnRZx.png)

---

:::success
如果只是要在 host build 後的 main.js 中觀察某個具名匯入是否被保留下來，要注意 TypeScript compiler 可能會自動移除未使用的 import，導致 build output 裡看不到該 import。

可以在 tsconfig 加上以下設定，讓 沒有 type 修飾的 import/export 保留在 JS 輸出中；有 type 修飾的則會被移除。

```ts=
{
  "compilerOptions": {
    "verbatimModuleSyntax": true,
  },
}
```

如果匯入的內容只是型別用途，則需要明確使用：

```ts=
import type { SomeType } from './somewhere';
```

:::

**問題發生流程**

```
@angular/core/rxjs-interop 走 fallback 自己載入
→ 它內部的 @angular/core 依賴在這個 fallback chunk 裡
  是用 __webpack_require__ 直接 require
→ 不是去 shared scope 拿 Host 的那份
→ 兩份 @angular/core instance 並存
→ inject context error
```

---

另一種方式是直接透過 console 查看 Module Federation 的 share scope object，不過需要等初始化完成後才能正確取得內容。

**以 host main.ts 為例**

```ts=
// main.ts
declare const __webpack_share_scopes__: any;

import("./bootstrap")
  .then(() => {
    // bootstrap 完成後，shared scope 已經初始化
    console.log(
      "@angular/core =>",
      __webpack_share_scopes__?.default?.["@angular/core"],
    );

    console.log(
      "@angular/core/rxjs-interop =>",
      __webpack_share_scopes__?.default?.["@angular/core/rxjs-interop"],
    );

    console.log("All =>", __webpack_share_scopes__?.default);
  })
  .catch((err) => console.error(err));
```

**console 查看結果**

![image](https://hackmd.io/_uploads/Bkpi-7kJMx.png)

## 微前端問題 2 - Webpack optimization 設定（ concatenateModules ）

webpack 的 Optimize 階段做 scope hoisting，它在 build time 把多個模組「展開」合併進同一個 scope，執行順序由靜態依賴圖決定。這個做法本身是正確的，因為它分析的是同步的依賴關係。

Module Federation 則在 runtime 插入了一個 `await——__webpack_init_sharing__`。這個 async 點的存在是結構性的，不管有沒有 host 在場都一樣，因為 Federation 的設計就是要在 runtime 保留一個協商窗口。

問題出在這兩件事疊在一起的時候：scope hoisting 把 AppModule 的初始化 code 展開成同步執行，但它跨越了那個 await。結果 AppModule 的 code 在 await 還沒完成、shared 模組還沒就緒的情況下就跑了，這才是 runtime error 的根本原因。

---

**為什麼沒加進 shared 就會炸**

沒有被列進 shared 的套件，Federation 不管它，webpack 用一般的 `__webpack_require__` 同步載入。問題是這個同步載入發生在 Federation 的 async 協商過程，誰先被 require 誰就先跑，內部的載入順序完全取決於觸發時機，而這個時機在協商過程中是不確定的。

以 RxJS 為例，BehaviorSubject 需要 Subject 已經存在才能繼承，但在協商過程中 RxJS 被觸發載入時，Subject 可能還沒就緒，繼承到的就是 undefined，直接炸掉。

---

**為什麼加進 shared 就沒事**

加進 shared 之後，Federation 接管這個套件的生命週期——在協商階段就把它登記進 shared scope，等協商完成後才把實例交出去。所以當 @angular/core 或其他套件去 require 它的時候，它一定已經就緒了，載入順序有保障。

---

**實務上的影響**

所有 Angular 核心套件之間都有相互依賴，只要有一個不在 shared 裡，就可能在協商過程中以錯誤的順序被載入。症狀通常是 Class extends value undefined，而且會一個一個浮出來——補一個，換下一個炸。解法是把所有核心套件一次補齊：@angular/platform-browser、@angular/platform-browser-dynamic、rxjs、rxjs/operators 都需要加進去。

https://github.com/angular-architects/module-federation-plugin/issues/1053

![image](https://hackmd.io/_uploads/BkjmSZ4ezl.png)

---

- 當時：@angular-architects/module-federation": "21.2.0",
- 對應 webpack 版本：webpack@5.101.2
- CVE 警告：
  - https://osv.dev/vulnerability/GHSA-38r7-794h-5758
  - https://osv.dev/vulnerability/GHSA-8fgc-7cc6-rx7x

gitlab commit 紀錄：
https://gitlab.aservice.com.tw/hepiuscare/frontend/apps/opd-order/-/commit/88b0f1eedfcd078b6dd9e48ac5de52f2d80a3173

---

git commit 紀錄：

https://gitlab.aservice.com.tw/hepiuscare/frontend/apps/opd-order/-/commit/4c117233bc79d0685e9e8c02b79c863e14325919

- 目前 @angular-architects/module-federation : "20.0.0"

本來依賴 webpack 5.105.0，被降版至 5.104.1

---

## 補充資料

### 已看

- https://medium.com/starbugs/%E5%BE%AE%E6%9C%8D%E5%8B%99%E5%BE%88%E5%A4%AF-%E9%82%A3%E4%BD%A0%E6%9C%89%E8%81%BD%E9%81%8E%E5%BE%AE%E5%89%8D%E7%AB%AF%E5%97%8E-%E5%88%9D%E6%8E%A2-micro-frontends-%E6%9E%B6%E6%A7%8B-e0a8469be601
- https://codelove.tw/@tony/post/qepY7a
- https://www.explainthis.io/zh-hant/swe/micro-frontend-guide

### 未看

- https://micro-frontends.org/?source=post_page-----e0a8469be601---------------------------------------
- https://dev.to/brandonvilla21/micro-frontends-module-federation-with-webpack-5-426?source=post_page-----e0a8469be601---------------------------------------
- https://github.com/naltatis/micro-frontends-in-action-code
- https://juejin.cn/post/7197977396602912826
- https://github.com/phodal/microfrontends
