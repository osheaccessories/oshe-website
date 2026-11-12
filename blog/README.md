# Oshe 站点 — 部署与域名切换指南

**品牌**：Oshe
**公司英文全称**：Oshe Accessories Co., Ltd.
**公司中文名**：欧奢（东莞）五金有限公司
**联系邮箱**：sales@osheaccessories.com
**当前主域名**：osheaccessories.com

## 域名策略（方案 A：主站 + 跳转）

| 域名 | 现在 | 将来（拉链量产后） |
|---|---|---|
| osheaccessories.com | ✅ 主站 | 变成 301 跳转 |
| oshezipper.com | 301 跳转到主站 | ✅ 变成主站 |

切换方法：编辑 `_redirects`，把阶段 1 的规则注释、阶段 2 的规则取消注释，重新部署即可。
**内容零迁移、SEO 权重自动转移、旧域名永久保留续费。**

### ⚠️ 域名切换时必须同步处理的 5 件事（重要，别忘）

切换主域名**不只是改 `_redirects`**，以下步骤缺一不可：

1. **Cloudflare Pages 里绑定新域名**
   - 主站项目 → Custom domains → 添加 `oshezipper.com`
   - 旧域名 `osheaccessories.com` 继续保留在项目中（它要承担跳转）

2. **给 oshezipper.com 开启独立邮箱**（最容易漏！）
   - Cloudflare → oshezipper.com → Email → Email Routing
   - 建 `sales@oshezipper.com` → 转发到 163（和现在主站一样配置一遍）

3. **全站替换邮箱地址**
   - `index.html`、各产品页里的 `sales@osheaccessories.com` → `sales@oshezipper.com`
   - 检查位置：顶部栏、联系区块、悬浮邮件按钮、Schema 的 `email`、`mailto:` 链接

4. **旧邮箱继续转发，不能停**
   - `sales@osheaccessories.com` 至少保留 **2 年**
   - 老客户的邮件往来、名片、合同里印的都是旧地址，停掉就断联了

5. **同步更新各处域名引用**
   - `index.html` 的 canonical、Schema 的 `url`
   - `sitemap.xml` 全部 `<loc>`
   - `robots.txt` 的 Sitemap 行
   - 之后到 Google Search Console 重新提交新域名的 sitemap，并做 **Change of Address**（地址变更）申报

> 建议：切换前一个月先在新域名上把网站跑起来（两个域名同时可访问），确认无误后再改 `_redirects` 切换主次，这样有回退余地。

## 目录结构（按"无痛迁移 WordPress"规范设计）

```
site/
├── index.html                          首页
├── _redirects                          Cloudflare 跳转规则（域名切换开关）
├── content.md                          全站文字底稿（迁移时直接复制）
├── robots.txt                          搜索引擎抓取规则
├── sitemap.xml                         站点地图（提交给 Google）
├── images/                             图片（命名：英文关键词.webp）
├── products/
│   ├── marine-grade-zipper/index.html  ✅ 已完成（作为其他产品页的模板）
│   ├── heavy-duty-open-end/            待补内容
│   ├── outdoor-bags-zipper/            待补内容
│   └── industrial-custom/              待补内容
├── blog/                               博客（上线后逐步加）
└── about/                              工厂介绍页（待补）
```

## 上线三步

1. 把 site 文件夹里**所有内容**（不是文件夹本身，`_redirects` 也要传）上传到 GitHub 仓库
2. Cloudflare Pages 连接仓库，构建设置全部留空，Deploy
3. 在 Cloudflare Pages → Custom domains 绑定 `osheaccessories.com`，同时把 `oshezipper.com` 也添加进同一项目（`_redirects` 会自动处理跳转）

## 表单接通（10 分钟）

1. 注册 https://formspree.io （免费，50 条/月）
2. 新建 form，拿到形如 `https://formspree.io/f/abcd1234` 的地址
3. 把 `index.html` 和产品页里的 `YOUR_FORM_ID` 替换成你的 ID，删除 form 标签上的 `onsubmit=...` 属性
4. 测试提交一条，你的邮箱应立即收到

## 上线后立即做

- [ ] Google Search Console 验证 + 提交 sitemap.xml
- [ ] GA4 统计代码加入每页 `<head>`
- [ ] 把首页/产品页里的占位数据（15+ years、50M+ 等）改成真实数字
- [ ] images/ 放入真实照片（命名见 content.md 底部的清单）

## 待办记忆（到期提醒）

- [ ] **拉链量产后：切换主域名到 oshezipper.com** → 执行上方「域名切换时必须同步处理的 5 件事」

## 将来迁 WordPress 时

- 购买主机 → 一键安装 WordPress → 选主题
- 按 content.md 逐页粘贴内容，URL 保持 `/products/xxx/` 不变
- 图片直接沿用 images/ 里的文件和文件名
- 全部迁移完再改 DNS，期间旧站照常访问，**零停机、零排名损失**
