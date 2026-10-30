# 免費公開 Beta 發布流程

本文件是維護者的發布檢查表。**目前沒有自動公開 APK 的 workflow；提交文件、合併 PR 或建立本倉庫 tag 都不會自行產生 APK。**

## 1. 準備候選版本

- 每版集中處理清楚的問題，保留既有中文輸入及鍵盤內語音功能。
- 在開發倉庫完成程式碼審查、回歸檢查及實機測試，再選定不可變的來源 commit。
- 公開 Beta 使用非除錯組態，核對 APK 最終的 `debuggable=false`，不能只把檔名的 `debug` 刪去。
- 保留套件識別及已確定的簽署身份；如有簽署遷移，需要先完成相容更新方案，不能任意換 key。
- versionCode 必須比已發出的所有相容安裝版本高。

## 2. 版本名稱

範例：`versionName=1.7.204-beta.1`、tag `v1.7.204-beta.1`、APK `Q-Meow-1.7.204-beta.1.apk`。

以上只作格式示範，並未分配或發布該版本。實際 versionCode 由開發版本紀錄選定，不能從公開倉庫的文件版本推算。

開發倉庫的版本 tag 指向 app 原始碼；本下載倉庫的同名 tag 指向對應的文件快照。兩者 commit 不同，須在編譯資料中另記 app 的來源 commit。

## 3. 需要通過的檢查

- [ ] 所有適用的回歸檢查通過，包含語音重試上限、取消及欄位切換。
- [ ] 全新安裝、覆蓋更新、基本中文／英文／符號輸入正常。
- [ ] 語音確實出字，長文及停止操作正常，取消後錄音釋放。
- [ ] 記錄已測試的手機、Android、辨識引擎、語言與網絡情況。
- [ ] 未測試項目及仍存在的問題清楚列出，沒有把單一手機成功當作全平台成功。
- [ ] APK 簽署驗證通過；套件、版本、CPU／Android 支援及非除錯狀態符合候選版本。
- [ ] 授權、第三方告示及資料處理說明與 APK 相符。
- [ ] 沒有在公開附件放入設定備份、API key、token、keystore 或簽署私鑰。

## 4. 編譯及附件

在開發倉庫的 Actions 產生並驗證候選 APK。首輪以人工上載到本倉庫的 Draft Release；後續如加入跨倉庫自動化，另行設定只限所需倉庫權限的憑證，不能假設一般 workflow token 可跨倉庫寫入。

每版至少附上：

- 經驗證的 APK。
- `SHA256SUMS.txt`：列出該 APK 的 SHA-256。
- `BUILD_INFO.txt`：套件名稱、versionName、versionCode、app 來源 commit、編譯 run、APK 檔名／大小、簽署憑證 SHA-256，以及驗證結果。憑證指紋可以公開，私鑰不可公開。
- 適用的授權／第三方告示；如有相應原始碼提供要求，另附所需材料。

Drive 保存 APK、對應原始碼及編譯資料作維護者備份；不要把含私人資料或秘密的備份連結當作公開下載入口。

## 5. 建立草稿

1. 在本倉庫的 Releases 選擇建立新 Release。
2. 使用與 app versionName 對應的 `v…` tag，目標為已核對的文件 commit。
3. 依 [Release 說明模板](../.github/RELEASE_TEMPLATE.md) 填寫真實資料並上載附件。
4. 標示為 **Pre-release**，先儲存 Draft。
5. 再次核對 APK 檔名、SHA-256、版本、簽署指紋、測試記錄及已知問題。

## 6. 公開

完成草稿核對後才按 Publish release。之後更新 README 的當期 Beta 連結，從未登入 GitHub 的瀏覽器驗證 APK 可下載，再進行一次安裝及更新抽查。

使用實際 Beta tag／Release 連結作下載入口；不要把正式版的 latest 路線當作 Beta 頻道。已發布的同一 tag 不應換成另一份 APK；有變更就使用新版本及新 tag。

## 7. 回報及修復

- 在 Issues 收集裝置、版本、錯誤碼及可重現步驟，先刪除個人資料。
- 嚴重問題要在 Release 頂部標明並停止推薦該版本，保留驗證資料以便追查。
- 修復版本使用較高 versionCode 及相容簽署。Android 會阻擋一般的較低版本覆蓋安裝，不以「解除安裝／清除資料」作預設回退方法。
- 如需採用較早的可用程式碼，先確認資料相容，再編成新的較高版本修復版。

參考：

- [GitHub Releases](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)
- [管理 Release](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)
- [Android app 版本](https://developer.android.com/studio/publish/versioning)
- [Android app 簽署](https://developer.android.com/studio/publish/app-signing)
