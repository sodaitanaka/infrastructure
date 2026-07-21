# HubSpotワークフロー全件一覧（エクスポート生データ）

> **出典・信頼度に関する注記**
> `hubspot-listing-lib-exports--2026-07-22.xlsx`（MARS側からの提供、2026年7月22日）をそのまま転記したもの。
> HubSpotの「ワークフロー」一覧画面からエクスポートした**メタデータのみ**（名前・ON/OFF・対象オブジェクト・トリガー種別・登録数・更新者/日）であり、**トリガー条件やアクションの中身（分岐条件・プロパティ更新内容）は含まれていない**。
> `report-coupon-code-redesign.html` Section 07 で指摘した「発行・有効化・チェーン名一括修正など、実際のワークフローの実物を確認できていない」という問題に対し、まず**候補となるワークフロー名とIDを特定する**ための一次資料として位置づける。個別の詳細（アクション中身）は、この一覧から絞り込んだ上で追って画面キャプチャ／手動転記が必要。

全132件中。

---

## クーポン／店舗／チェーン関連ワークフロー抜粋（45件）

名前に「クーポン／Coupon／Chain／Store／Redemption」を含むものを抽出。**Section 07の未確認事項に対する最有力候補**を先頭にまとめる。

### 最優先で詳細確認したい候補

| Record ID | Name | ON/OFF | Object | 推定される役割・関連する未確認事項 |
| --- | --- | --- | --- | --- |
| 382790401463 | **[SYS] Set Deal's Redemption Chain Name** | true | DEAL | `redemption_chain_name`（および恐らく`redemption_store_code`等の兄弟プロパティ）を書き込む本命候補。Section 07最重要の未確認事項（利用実績画面・CSVレポートが参照する`redemption_*`系プロパティの書き込み元）に直結。**発行時店舗のコピーか、実際の使用店舗の反映かをこのWFの中身で確認する必要がある。** |
| 337185620512 | **[SYS] Set Coupon Code** | true | DEAL | `oneofcoupon.md`記載の発行WF内「アクション③：クーポンコード生成」に相当する本体と推定。Section 02の組成ロジック（店舗種別+取引ID断片+クーポンID+PetID断片）が実際にこの中で行われているかを確認したい最優先ワークフロー。 |
| 423636589186 | [SYS] Update Issuing Store Code | **false（無効）** | DEAL | 名称上は店舗変更（issuing_store_code更新）を目的とした既存ワークフローだが、現在**無効化**されている。Section 04の「①店舗変更」で提案する内容と役割が重複する可能性があり、なぜ無効化されたか確認したい。 |
| 505039467202 | [TEMP] 閉店済み店舗のStore Coupon Type・Store Coupon Informationを更新 | true | COMPANY | Section 01で会議に挙がった「倒産・閉店店舗の救済（クーポンタイプ変更）」と**同じ目的の運用が既に`[TEMP]`（一時対応）ワークフローとして存在**している。恒久化されていない理由、対象がCOMPANY単位である点（Deal個別ではなく店舗の親会社単位）を確認したい。Step 1の提案はこれを一般化・恒久化する形に近い。 |

### その他のクーポン関連（発行・配信・有効期限・状態管理）

| Record ID | Name | ON/OFF | Object | Trigger |
| --- | --- | --- | --- | --- |
| 351072933283 | [SYS] Send Instant Coupon | true | DEAL | Filter criteria | ※詳細は`workflow-sys-send-instant-coupon.md`で確認済み
| 352634094763 | [TEST] Send Scheduled Coupon Instantly for Testing | false | DEAL | Filter criteria |
| 350814347317 | [SYS] Send Scheduled Coupon | true | DEAL | Filter criteria |
| 589466226411 | [SYS] フローズン新発売クーポン_クーポンコード送信 | true | DEAL | Events |
| 595392460499 | OLD_[SYS] フローズン新発売クーポン_クーポンコード送信_改修1031 | false | DEAL | Events |
| 589480684266 | [SYS] フローズン新発売クーポン_251104 | true | pets | -- |
| 485464248039 | [SYS] リニューアル記念クーポン_クーポンコード送信 | true | DEAL | Events |
| 475584350932 | [SYS] リニューアル記念クーポン | true | pets | -- |
| 783992018654 | [SYS] 10月猫パウチクーポン_クーポンコード送信 | false | DEAL | Events |
| 784012895941 | [SYS] 10月猫パウチクーポン_2610 | false | pets | -- |
| 564660816571 | [SYS]FLOW-SPT-001(即時クーポン)（ブリーダー） | true | pets | -- |
| 384207995152 | [SYS]FLOW-SPT-001(即時クーポン) | true | pets | -- |
| 564893065914 | [SYS] Welcome Journey 2（即時クーポン）（ブリーダー） | true | pets | -- |
| 808897171178 | ブリーダーのクーポン印字の修正_取引 | true | DEAL | Filter criteria |
| 808911188714 | ブリーダーのクーポン印字の修正_会社 | true | COMPANY | Filter criteria |
| 411720150284 | [MGRTN] Configure expired coupons | true | DEAL | Filter criteria |
| 409611057186 | [MGRTN] Set Deal Stage for Active/Inactive/Expired Coupons | true | DEAL | Filter criteria |
| 361427659929 | [SYS] Set Expired Coupons | true | DEAL | Filter criteria |
| 410293936220 | [SYS] Configure Store Coupon Type | true | DEAL | Filter criteria |
| 410279565379 | [MGRTN] Configure Coupon ID 12 | true | DEAL | Filter criteria |
| 361786236068 | [SYS] Set Coupon's Breeder Flag | true | DEAL | Filter criteria |
| 350807335093 | [SYS] Generate Coupon Asset Code | true | coupon_assets | Filter criteria |
| 350792173208 | [MGRTN] Set Coupon Asset Code | true | DEAL | Filter criteria |
| 354807714085 | [MGRTN] Set Coupon Asset Code's Coupon Product Information | true | coupon_assets | Filter criteria |
| 350141123996 | [SYS] Associate Coupon Asset to Deal | true | DEAL | Filter criteria |
| 409271365141 | [TEMP] Associate Coupon Asset to Deal | false | DEAL | -- |
| 409286772874 | [SYS] Remove Coupon Asset Association | true | DEAL | Filter criteria |
| 409256715925 | [SYS] Remove Coupon Asset Association v0 | false | DEAL | -- |
| 759586911956 | デジタルクーポンキャンペーン202604 | true | DEAL | Filter criteria |
| 760191175411 | デジタルクーポンキャンペーン202604_メルマガ送信 | false | CONTACT | Filter criteria |
| 760411707083 | テスト用_デジタルクーポンキャンペーン202604 | true | DEAL | Filter criteria |
| 761475560164 | テスト用_デジタルクーポンキャンペーン202604_メルマガ送信 | true | CONTACT | Filter criteria |
| 400303239424 | 【TEST】D2Cクーポン検証 | true | CONTACT | Filter criteria |

### 店舗・チェーン（マスタ側）関連

| Record ID | Name | ON/OFF | Object | Trigger | 備考 |
| --- | --- | --- | --- | --- | --- |
| 340907260041 | [SYS] Set Store's Chain Code | true | COMPANY | Filter criteria | 店舗（Company）自身のchain_codeを設定。Deal側の`issuing_chain_code`とは別軸 — Section 04の「Company側の値が変わっても既発行Dealには再同期されない」という分析と整合 |
| 340904087706 | [MGRTN] Set Chain Records Chain Code | true | COMPANY | Filter criteria | 上記の一括移行版とみられる |
| 340909779786 | [MGRTN] Set Issuing Store Code When Issuing Store is Associated | true | DEAL | Filter criteria | Deal作成時にissuing_store_codeを設定する初期化用とみられる（事後変更用ではない） |
| 340907239458 | [SYS]  Associate Store Issued At When Issuing Store Code is Known | true | DEAL | Filter criteria | 「Store Issued At」というタイムスタンプ系プロパティの設定とみられ、redemption系とは別物の可能性 |
| 384092377222 | [SYS] Set Pet's Registered at Store Chain Name | true | pets | Filter criteria | Pet側のプロパティ。Deal側のredemption_chain_nameとは別 |
| 516550552260 | [SYS] Set Latest Pet Registration Date and Latest Store Code | true | pets | Filter criteria | |

---

## 全件一覧（132件・生データ）

| Record ID | Name | On/Off | Object type | Trigger Type | Enrolled total | Updated on | Updated by | Created on | Created by |
|---|---|---|---|---|---|---|---|---|---|
| 808897171178 | ブリーダーのクーポン印字の修正_取引 | true | DEAL | Filter criteria | 57710.0 | 2026-07-17 17:06 | 聡大 田中 | 2026-07-08 11:09 | 聡大 田中 |
| 808911188714 | ブリーダーのクーポン印字の修正_会社 | true | COMPANY | Filter criteria | 1553.0 | 2026-07-17 17:05 | 聡大 田中 | 2026-07-08 11:07 | 聡大 田中 |
| 760191175411 | デジタルクーポンキャンペーン202604_メルマガ送信 | false | CONTACT | Filter criteria | 4766.0 | 2026-06-23 20:05 | 聡大 田中 | 2026-04-10 13:58 | 聡大 田中 |
| 795441285842 | welcomejourney6 割引金額挿入 | true | DEAL | Filter criteria | 12562.0 | 2026-06-22 17:28 | 聡大 田中 | 2026-06-17 16:27 | 聡大 田中 |
| 788792956653 | D2C_専売品Upsell提案 Shopify | true | ORDER | Events | 176.0 | 2026-06-18 15:14 | Hiroyu Kato | 2026-06-04 10:14 | Hiroyu Kato |
| 546856069875 | D2C_商品再購入促進_定期便提案 Shopify | true | ORDER | Events | 3905.0 | 2026-06-18 15:13 | Hiroyu Kato | 2025-09-22 17:30 | Hiroyu Kato |
| 529502639864 | D2C_Pucchi_ThanksMail_5DAfter Shopify | true | ORDER | Filter criteria | 733.0 | 2026-06-18 15:11 | Hiroyu Kato | 2025-09-02 20:24 | Hiroyu Kato |
| 529450711801 | D2C_Pucchi_ThanksMail_3DAfter Shopify | true | ORDER | Filter criteria | 733.0 | 2026-06-18 15:09 | Hiroyu Kato | 2025-09-02 20:23 | Hiroyu Kato |
| 529435067074 | D2C_Pucchi_ThanksMail_1DAfter Shopify | true | ORDER | Filter criteria | 733.0 | 2026-06-18 15:08 | Hiroyu Kato | 2025-09-02 20:00 | Hiroyu Kato |
| 553863954108 | D2C_お試しサンプルSTEPメール（犬）Shopify | true | ORDER | Events | 3119.0 | 2026-06-18 15:06 | Hiroyu Kato | 2025-09-26 13:49 | Hiroyu Kato |
| 551016153827 | D2C_お試しサンプルSTEPメール（猫）Shopify | true | ORDER | Events | 3121.0 | 2026-06-18 15:04 | Hiroyu Kato | 2025-09-25 16:06 | Hiroyu Kato |
| 546763247325 | D2C_グリ子犬→成犬切り替え Shopify | true | ORDER | Filter criteria | 52.0 | 2026-06-18 15:03 | Hiroyu Kato | 2025-09-22 18:04 | Hiroyu Kato |
| 546781686502 | D2C_グリニーズ子犬おすすめ Shopify | true | CONTACT | Filter criteria |  | 2026-06-18 15:01 | Hiroyu Kato | 2025-09-22 17:57 | Hiroyu Kato |
| 546761219792 | D2C_未購入お試しサンプルご案内 Shopify | true | CONTACT | Filter criteria | 2708.0 | 2026-06-18 14:58 | Hiroyu Kato | 2025-09-22 17:10 | Hiroyu Kato |
| 411993593463 | [SYS] Welcome Journey 6（生後12か月） | true | pets |  -- | 62588.0 | 2026-06-17 16:35 | 聡大 田中 | 2025-03-14 14:20 | 木村 泰之 |
| 564887804645 | [SYS] Welcome Journey 6（生後12か月）（ブリーダー） | true | pets |  -- | 8856.0 | 2026-06-17 16:34 | 聡大 田中 | 2025-10-07 09:15 | Naoya Kashida |
| 783992018654 | [SYS] 10月猫パウチクーポン_クーポンコード送信 | false | DEAL | Events |  | 2026-06-10 12:16 | 聡大 田中 | 2026-05-26 19:40 | 聡大 田中 |
| 784012895941 | [SYS] 10月猫パウチクーポン_2610 | false | pets |  -- |  | 2026-05-26 19:40 | 聡大 田中 | 2026-05-26 19:40 | 聡大 田中 |
| 759586911956 | デジタルクーポンキャンペーン202604 | true | DEAL | Filter criteria | 27503.0 | 2026-04-13 18:14 | 聡大 田中 | 2026-04-09 11:16 | 聡大 田中 |
| 761475560164 | テスト用_デジタルクーポンキャンペーン202604_メルマガ送信 | true | CONTACT | Filter criteria | 1.0 | 2026-04-13 18:07 | 聡大 田中 | 2026-04-13 17:39 | 聡大 田中 |
| 760411707083 | テスト用_デジタルクーポンキャンペーン202604 | true | DEAL | Filter criteria | 5436.0 | 2026-04-13 18:02 | 聡大 田中 | 2026-04-10 19:15 | 聡大 田中 |
| 708195138268 | [SYS] SHOPIFY IMPORT APP - マーケティングコンタクト登録 | true | CONTACT | Filter criteria | 1732.0 | 2026-02-06 16:19 | minagawa yuki | 2026-02-06 16:08 | minagawa yuki |
| 529374493384 | [SYS] SHOPIFY IMPORT APP | true | CONTACT | Filter criteria | 141957.0 | 2026-02-06 16:07 | minagawa yuki | 2025-09-02 18:28 | minagawa yuki |
| 352634094763 | [TEST] Send Scheduled Coupon Instantly for Testing | false | DEAL | Filter criteria | 293318.0 | 2025-11-03 12:01 | 聡大 田中 | 2024-08-04 17:51 | Jase Bajade |
| 351072933283 | [SYS] Send Instant Coupon | true | DEAL | Filter criteria | 320474.0 | 2025-11-03 11:37 | Naoya Kashida | 2024-07-30 12:16 | Jase Bajade |
| 595392460499 | OLD_[SYS] フローズン新発売クーポン_クーポンコード送信_改修1031 | false | DEAL | Events | 2.0 | 2025-11-03 10:33 | 聡大 田中 | 2025-10-31 19:06 | 聡大 田中 |
| 589466226411 | [SYS] フローズン新発売クーポン_クーポンコード送信 | true | DEAL | Events | 61648.0 | 2025-11-03 10:19 | Naoya Kashida | 2025-10-27 16:51 | Naoya Kashida |
| 591864972004 | アマゾン保護プログラム｜保護ペット作成後（複製） | true | pets | Events | 3.0 | 2025-10-31 09:41 | 悠斗 中野 | 2025-10-29 13:30 | 悠斗 中野 |
| 583123838656 | アマゾン保護プログラム｜保護ペット作成後 | false | pets | Events | 12.0 | 2025-10-29 13:56 | 悠斗 中野 | 2025-10-22 17:42 | 悠斗 中野 |
| 583111669492 | アマゾン保護プログラム｜迎主フォーム送信後 | true | CONTACT | Events | 20.0 | 2025-10-29 11:53 | 悠斗 中野 | 2025-10-22 17:37 | 悠斗 中野 |
| 583006901950 | アマゾン保護プログラム｜保護団体スタッフの2人目以降登録 | true | CONTACT | Events | 13.0 | 2025-10-28 11:08 | 悠斗 中野 | 2025-10-22 13:51 | 悠斗 中野 |
| 541569500909 | アマゾン保護プログラム｜会社(保護団体)登録後 | true | COMPANY | Events | 13.0 | 2025-10-28 11:01 | 悠斗 中野 | 2025-09-17 18:54 | 悠斗 中野 |
| 589480684266 | [SYS] フローズン新発売クーポン_251104 | true | pets |  -- | 61669.0 | 2025-10-27 17:37 | Naoya Kashida | 2025-10-27 16:44 | Naoya Kashida |
| 583128567493 | アマゾン保護プログラム｜迎え主お問い合わせフォーム送信後 | true | CONTACT | Events | 6.0 | 2025-10-23 15:39 | 悠斗 中野 | 2025-10-22 18:27 | 悠斗 中野 |
| 583850082025 | 動作テスト_毎日12時にクリア：アマゾン保護プログラム｜「今月作成されたペット」をクリア（複製） | true | pets | Filter criteria | 21.0 | 2025-10-23 09:42 | 悠斗 中野 | 2025-10-23 09:41 | 悠斗 中野 |
| 583850060475 | 動作テスト_毎日12時にリセット：アマゾン保護プログラム｜「今年作成されたペット」をクリア | true | pets | Filter criteria | 21.0 | 2025-10-23 09:40 | 悠斗 中野 | 2025-10-23 09:38 | 悠斗 中野 |
| 583850470080 | 動作テスト用_毎日10時に送信されます：アマゾン保護プログラム｜月一の頭数報告メールの送信 | true | COMPANY | Filter criteria | 1606.0 | 2025-10-23 09:33 | 悠斗 中野 | 2025-10-23 09:32 | 悠斗 中野 |
| 583845019369 | 動作テスト用：アマゾン保護プログラム｜迎え主用定期便誘引メールの送信 | true | pets | Filter criteria | 12.0 | 2025-10-23 09:27 | 悠斗 中野 | 2025-10-23 09:26 | 悠斗 中野 |
| 583142978271 | アマゾン保護プログラム｜迎え主用定期便誘引メールの送信 | true | pets | Filter criteria | 12.0 | 2025-10-22 18:54 | 悠斗 中野 | 2025-10-22 18:32 | 悠斗 中野 |
| 583210917622 | アマゾン保護プログラム｜「今年作成されたペット」をクリア | true | pets | Filter criteria |  | 2025-10-22 18:48 | 悠斗 中野 | 2025-10-22 18:46 | 悠斗 中野 |
| 583210899167 | アマゾン保護プログラム｜「今月作成されたペット」をクリア | true | pets | Filter criteria |  | 2025-10-22 18:45 | 悠斗 中野 | 2025-10-22 18:44 | 悠斗 中野 |
| 583210862323 | アマゾン保護プログラム｜月一の頭数報告メールの送信 | true | COMPANY | Filter criteria | 54.0 | 2025-10-22 18:41 | 悠斗 中野 | 2025-10-22 18:36 | 悠斗 中野 |
| 564660816571 | [SYS]FLOW-SPT-001(即時クーポン)（ブリーダー） | true | pets |  -- | 8856.0 | 2025-10-17 18:39 | Naoya Kashida | 2025-10-07 09:09 | Naoya Kashida |
| 564893065914 | [SYS] Welcome Journey 2（即時クーポン）（ブリーダー） | true | pets |  -- | 8856.0 | 2025-10-09 11:01 | Naoya Kashida | 2025-10-07 09:11 | Naoya Kashida |
| 338131391153 | [SYS] When Pet is Registered Trigger Welcome Journeys | true | pets | Filter criteria | 74156.0 | 2025-10-07 09:22 | Naoya Kashida | 2024-06-14 11:21 | Jase Bajade |
| 564649875178 | [SYS] Welcome Journey 5（生後8か月）（ブリーダー） | true | pets |  -- | 8856.0 | 2025-10-07 09:20 | Naoya Kashida | 2025-10-07 09:14 | Naoya Kashida |
| 564786100947 | [SYS] Welcome Journey 4（生後6か月）（ブリーダー） | true | pets |  -- | 8856.0 | 2025-10-07 09:14 | Naoya Kashida | 2025-10-07 09:13 | Naoya Kashida |
| 564649856720 | [SYS] Welcome Journey 3(登録後14日後)（ブリーダー） | true | pets |  -- | 8856.0 | 2025-10-07 09:13 | Naoya Kashida | 2025-10-07 09:12 | Naoya Kashida |
| 564826306284 | [SYS] Welcome Journey 1(登録後14日後)（ブリーダー） | true | pets |  -- | 8856.0 | 2025-10-07 09:11 | Naoya Kashida | 2025-10-07 09:10 | Naoya Kashida |
| 391174932788 | D2C_お試しサンプルSTEPメール（猫） | false | DEAL | Events | 3543.0 | 2025-09-24 17:12 | Hiroyu Kato | 2024-12-17 10:43 | Nagisa Ueyama |
| 418041728546 | D2C_グリ子犬→成犬切り替え | false | DEAL | Filter criteria | 38.0 | 2025-09-24 17:12 | Hiroyu Kato | 2025-04-02 10:28 | Nagisa Ueyama |
| 398403745936 | D2C_未購入お試しサンプルご案内 | false | CONTACT | Filter criteria | 1973.0 | 2025-09-24 17:12 | Hiroyu Kato | 2025-01-20 14:59 | Nagisa Ueyama |
| 416436324657 | D2C_グリニーズ子犬おすすめ | false | CONTACT | Filter criteria | 490.0 | 2025-09-24 17:12 | Hiroyu Kato | 2025-03-28 14:39 | Nagisa Ueyama |
| 385167475472 | D2C_お試しサンプルSTEPメール（犬） | false | DEAL | Events | 3630.0 | 2025-09-24 17:12 | Hiroyu Kato | 2024-11-22 15:21 | Nagisa Ueyama |
| 384456038224 | D2C_再購入促進 | false | DEAL | Events | 2124.0 | 2025-09-24 17:11 | Hiroyu Kato | 2024-11-20 13:06 | Nagisa Ueyama |
| 529492215545 | [SYS]SHOPIF- TAGからメンバーランクを更新 | true | CONTACT | Events | 74090.0 | 2025-09-02 20:26 | minagawa yuki | 2025-09-02 19:57 | minagawa yuki |
| 516550552260 | [SYS] Set Latest Pet Registration Date and Latest Store Code | true | pets | Filter criteria | 56602.0 | 2025-08-19 11:26 | minagawa yuki | 2025-08-18 14:11 | minagawa yuki |
| 475584350932 | [SYS] リニューアル記念クーポン | true | pets |  -- | 278135.0 | 2025-08-05 17:52 | Naoya Kashida | 2025-06-26 18:30 | Naoya Kashida |
| 490021634761 | 【PKP】マーケティング対象外への自動割り振りフロー | true | CONTACT | Filter criteria | 72479.0 | 2025-08-04 15:22 | 木村 泰之 | 2025-07-15 16:31 | 木村 泰之 |
| 485464248039 | [SYS] リニューアル記念クーポン_クーポンコード送信 | true | DEAL | Events | 277866.0 | 2025-07-31 16:30 | Naoya Kashida | 2025-07-10 10:16 | Naoya Kashida |
| 505039467202 | [TEMP] 閉店済み店舗のStore Coupon Type・Store Coupon Informationを更新 | true | COMPANY |  -- | 417.0 | 2025-07-31 12:25 | Naoya Kashida | 2025-07-31 11:35 | Naoya Kashida |
| 350814347317 | [SYS] Send Scheduled Coupon | true | DEAL | Filter criteria | 263645.0 | 2025-06-27 14:33 | Naoya Kashida | 2024-07-29 17:45 | Jase Bajade |
| 436837582491 | 名前のないワークフロー - 2025-06-13 05:20:28 GMT+0000 | false | DEAL |  -- |  | 2025-06-27 13:03 |  | 2025-06-13 14:20 | Hiroyu Kato |
| 423636589186 | [SYS] Update Issuing Store Code | false | DEAL |  -- |  | 2025-06-27 13:03 |  | 2025-04-21 14:37 | Jase Bajade |
| 423629902734 | 名前のないワークフロー - 2025-04-21 05:37:31 GMT+0000 | false | CONTACT |  -- |  | 2025-06-27 13:03 |  | 2025-04-21 14:37 | Jase Bajade |
| 386445766415 | [SYS]ぺとらば - お問い合わせ登録後フロー | true | form_attributes | Events | 466.0 | 2025-04-22 07:05 | minagawa yuki | 2024-11-27 17:01 | minagawa yuki |
| 380243800837 | [SYS] Send PKP Registration Email | true | CONTACT | Filter criteria | 63319.0 | 2025-04-22 06:56 | minagawa yuki | 2024-11-06 15:38 | Jase Bajade |
| 411720150284 | [MGRTN] Configure expired coupons | true | DEAL | Filter criteria | 82427.0 | 2025-04-21 08:57 | Jase Bajade | 2025-03-13 19:58 | Jase Bajade |
| 409611057186 | [MGRTN] Set Deal Stage for Active/Inactive/Expired Coupons | true | DEAL | Filter criteria | 2028379.0 | 2025-04-21 08:56 | Jase Bajade | 2025-03-05 18:46 | Jase Bajade |
| 419712461830 | [SYS]2025インターペット フォーム登録後アフターフォロー（4/4登録者） | true | form_attributes | Filter criteria | 1839.0 | 2025-04-20 20:06 |  | 2025-04-07 09:32 | Nagisa Ueyama |
| 414121239723 | [MGRTN] Set Member Number (Legacy) | true | CONTACT | Filter criteria | 67880.0 | 2025-04-20 20:06 |  | 2025-03-21 15:28 | Jase Bajade |
| 413200116000 | [SYS]2025グロッサリー展示会・イベントフォーム登録後フロー | true | form_attributes | Filter criteria | 60.0 | 2025-04-20 20:06 |  | 2025-03-19 15:04 | Nagisa Ueyama |
| 411112152890 | Test ー - 2025-03-11 05:00:30 GMT+0000 | false | pets | Events |  | 2025-04-20 20:06 |  | 2025-03-11 14:00 | Randika Srimal |
| 410293936220 | [SYS] Configure Store Coupon Type | true | DEAL | Filter criteria | 4882982.0 | 2025-04-20 20:06 |  | 2025-03-07 15:58 | Jase Bajade |
| 410279565379 | [MGRTN] Configure Coupon ID 12 | true | DEAL | Filter criteria | 160347.0 | 2025-04-20 20:06 |  | 2025-03-07 15:50 | Jase Bajade |
| 410278069840 | [SYS]2025インターペット フォーム登録後アフターフォロー | true | form_attributes | Filter criteria | 4379.0 | 2025-04-20 20:06 |  | 2025-03-07 14:44 | Nagisa Ueyama |
| 409286772874 | [SYS] Remove Coupon Asset Association | true | DEAL | Filter criteria | 3344273.0 | 2025-04-20 20:06 |  | 2025-03-04 12:07 | Jase Bajade |
| 409271365141 | [TEMP] Associate Coupon Asset to Deal  | false | DEAL |  -- | 256898.0 | 2025-04-20 20:06 |  | 2025-03-04 10:44 | Jase Bajade |
| 409256715925 | [SYS] Remove Coupon Asset Association v0 | false | DEAL |  -- | 3717.0 | 2025-04-20 20:06 |  | 2025-03-04 09:29 | Jase Bajade |
| 409106516091 | 名前のないワークフロー - 2025-03-03 13:33:59 GMT+0000 | false | DEAL |  -- |  | 2025-04-20 20:06 |  | 2025-03-03 22:33 | Jase Bajade |
| 402749137721 | [SYS]ニュートロ公式通販メルマガ配信オプトイン | false | CONTACT | Filter criteria | 5.0 | 2025-04-20 20:06 |  | 2025-02-06 17:39 | minagawa yuki |
| 402744290215 | [SYS]ニュートロ公式通販メルマガ配信オプトアウト | false | CONTACT | Filter criteria | 2.0 | 2025-04-20 20:06 |  | 2025-02-06 17:25 | minagawa yuki |
| 402374162466 | [SYS]2025インターペット・イベントフォーム登録後フロー | true | form_attributes | Filter criteria | 6935.0 | 2025-04-20 20:06 |  | 2025-02-05 10:32 | minagawa yuki |
| 402159543888 | 名前のないワークフロー - 2025-02-04 09:23:16 GMT+0000 | false | CONTACT | Events |  | 2025-04-20 20:06 |  | 2025-02-04 18:23 | minagawa yuki |
| 401826228030 | [TEST]Workflow登録後 - インターペット | false | FEEDBACK_SUBMISSION |  -- |  | 2025-04-20 20:06 |  | 2025-02-03 14:03 | minagawa yuki |
| 400303239424 | 【TEST】D2Cクーポン検証 | true | CONTACT | Filter criteria | 74.0 | 2025-04-20 20:06 |  | 2025-01-28 11:30 | Nagisa Ueyama |
| 399437200567 | Scenario 1 | true | pets |  -- |  | 2025-04-20 20:06 |  | 2025-01-24 14:08 | Jase Bajade |
| 399458189379 | Scenario 1 - Contact Based | true | CONTACT |  -- | 6.0 | 2025-04-20 20:06 |  | 2025-01-24 14:14 | Jase Bajade |
| 397650161296 | 名前のないワークフロー - 2025-01-16 05:21:44 GMT+0000 | false | CONTACT | Filter criteria |  | 2025-04-20 20:06 |  | 2025-01-16 14:21 | minagawa yuki |
| 357170796675 | [SYS] When PKP Pet is Registered Configure Registration Email | true | pets | Filter criteria | 72142.0 | 2025-03-28 10:29 | minagawa yuki | 2024-08-21 14:03 | Jase Bajade |
| 365208515502 | [MGRTN] Compute Activation Date | true | DEAL | Filter criteria | 7453271.0 | 2025-03-17 16:01 | Jase Bajade | 2024-09-17 12:14 | Jase Bajade |
| 378898440022 | [SYS] Welcome Journey 4（生後6か月） | true | pets |  -- | 62688.0 | 2025-03-14 16:56 | 木村 泰之 | 2024-10-31 14:01 | Varun Mahadevan |
| 350141123996 | [SYS] Associate Coupon Asset to Deal | true | DEAL | Filter criteria | 4116999.0 | 2025-03-09 16:21 | Jase Bajade | 2024-07-26 18:54 | Jase Bajade |
| 350792173208 | [MGRTN] Set Coupon Asset Code | true | DEAL | Filter criteria | 4311559.0 | 2025-03-07 15:22 | Jase Bajade | 2024-07-29 17:12 | Jase Bajade |
| 340907239458 | [SYS]  Associate Store Issued At When Issuing Store Code is Known | true | DEAL | Filter criteria | 1628302.0 | 2025-03-04 15:03 | Jase Bajade | 2024-06-24 17:37 | Jase Bajade |
| 378870989925 | [SYS] Welcome Journey 5（生後8か月） | true | pets |  -- | 62688.0 | 2025-03-04 14:38 | 木村 泰之 | 2024-10-31 14:03 | Varun Mahadevan |
| 378897088691 | [SYS] Welcome Journey 2（登録後14日後） | true | pets |  -- | 62688.0 | 2025-03-04 13:46 | 木村 泰之 | 2024-10-31 13:55 | Varun Mahadevan |
| 378870572384 | [SYS] Welcome Journey 3(登録後14日後) | true | pets |  -- | 62688.0 | 2025-03-04 13:42 | 木村 泰之 | 2024-10-31 13:58 | Varun Mahadevan |
| 337184685920 | [SYS] Welcome Journey 1(登録後14日後) | true | pets |  -- | 65059.0 | 2025-03-04 13:38 | minagawa yuki | 2024-06-11 16:20 | Jase Bajade |
| 384207995152 | [SYS]FLOW-SPT-001(即時クーポン) | true | pets |  -- | 62648.0 | 2025-03-04 13:33 | minagawa yuki | 2024-11-19 21:50 | minagawa yuki |
| 337185620512 | [SYS] Set Coupon Code | true | DEAL | Filter criteria | 549520.0 | 2025-01-24 15:26 | Jase Bajade | 2024-06-11 16:30 | Jase Bajade |
| 389525281303 | [SYS]TEST-FLOW-SPT-001 | true | pets |  -- | 199.0 | 2025-01-14 10:01 |  | 2024-12-11 10:14 | minagawa yuki |
| 385114379403 | [TEST] Valentine Day Blast | true | CONTACT |  -- | 2.0 | 2025-01-14 10:01 |  | 2024-11-22 12:19 | Jase Bajade |
| 384131610494 | [SYS] Copy Member ID of the Owner to the Pet Record | true | pets | Filter criteria | 1045322.0 | 2025-01-14 10:01 |  | 2024-11-19 15:16 | Lahiru Pramod |
| 384139138699 | [SYS] Send Line Message | true | DEAL |  -- | 71965.0 | 2025-01-14 10:01 |  | 2024-11-19 16:08 | Jase Bajade |
| 384092377222 | [SYS] Set Pet's Registered at Store Chain Name | true | pets | Filter criteria | 886360.0 | 2025-01-14 10:01 |  | 2024-11-19 13:30 | Lahiru Pramod |
| 384093691455 | 名前のないワークフロー - 2024-11-19 03:51:22 GMT+0000 | false | pets | Events |  | 2025-01-14 10:01 |  | 2024-11-19 12:51 | minagawa yuki |
| 383687906829 | [SYS] Set Store Manager as Marketing Contact | true | CONTACT | Filter criteria | 2378.0 | 2025-01-14 10:01 |  | 2024-11-18 15:52 | Jase Bajade |
| 382790401463 | [SYS] Set Deal's Redemption Chain Name | true | DEAL | Filter criteria | 5160115.0 | 2025-01-14 10:01 |  | 2024-11-15 16:04 | Jase Bajade |
| 380869680013 | [SYS] Send O2O Registration Email | true | CONTACT | Filter criteria | 7.0 | 2025-01-14 10:01 |  | 2024-11-08 12:58 | Jase Bajade |
| 380842738809 | [SYS] Route Registration Email | true | pets |  -- | 71729.0 | 2025-01-14 10:01 |  | 2024-11-08 12:27 | Jase Bajade |
| 370699681039 | MO-117 - Remove Duplicate Ebisumart Line Items | true | DEAL | Events | 263505.0 | 2025-01-14 10:01 |  | 2024-10-04 18:02 | Randika Srimal |
| 361786236068 | [SYS] Set Coupon's Breeder Flag  | true | DEAL | Filter criteria | 4928712.0 | 2025-01-14 10:01 |  | 2024-09-05 15:27 | Jase Bajade |
| 361427659929 | [SYS] Set Expired Coupons | true | DEAL | Filter criteria | 697299.0 | 2025-01-14 10:01 |  | 2024-09-04 18:00 | Jase Bajade |
| 350807335093 | [SYS] Generate Coupon Asset Code | true | coupon_assets | Filter criteria | 456.0 | 2024-12-17 15:10 | minagawa yuki | 2024-07-29 16:41 | Jase Bajade |
| 344070303144 | Valentine Day Blast | false | CONTACT | Filter criteria |  | 2024-11-22 12:19 | Jase Bajade | 2024-07-05 09:44 | Varun Mahadevan |
| 341809291338 | [SYS] Set Membership Portal Status to Active After Registration | true | CONTACT | Filter criteria | 85414.0 | 2024-11-13 15:27 | Jase Bajade | 2024-06-27 11:20 | Jase Bajade |
| 343805647258 | [SYS] Set Pet Age Type | true | pets | Filter criteria | 92159.0 | 2024-10-28 14:38 | Jase Bajade | 2024-07-04 08:54 | Jase Bajade |
| 343847434849 | New Pet Owner Blast | false | CONTACT | Filter criteria |  | 2024-10-10 10:38 | Varun Mahadevan | 2024-07-04 14:11 | Varun Mahadevan |
| 357676749477 | [SYS] Set Deal Amount | true | DEAL | Filter criteria | 4915904.0 | 2024-09-10 12:40 | Jase Bajade | 2024-08-22 15:18 | Jase Bajade |
| 353020867084 | [SYS] Set Member Number | true | CONTACT | Filter criteria | 191127.0 | 2024-09-09 17:23 | Jase Bajade | 2024-08-06 09:02 | Jase Bajade |
| 340909779786 | [MGRTN] Set Issuing Store Code When Issuing Store is Associated | true | DEAL | Filter criteria | 4276050.0 | 2024-09-06 18:02 | Jase Bajade | 2024-06-24 17:46 | Jase Bajade |
| 357190142990 | [SYS] When PKP Pet is Registered Send Registration Email | true | COMPANY | Filter criteria | 63958.0 | 2024-08-23 16:35 |  | 2024-08-21 12:14 | Jase Bajade |
| 355065548885 | 名前のないワークフロー - 2024-08-13 01:25:54 GMT+0000 | false | CONTACT |  -- |  | 2024-08-23 16:35 |  | 2024-08-13 10:25 | Joshua Lee |
| 354807714085 | [MGRTN] Set Coupon Asset Code's Coupon Product Information | true | coupon_assets | Filter criteria | 176.0 | 2024-08-23 16:35 |  | 2024-08-12 16:55 | Jase Bajade |
| 352254371121 | [SYS] Set Pet Number | true | pets | Filter criteria | 72454.0 | 2024-08-23 16:35 |  | 2024-08-02 17:58 | Jase Bajade |
| 348119326322 | [SYS] When Pet is Created Set Registration Date | true | pets | Filter criteria | 72460.0 | 2024-07-30 11:39 |  | 2024-07-19 16:38 | Jase Bajade |
| 344096489810 | Target Previous Subscription-Customers | false | CONTACT | Events |  | 2024-07-30 11:39 |  | 2024-07-05 12:50 | Varun Mahadevan |
| 342224779581 | [SYS] When Store Staff is Created Set Registration Date | true | CONTACT | Filter criteria | 711.0 | 2024-07-30 11:39 |  | 2024-06-28 16:28 | Jase Bajade |
| 340907260041 | [SYS] Set Store's Chain Code | true | COMPANY | Filter criteria | 5155.0 | 2024-07-30 11:39 |  | 2024-06-24 17:20 | Jase Bajade |
| 340904087706 | [MGRTN] Set Chain Records Chain Code | true | COMPANY | Filter criteria | 490.0 | 2024-07-30 11:39 |  | 2024-06-24 17:14 | Jase Bajade |
| 345715098165 | フォーム送信後にフォローアップEメールを送信 | true | CONTACT | Filter criteria | 127.0 | 2024-07-12 17:32 | Nagisa Ueyama | 2024-07-11 11:03 | Kaori Anzai |

---

## 次のアクション

上記「最優先で詳細確認したい候補」の4件（特に `[SYS] Set Deal's Redemption Chain Name` と `[SYS] Set Coupon Code`）について、トリガー条件・アクション内容の画面キャプチャまたは手動転記が入手できれば、`workflow-sys-send-instant-coupon.md`と同じ形式で個別ファイル化し、`report-coupon-code-redesign.html` Section 07を更新する。
