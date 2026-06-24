# Angular Module Federation 踩坑實錄：我以為 shared 了就沒事了

## 講者：安東尼（Antonio）

### 講者介紹

我是 Antonio，目前任職於新創醫療資訊系統公司，主要負責 Angular 前端開發與系統架構設計。近年持續投入 Micro Frontends、Webpack Module Federation 與前端相關技術研究。

曾榮獲 2025 iThome 鐵人賽佳作，並以核心開發人員身分參與 2025 WebConf 的籌備與建置，同時擔任 2025 六角學院前後端專題班教練與前端班課程講師，在技術實踐之外，也持續投入社群教學與人才培育。

一路從實務開發到大型系統維護，踩過不少 shared scope、runtime error 與 webpack optimization 的坑，也因此開始深入研究 Module Federation 在實際專案中的運作方式與限制。希望透過這次分享，將自己在 Angular Module Federation 上的實戰經驗整理出來，幫助更多開發者少走一些彎路。

- 個人部落格：https://ling-jun-hao.github.io/Blog/
- 鐵人賽：https://ithelp.ithome.com.tw/users/20116554/ironman/7800

### 主題

Angular Module Federation 踩坑實錄：我以為 shared 了就沒事了

### 議程大綱

- 微前端與 Module Federation 核心概念
- Angular secondary entry point 的 shared 共用踩坑
- Webpack Production optimization 如何影響 Module Federation
- 微前端架構下的樣式隔離與樣式污染問題

### 推薦受眾

- Angular 前端工程師
- 正在導入 Micro Frontends 的團隊
- 對 Webpack Module Federation 有興趣的開發者
