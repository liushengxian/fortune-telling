# 见喜 · 一签好光景

Vue 3 + Vite 的移动优先求签页面。宣纸色调、木质签筒、摇签与揭签动画；六种签文全部为上签（吉）或上上签（大吉）。

## 本地运行

```sh
npm install
npm run dev
```

## 生产构建

```sh
npm run build
npm run preview
```

在 `src/App.vue` 的 `fortunes` 中编辑签诗、签意和寄语，在 `src/style.css` 中修改视觉样式。页面支持键盘操作和系统减少动画设置。在线字体加载失败时使用系统宋体。

签文仅供娱乐。

## GitHub Pages 部署

1. 在仓库 **Settings → Pages → Build and deployment → Source** 中选择 **GitHub Actions**。
2. 将配置提交并推送到 `main`，工作流会自动安装依赖、构建并部署 `dist`。也可以在 Actions 中手动运行 **Deploy to GitHub Pages**。
3. 部署成功后访问 https://liushengxian.github.io/fortune-telling/ 。

生产构建的 Vite `base` 已设为 `/fortune-telling/`，本地开发仍使用 `/`。若更改仓库名称或使用独立域名，请同步调整 `vite.config.js` 的 `base`。


使用 `npm run build` 和 `npm run preview` 验证生产构建时，访问预览地址下的 `/fortune-telling/` 路径。
