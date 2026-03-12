# LinkedIn追加機能 実装パターン比較（A〜F）

## パターンA〜Fのアーキテクチャ図

---

## パターンA — BD / Dataset のみ

**一括購入・買い切り（差分更新なし）**

DatasetをCSVで一括購入してスプシに投入。更新は都度T3担当者が再購入して差し替えるシンプルな構成。

```mermaid
flowchart TD
    A[T3担当者] -->|Dataset一括購入| B[Bright Data\nDataset]
    B -->|CSV出力| C[Google Sheets]
    C -->|条件フィルタリング| D[T3担当者\nフィルタ作業]
    D -->|対象リスト| E[n8n / Claude]
    E -->|メッセージ生成| F[T3担当者\n最終承認]
    F -->|手動送信| G[LinkedIn DM]

    style B fill:#DBEAFE,stroke:#2563EB
    style E fill:#D1FAE5,stroke:#10B981
    style G fill:#0A66C2,color:#fff,stroke:#0A66C2
```

**費用感**
- 開発費：80〜120万円
- 月額ランニング：〜2万円（API費のみ）
- 初回Dataset購入：数十〜200万円（別途）

---

## パターンB — BD / Dataset + Scraper API

**初回購入 ＋ 差分更新あり（URL指定で最新情報取得）**

DatasetでURLリストを取得後、Scraper APIに対象URLを渡して最新プロフィール情報を随時取得。転職・情報更新を検知できる。

```mermaid
flowchart TD
    A[T3担当者] -->|初回Dataset購入\nURLリスト取得| B[Bright Data\nDataset]
    B -->|URLリスト| C[Bright Data\nScraper API]
    C -->|最新プロフィール取得\n¥0.23/件| D[Google Sheets]
    D -->|条件フィルタリング| E[T3担当者\nフィルタ作業]
    E -->|対象リスト| F[n8n / Claude]
    F -->|メッセージ生成| G[T3担当者\n最終承認]
    G -->|手動送信| H[LinkedIn DM]

    A -->|差分更新時\nURL再指定| C

    style B fill:#DBEAFE,stroke:#2563EB
    style C fill:#DBEAFE,stroke:#2563EB
    style F fill:#D1FAE5,stroke:#10B981
    style H fill:#0A66C2,color:#fff,stroke:#0A66C2
```

**費用感**
- 開発費：100〜150万円
- 月額ランニング：2〜4万円（Scraper API ¥0.23/件 + API費）
- 初回Dataset購入：別途

---

## パターンC — BD + AUTOHUNT 連携

**BDデータ × AUTOHUNTで転職潜在層にDM送信**

BDで取得したデータをAUTOHUNTにインポート。転職兆候スコアリングとLinkedIn一括DM送信機能を活用。

```mermaid
flowchart TD
    A[T3担当者] -->|Dataset購入| B[Bright Data\nDataset]
    B -->|データインポート| C[AUTOHUNT]
    A -->|フィルタ条件設定| C
    C -->|転職兆候スコアリング\n特許AI技術| D[スコアリング結果]
    D -->|高スコア候補者| E[n8n / Claude\n※オプション]
    E -->|カスタムメッセージ| F[AUTOHUNT\nDM自動送信]
    D -->|AHテンプレート| F
    F -->|LinkedIn API経由\n一括自動送信| G[LinkedIn DM]

    style B fill:#DBEAFE,stroke:#2563EB
    style C fill:#FEF3C7,stroke:#F59E0B
    style E fill:#D1FAE5,stroke:#10B981
    style G fill:#0A66C2,color:#fff,stroke:#0A66C2
```

**費用感**
- 開発費：50〜80万円
- 月額ランニング：AUTOHUNT月額 + API費（AH料金要確認）
- 初回Dataset購入：別途

---

## パターンD — BD / Dataset + Scraper API + フロント

**差分更新あり ＋ 専用UI（マルチテナント対応）**

BパターンにWebフロント画面を追加。候補者一覧・フィルタ・URLワンクリック補助を実装。複数クライアント展開向け。

```mermaid
flowchart TD
    A[T3担当者] -->|初回Dataset購入\nURLリスト取得| B[Bright Data\nDataset]
    B -->|URLリスト| C[Bright Data\nScraper API]
    C -->|最新プロフィール取得| D[(Database\nマルチテナント対応)]
    D -->|データ表示| E[Web UI\nReact / Vue]
    E -->|フィルタ・承認操作| F[T3担当者\nUI上で作業]
    F -->|URLクリック補助| G[LinkedIn\nプロフィール表示]
    F -->|対象リスト| H[n8n / Claude]
    H -->|メッセージ生成| F
    G -->|手動DM送信| I[LinkedIn DM]

    A -->|差分更新時\nURL再指定| C

    style B fill:#DBEAFE,stroke:#2563EB
    style C fill:#DBEAFE,stroke:#2563EB
    style D fill:#EDE9FE,stroke:#7C3AED
    style E fill:#EDE9FE,stroke:#7C3AED
    style H fill:#D1FAE5,stroke:#10B981
    style I fill:#0A66C2,color:#fff,stroke:#0A66C2
```

**費用感**
- 開発費：200〜280万円（パターンB + UI開発 80〜130万円）
- 月額ランニング：2〜4万円（Scraper API + API費）
- 初回Dataset購入：別途
- 移行推奨：マルチテナント展開確定後にB→D

---

## パターンE — Phantombuster / スクレイピングのみ

**PBでデータ収集・メッセージ生成（DM送信は人力）**

PB LinkedIn Profile ScraperでデータをCSV取得後、n8nでメッセージ生成。初期Dataset購入費なしで始められる。

```mermaid
flowchart TD
    A[T3担当者] -->|検索条件設定| B[Phantombuster]
    B -->|LinkedIn自動スクレイピング\n$69〜$199/月| C[LinkedIn\nプロフィールデータ]
    C -->|CSV出力| D[Google Sheets]
    D -->|条件フィルタリング| E[T3担当者\nフィルタ作業]
    E -->|対象リスト| F[n8n / Claude]
    F -->|メッセージ生成| G[T3担当者\n最終承認]
    G -->|手動送信| H[LinkedIn DM]

    style B fill:#FEE2E2,stroke:#EF4444
    style F fill:#D1FAE5,stroke:#10B981
    style H fill:#0A66C2,color:#fff,stroke:#0A66C2
```

> ⚠️ **リスク注意**：LinkedInのToSグレーゾーン。レート制限遵守により実運用上のBANリスクは管理可能。

**費用感**
- 開発費：80〜120万円
- 月額ランニング：2〜5万円（PB $69〜$199/月 + API費）
- 初期Dataset購入：不要（コスト優位）

---

## パターンF — Phantombuster / DM自動送信（PoC）

**PBでDM自動送信まで試験的に実施（BAN・ToSリスクあり）**

Phantombuster Message Senderで自動DM送信を試験的に実施。LinkedInアカウントへの影響が大きく、**商用本番での継続利用は非推奨**。

```mermaid
flowchart TD
    A[T3担当者] -->|条件設定| B[Phantombuster]
    B -->|LinkedIn自動スクレイピング| C[LinkedIn\nプロフィールデータ]
    C -->|データ取得| D[Google Sheets]
    D -->|条件フィルタリング| E[T3担当者\nフィルタ作業]
    E -->|対象リスト| F[n8n / Claude]
    F -->|メッセージ生成| G[T3担当者\n最終承認]
    G -->|自動送信リスト渡し| H[Phantombuster\nMessage Sender]
    H -->|LinkedIn自動DM一括送信\n⚠️ BAN高リスク| I[LinkedIn DM]

    style B fill:#FEE2E2,stroke:#EF4444
    style H fill:#FEE2E2,stroke:#EF4444
    style F fill:#D1FAE5,stroke:#10B981
    style I fill:#0A66C2,color:#fff,stroke:#0A66C2
```

> 🚨 **HIGH RISK**：LinkedIn ToS違反。週100件超でアカウント停止リスク大。T3メインアカウントでの本番利用は不可。PoC・検証用途限定。

**費用感**
- 開発費：130〜180万円
- 月額ランニング：5〜10万円
- 初期Dataset購入：不要

---

## パターン比較サマリー

| | A | B | C | D | E | F |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| データソース | BD Dataset | BD Dataset + Scraper | BD + AUTOHUNT | BD Dataset + Scraper | Phantombuster | Phantombuster |
| 差分更新 | ✗ | ✓ | ✓ | ✓ | ✓ | ✓ |
| DM送信 | 手動 | 手動 | 自動（AH） | 手動 | 手動 | 自動（PB）⚠️ |
| 専用UI | ✗ | ✗ | AH UI | ✓ Web UI | ✗ | ✗ |
| 初期Dataset費 | 要 | 要 | 要 | 要 | 不要 | 不要 |
| 開発費 | 80〜120万 | 100〜150万 | 50〜80万 | 200〜280万 | 80〜120万 | 130〜180万 |
| 月額ランニング | 〜2万 | 2〜4万 | AH月額+ | 2〜4万 | 2〜5万 | 5〜10万 |
| リスク | 低 | 低 | 低〜中 | 低 | 中 | 高 |
