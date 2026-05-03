# AI / CoWoS 情報

GitHub Pages / Hugo 專案，用於發布 AI infrastructure、CoWoS、先進封裝、半導體供應鏈與模型實驗室情報。

- Site: https://promatw.github.io/ai-intel/
- Repo: https://github.com/promatw/ai-intel
- Theme: Hugo PaperMod via Hugo Modules
- Publish path: `content/posts/`
- State files: `data/state.json`, `data/monthly_state.json`, `data/published_urls.json`

## Local setup

本機若已安裝 Hugo Extended 與 Go：

```powershell
hugo mod get -u
hugo server
```

GitHub Actions 會在 push 到 `main` 時自動建置並部署到 GitHub Pages。
