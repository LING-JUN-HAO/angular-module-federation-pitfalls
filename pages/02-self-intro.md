---
layout: center
---

<div class="intro-wrap">

<img src="/profile.jpg" class="intro-avatar" />

<div class="intro-text">
  <div class="intro-name">Antonio（安東尼）</div>
  <div class="intro-role">前端工程師 · 新創醫療公司</div>

  <div class="intro-subhead">相關經歷</div>
  <div class="intro-highlights">
    <div class="intro-highlight">2025 iThome 鐵人賽佳作</div>
    <div class="intro-highlight">WebConf 網站開發人員</div>
    <div class="intro-highlight">六角學院課程講師</div>
  </div>
</div>

</div>

<style>
.intro-wrap {
  display: flex;
  align-items: center;
  gap: 1.5rem;
}
.intro-avatar {
  width: 140px;
  height: 140px;
  border-radius: 50%;
  object-fit: cover;
  object-position: center top;
  border: 2px solid #1e3a5f;
  flex-shrink: 0;
}
.intro-text {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}
.intro-name {
  font-size: 2rem;
  font-weight: 800;
  color: #f1f5f9;
}
.intro-role {
  font-size: 0.95rem;
  color: #94a3b8;
}
.intro-tag {
  display: inline-block;
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.06em;
  color: #7dd3fc;
  background: #0c1a30;
  border: 1px solid #1e4a6e;
  border-radius: 6px;
  padding: 0.2em 0.7em;
  margin-top: 0.2rem;
  align-self: flex-start;
}
.intro-subhead {
  font-size: 0.6rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #475569;
  margin-top: 0.55rem;
}
.intro-highlights {
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
  margin-top: 0.35rem;
}
.intro-highlight {
  font-size: 0.78rem;
  color: #cbd5e1;
  padding-left: 1rem;
  position: relative;
}
.intro-highlight::before {
  content: '•';
  position: absolute;
  left: 0;
  color: #7dd3fc;
  font-weight: 700;
}
</style>
