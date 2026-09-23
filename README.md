# METRIC — 發行與下載

本 repo **僅存放發行物**，不含原始碼。

- 安裝檔：[Releases](../../releases)
- 下載頁：<https://mlmicocake-beep.github.io/metric-releases/>

## 檔案

每個版本包含兩種安裝檔，各附一份原廠簽章清單：

| 檔案 | 安裝位置 |
|---|---|
| `Metric-Setup-<版本>.exe` | 使用者電腦 |
| `MetricPortalSetup-<版本>.exe` | 企業伺服器（由 IT 管理） |
| `*.exe.manifest.json` | 對應的簽章清單，與 exe 成對 |

## 簽章清單的作用

METRIC Portal 以系統權限執行，而「套用更新」即執行上傳的安裝檔。
Portal 僅安裝經**原廠 Ed25519 私鑰簽署**的檔案：清單遭竄改、exe 遭替換或下載來源遭冒充，
均會在安裝前被阻擋。私鑰僅存放於原廠機器，不在本 repo，亦不包含於任何出貨物。

因此 **exe 與 `.manifest.json` 必須成對上架**。缺少清單時，客戶環境中的 Portal 將無法安裝該檔案。

## IT 安裝說明

此處下載的安裝檔**未內建入口位址**，安裝時會詢問要連線的伺服器。
若企業內部已部署 METRIC Portal，自該 Portal 下載頁取得的安裝檔已內建位址，
無須手動輸入，請優先使用。

## 驗證下載的檔案

```
certutil -hashfile Metric-Setup-<版本>.exe SHA256
```

計算結果應與 `.manifest.json` 中的 `sha256` 一致。
