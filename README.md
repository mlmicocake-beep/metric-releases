# METRIC — 發行與下載

本 repo **僅存放發行物與價目表**，不含產品原始碼。

- 安裝檔：[Releases](../../releases)
- 下載頁：<https://mlmicocake-beep.github.io/metric-releases/>

## 檔案

每個版本以 `v<Portal 版本>-agent<Agent 版本>` 標記（例如 `v3.0.5-agent0.21.5.7`），
包含下列檔案，每個檔案各附一份原廠簽章清單：

| 檔案 | 用途 |
|---|---|
| `MetricPortalSetup-<版本>.exe` | 企業伺服器（由 IT 管理），**內含同版本 Agent**；首次安裝請使用此檔 |
| `MetricPortalSetup-<版本>-portal-only.exe` | 僅升級 Portal，不含 Agent；不可用於全新安裝 |
| `Metric-Setup-<版本>.exe` | 使用者電腦的 Agent 獨立安裝檔 |
| `metric-source-<版本>.zip` | Agent 就地更新套件，由 Portal 派送 Agent 更新時使用，無須手動下載 |
| `*.manifest.json` | 對應的簽章清單，與檔案成對 |

部分新功能須 Portal 與 Agent 同時更新至同一版本標記方可使用，
例如 3.0.5／0.21.5.7 起的技能與外掛市集；舊版 Agent 仍可連線，沿用原有功能。

## 價目表

`pricing/model-prices.json` 為主要模型的官方計費（每筆附官方來源與核對日期）。
Portal 更新模型規格表時（自動同步或「立即更新」）一併讀取此檔，作為建議單價的來源。

## 簽章清單的作用

METRIC Portal 以系統權限執行，而「套用更新」即執行上傳的安裝檔。
Portal 僅安裝經**原廠 Ed25519 私鑰簽署**的檔案：清單遭竄改、exe 遭替換或下載來源遭冒充，
均會在安裝前被阻擋。私鑰僅存放於原廠機器，不在本 repo，亦不包含於任何出貨物。

因此 **每個檔案與其 `.manifest.json` 必須成對上架**。缺少清單時，客戶環境中的 Portal 將無法安裝該檔案。

## IT 安裝說明

此處下載的安裝檔**未內建入口位址**，安裝時會詢問要連線的伺服器。
若企業內部已部署 METRIC Portal，自該 Portal 下載頁取得的安裝檔已內建位址，
無須手動輸入，請優先使用。

## 驗證下載的檔案

```
certutil -hashfile Metric-Setup-<版本>.exe SHA256
```

計算結果應與 `.manifest.json` 中的 `sha256` 一致。
