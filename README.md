# 運轉人生 DriverHelper 下載網站

此 repository 是 DriverHelper 的手機優先單頁下載網站。網站採純 HTML、CSS 與少量 JavaScript，不需安裝套件或執行建置指令；GitHub Pages workflow 會直接部署 repository 根目錄的內容。

## 本地預覽

在 repository 根目錄執行：

```bash
python3 -m http.server 8000
```

然後開啟 <http://localhost:8000>。也可以直接開啟 `index.html`，但使用本地伺服器會更接近 GitHub Pages 的實際行為。

## 啟用 GitHub Pages

1. 將網站檔案與 `.github/workflows/deploy-pages.yml` 推送到 `main` 分支。
2. 在 GitHub repository 開啟 **Settings → Pages**。
3. 在 **Build and deployment** 的 **Source** 選擇 **GitHub Actions**。
4. 開啟 **Actions**，等待 `Deploy DriverHelper download site to GitHub Pages` workflow 完成。

此 repository 的預期網址為：

```text
https://richman6842.github.io/DriverHelper-Download/
```

一般 GitHub Pages 專案網站的網址格式為：

```text
https://<GitHub 使用者名稱>.github.io/<repository 名稱>/
```

若之後設定自訂網域，請以 GitHub Pages 顯示的正式網址為準，並同步更新 `index.html` 內的 `og:url`。

## 產生正式網站 QR Code

頁面目前刻意只顯示 QR Code 預留區，沒有放置假 QR Code，也沒有讓 QR Code 指向 APK。部署完成並確認正式網站網址可開啟後：

1. 複製 GitHub Pages 的正式網址，例如 `https://richman6842.github.io/DriverHelper-Download/`。
2. 使用可信任的 QR Code 產生工具，內容只填入這個網站網址。
3. 產生後先用手機實際掃描，確認開啟的是本下載網站。
4. 將 QR Code 匯出為 PNG，存成 `assets/site-qr.png`。
5. 將 `index.html` 內的 `.qr-placeholder` 區塊替換成：

   ```html
   <img class="site-qr" src="assets/site-qr.png" alt="掃描前往 DriverHelper 官方下載網站">
   ```

6. 將「部署完成後產生網站 QR Code」改成「掃描前往 DriverHelper 官方下載網站」。

QR Code 必須永遠指向網站網址，不要指向 Google Drive。如此未來更換 APK，只需修改網站，不必重印 QR Code。

若電腦已安裝 Node.js，也可用一次性指令在 repository 根目錄產生 QR Code（套件不會加入專案依賴）：

```bash
mkdir -p assets
npx --yes qrcode "https://richman6842.github.io/DriverHelper-Download/" -o assets/site-qr.png -w 800 -m 2
```

## 更換 APK 下載連結

目前 Google Drive 檔案使用直接下載格式：

```text
https://drive.google.com/uc?export=download&id=1AZrdsb7CI27t1ArczS1wP2CDYLUR1b2w
```

未來上傳新版 APK 後：

1. 確認 Google Drive 分享權限允許持有連結者存取。
2. 取得新檔案 ID，組成 `https://drive.google.com/uc?export=download&id=新檔案ID`。
3. 在 `index.html` 搜尋 `data-download-link`；把兩個下載按鈕的 `href` 都換成新網址。
4. 本地測試兩個下載按鈕後，推送到 `main`，GitHub Actions 會自動更新網站。

網站網址不變，因此既有 QR Code 不需要重新製作。

## 檔案說明

- `index.html`：頁面內容、下載連結與基本社群分享資訊
- `styles.css`：黑金主題、手機優先響應式版面與無障礙樣式
- `script.js`：FAQ 單一展開行為與頁尾年份
- `assets/driverhelper-promo.jpg`：首頁品牌主視覺與社群分享預覽圖
- `.nojekyll`：讓 GitHub Pages 直接提供靜態檔案
- `.github/workflows/deploy-pages.yml`：位於 repository 根目錄的自動部署 workflow
