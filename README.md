# METRIC — 發行與下載

本 repo **僅存放發行物與價目表**，不含產品原始碼。

- 安裝檔：[Releases](../../releases)
- 下載頁：<https://mlmicocake-beep.github.io/metric-releases/>
- 產品網站與部署指南：<https://mlmicocake-beep.github.io/metric-website/>

## 現行版本

**Portal 3.2.1 ／ Agent 0.21.6+1**，標記 `v3.2.1-agent0.21.6.1`。

| 檔案 | 用途 |
|---|---|
| `MetricPortalSetup-3.2.1.exe` | 企業伺服器（由 IT 管理），**內含同版本 Agent**；首次安裝與由 3.0.6 以前升級請使用此檔 |
| `MetricPortalSetup-3.2.1-portal-only.exe` | 僅升級 Portal，不含 Agent；不可用於全新安裝 |
| `METRIC-AI-Setup-0.21.6.1.exe` | 使用者電腦的 Agent 獨立安裝檔 |
| `metric-source-0.21.6.1.zip` | Agent 就地更新套件，由 Portal 派送 Agent 更新時使用，無須手動下載 |
| `*.manifest.json` | 對應的原廠簽章清單，與檔案成對 |

每個版本以 `v<Portal 版本>-agent<Agent 版本>` 標記。Agent 版本於畫面與發行說明中顯示為 `0.21.6+1`，標記與檔名一律使用 `0.21.6.1`。
部分新功能須 Portal 與 Agent 同時更新至同一版本標記方可完整使用；舊版 Agent 仍可連線，沿用原有功能。

## 由 3.0.6 以前升級（3.0.8 起的新原廠簽章金鑰）

3.0.8 以上可直接線上更新至最新版本；以下適用於 3.0.6 以前的 Portal。

3.0.8 起，授權檔與安裝檔改用各自的新原廠簽章金鑰，舊金鑰簽發的授權檔與安裝檔不再接受。

1. **先向原廠索取新版授權檔**（`license.json`）。3.0.8 起會將舊金鑰簽發的授權視為無效：新授權上傳前，所有使用者暫停服務，也無法核准新使用者。
2. **下載最新的完整安裝檔（例如 `MetricPortalSetup-3.2.1.exe`）直接執行安裝**。Portal 3.0.6 以前的版本無法驗證本版安裝檔，因此**無法線上更新**；資料與設定保留。
3. **安裝完成後立即上傳新授權**：後台「版本與授權 → 授權與席次」上傳，立即生效，無須重新啟動。「簽章」欄顯示「現行金鑰」即完成。

完成後，後續版本可照常線上更新。

## 系統需求（Portal 以資料保存一年估算）

Portal 作業系統為 Windows Server 2019／2022／2025（x64），資料庫請放在 SSD。

| 使用人數 | 最低 | 建議 |
|---|---|---|
| 100 人 | 4 vCPU／8 GB／125 GB | 4 vCPU／16 GB／170 GB |
| 300 人 | 4 vCPU／16 GB／160 GB | 8 vCPU／32 GB／260 GB |
| 1,000 人 | 8 vCPU／32 GB／340 GB | 8 vCPU／64 GB／570 GB |

磁碟含每日資料庫備份的輪替保留（最近 7 天、4 週、12 個月；壓縮保存，可另放在其他磁碟或網路儲存）。
對內只需開放 TCP 8788（HTTPS；Portal 不開啟未加密的 HTTP）。

Agent：Windows 10／11，最低 4 核／16 GB／20 GB，建議 8 核／32 GB／50 GB SSD；無須系統管理員權限。

## 線上更新

Portal 出廠預設為線上更新：每 6 小時向本 repo 的 Releases 查詢新版本（時間可於後台排程修改），須已安裝有效的授權檔。
**Portal 本身的下載與安裝仍由 IT 執行**，另可選擇開啟自動更新（預設關閉）。不允許對外連線的環境可將「更新來源」切換為「離線」，改由原廠提供安裝檔於後台上傳。
防火牆須放行 `api.github.com`、`github.com`、`release-assets.githubusercontent.com`。

安裝檔清單可註明更新前置條件（最低可升級的 Portal 版本、所需的授權）；條件未滿足時，更新頁會說明原因，不會安裝。

## 簽章清單的作用

METRIC Portal 以系統權限執行，而「套用更新」即執行上傳的安裝檔。
Portal 僅安裝經**原廠發行金鑰（Ed25519）簽署**的檔案：清單遭竄改、exe 遭替換或下載來源遭冒充，
均會在安裝前被阻擋。授權檔另由原廠授權金鑰簽署，兩把金鑰各司其職，用錯用途的簽章一律不接受。
私鑰僅存放於原廠，不在本 repo，亦不包含於任何出貨物。

因此 **每個檔案與其 `.manifest.json` 必須成對上架**。缺少清單時，客戶環境中的 Portal 將無法安裝該檔案。

## 價目表

`pricing/model-prices.json` 為主要模型的官方計費（每筆附官方來源與核對日期）。
Portal 更新模型規格表時（自動同步或「立即更新」）一併讀取此檔，作為建議單價的來源。

## IT 安裝說明

此處下載的 Agent 安裝檔**未內建入口位址**，安裝時會詢問要連線的伺服器。
企業內部已部署 METRIC Portal 時，自該 Portal 下載頁取得的安裝檔已內建位址，
無須手動輸入，請優先使用。

## 驗證下載的檔案

```
certutil -hashfile MetricPortalSetup-3.2.1.exe SHA256
```

計算結果應與同名 `.manifest.json` 中的 `sha256` 一致。
