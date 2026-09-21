# METRIC — 發行與下載

這個 repo **只放發行物**，沒有原始碼。

- 安裝檔在 [Releases](../../releases)
- 下載頁：<https://mlmicocake-beep.github.io/metric-releases/>

## 檔案

每一版會有兩種安裝檔，各自配一份原廠簽章清單：

| 檔案 | 裝在哪 |
|---|---|
| `Metric-Setup-<版本>.exe` | 使用者的電腦 |
| `MetricPortalSetup-<版本>.exe` | 公司的伺服器（IT 負責） |
| `*.exe.manifest.json` | 對應的簽章清單，與 exe 成對 |

## 為什麼要有簽章清單

METRIC Portal 以系統權限執行，而「套用更新」做的事就是執行上傳的安裝檔。
Portal 只安裝**原廠 Ed25519 私鑰簽過**的檔案：清單被改、exe 被換、或有人冒充下載來源，
都會在安裝前就被擋下來。私鑰只存在原廠機器上，不在這個 repo 裡，也不在任何出貨物裡。

所以 **exe 與 `.manifest.json` 一定要成對上架**。少了清單，客戶的 Portal 收不進去。

## 給 IT 的安裝說明

從這裡下載的安裝檔**不帶入口位址** —— 它不知道要連貴公司哪一台伺服器，安裝時會詢問。
若公司內部已經架好 METRIC Portal，從那台 Portal 的下載頁取得的安裝檔已經帶好位址，
不需要手動填寫，請優先使用。

## 驗證下載的檔案

```
certutil -hashfile Metric-Setup-<版本>.exe SHA256
```

算出來的值應與下載頁顯示的、以及 `.manifest.json` 裡的 `sha256` 一致。
