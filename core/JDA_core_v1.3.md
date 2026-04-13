# JDA Core v1.3

## Judgement-Driven Architecture

---

## 1. Introduction

近年、AIの発展により企業における意思決定のあり方が大きく変化しつつある。  
しかし、多くのAI導入は次のような問題を抱えている。

- AIをどこに適用すべきか分からない  
- PoC（概念実証）が乱立し、本番導入に至らない  
- ROIが不明確で投資判断ができない  
- AI導入そのものが目的化する  

これらの問題の根本原因は、AI導入の単位が不適切であることにある。

多くのAI導入は、業務やプロセスを単位として検討される。

業務 → 自動化

しかし企業活動は実際には次の構造を持つ。

情報 → 判断 → 行動 → 結果

企業の成果を決定するのは行動であるが、  
その行動を決定しているのは判断である。

したがって、企業活動の本質的単位は「業務」ではなく「判断」である。

さらにAI時代においては、AIモデルそのものはコモディティ化していく。  
企業の競争優位は、

・どのAIを使うか  
ではなく  
・AIに何を学習させるか  

によって決まる。

しかしここで重要な問題がある。

企業は何を学習させるべきかが定義されていない。

プロプライエタリデータの重要性は広く指摘されているが、

・どのデータを取得すべきか  
・どのように構造化すべきか  
・どのように学習に活用すべきか  

については体系的に整理されていない。

本研究はこの問いに対して次の仮説を提示する。

AI時代のプロプライエタリデータは「判断データ」である。

Judgement-Driven Architecture（JDA）は、  
企業の判断を抽出し、構造化し、ログとして蓄積し、  
人とAIがそこから学習可能になるよう設計するアーキテクチャである。

---

## 2. 基本仮説

JDAは以下の仮説に基づく。

### 仮説1：企業はデータではなく判断によって動く

企業活動はデータによって記述されるが、  
実際に企業の状態を変化させるのは判断である。  

データは判断の材料であり、  
企業の制御単位は判断である。

---

### 仮説2：企業の成果は判断の質によって決まる

行動は判断によって選択される。  

したがって、企業の成果は行動そのものではなく、  
その前提となる判断の質に依存する。

---

### 仮説3：企業文化は判断の累積である

企業文化とは、明文化されたルールではなく、  
日々の意思決定の傾向の蓄積である。  

どのような判断が繰り返されるかによって、  
企業の行動様式が形成される。

---

### 仮説4：判断ログにより組織とAIは学習可能になる

従来の企業システムは、

・データ（何が起きたか）  
・行動（何をしたか）  

は記録してきたが、

・なぜその判断をしたのか  
・どの選択肢が存在したのか  

は記録してこなかった。

このため、

行動は再現できるが、意思決定は学習できない

という限界が存在する。

判断をログとして蓄積することで、

・組織は意思決定を振り返り学習できる  
・AIは意思決定そのものを学習できる  

ようになる。

---

## 3. 判断の定義

Judgementとは、

複数の可能な未来から一つの状態を確定させる行為

である。

### 特性

・状態遷移を確定する  
・行動を拘束する  
・責任主体を持つ  

---

### Judgement Dimensions

・Proceed（進行性）  
・Validity（妥当性）  
・Accountability（責任性）  
・Venture（探索性）  

Ventureは判断の種類ではなく、探索度の軸である。

---

## 4. アーキテクチャ

Discovery → Investment → Learning

・Discovery：判断構造の発見  
・Investment：判断投資評価  
・Learning：設計・ログ・学習  

---

## 5. 判断構造

企業活動は以下の概念で整理される。

### Business Journey（BJ）

業務単位のまとまり。  
Phase0 Foundationで使用する。

例：  
・BJ01 新規クライアント獲得  
・BJ07 入金確認・消込  

---

### Judgement Journey（JJ）

判断単位の連鎖。  
Phase1 Discovery以降で使用する。  
BJを判断視点で再解釈したもの。

例：  
JJ01 アタリ判断の連鎖：  
「この会社を対象にするか？」  
→「今接触するか？」  
→「アタリと判断するか？」  
→「リストに追加するか？」

---

### Judgement Point（JP）

最小単位の判断。

「〜するか？」の形式で表現する。

判断は「顧客状態 × 企業行動」の交点で発生する。

JPは状態遷移を定義する単位であり、以下の要素を持つ。

- trigger_state  
- decision_options  
- transition_map  

例：  
・この会社に営業するか？  
・アタリと判断するか？  
・この入金はどの請求か？  

---

### Judgement Chain

BJを並べた結果として見える  
企業活動全体の判断の連鎖。

明示的に設計するものではなく、  
BJとJJが揃った時点で自然に可視化される。

BJとJJは抽象構造であり、  
実装段階ではCaseに適用されることで状態遷移として実行される。

---

### Case

CaseはBJをドメイン適用した際の判断対象単位である。  
BJの概念を置き換えるものではなく、  
BJ内の判断対象を実行単位として具体化したものである。

Case = Entity × Context

#### 同一性条件

- Entity  
- Context  
- 判断前提  

これらが変化し、判断前提が変わる場合は別Caseとする。

#### ライフサイクル

- 開始：初回状態生成  
- 進行：状態遷移  
- 終了：最終状態  
- 再開：再遷移または新Case生成  

---

### State

StateはCaseに紐づく状態であり、履歴として管理される。

- case_id  
- state_name  
- entered_at  
- entered_by_jp  
- entered_by_jlog_id  

---

### 関係性

BJ（業務単位）  
↓ 判断視点で再解釈  
JJ（判断単位）  
└ JP（最小判断）  

JPがCaseに適用されることで  
状態遷移として実行される。  

結果としてJudgement Chainが観測される。

企業活動はプロセスではなく、  
判断構造として捉えられる。

---

## 6. 投資理論

### Judgement ROI

ROI = 頻度 × 影響額 × 改善率 − コスト

---

### JULIA

JULIAは判断投資評価フレームである。

JULIAは、従来のROI中心の評価を拡張し、  
複数の観点から判断価値を評価する。

この構造は、Balanced Scorecard（BSC）の考え方を参考にしている。

---

### JULIAの評価軸

・ROI（財務）  
・Business Impact（顧客・事業）  
・Automation Potential（内部プロセス）  
・Learning Value（学習）  

JULIAはROIを内包しつつ、多面的に判断価値を評価する。

---

## 7. 学習システム

### JSC

Judgement State Control。  
状態管理および遷移制御の補助レイヤーである。

- JPが遷移定義の主構造を担う  
- JSCは制御・整合性の補助を担う  

※ JSCの完全再定義はv1.4で実施する

---

### JDC

判断設計フレーム  

---

### JLog

判断ログ  

---

### VLog

妥当性ログ  

---

### Lifecycle

Before → During → After

---

### Interface

#### JP Interface

Input:

- state_before  
- data_inputs  

Output:

- state_after  
- decision  
- JLog  

#### Judgement Interface

Input:

- state_before  
- data_inputs  
- condition_data  
- perspective_data  

Process:

- option_generation  
- evaluation  
- comparison  

Output:

- decision  
- reason  
- state_after  
- jlog_id  
- status（success / pending / rejected）  

JP InterfaceはJudgement Interfaceを内包する。

---

### Learning Loop

Data → AI → Options → Human → JLog → Action → Result → VLog → Learning

JDAは以下の統合構造として理解される。

State Machine  
+ JLog  
+ VLog  
+ Learning Loop  

JLogとVLogの差分をもとに以下を更新する。

- Condition  
- Perspective  
- Decision Criteria  

これにより、JPの以下が更新されうる。

- trigger_state  
- decision_options  
- transition_map  

FSMではtransition_mapは固定である。  
JDAではtransition_mapは更新される。  

遷移関数自体が学習対象である。

---

## 8. 実行モデル

### 8.1 状態依存実行

判断はstate_beforeに依存して実行される。

---

### 8.2 並列実行

JDAは楽観ロックモデルを採用する。

- state_versionを参照  
- 不一致時は再判断  

---

### 8.3 分散前提

- Stateは整合性管理対象  
- JLogは順序保証が必要  
- Learning更新は非同期  

Learning更新は実行中のCaseには適用しない。  
更新は次のCase生成時から有効とする。

---

## 9. AI実装

### 9.1 AI導入の問題

AI導入の失敗は単位の誤りに起因する。

業務 → 自動化（誤り）  
判断 → 改善（正しい）  

AIは行動は再現できるが、意思決定は学習できない。

---

### 9.2 JDAの役割

JDAは以下の構造でこれを解決する。

判断抽出 → JULIA → 設計 → ログ → 学習

---

### 9.3 AIエージェントの役割分類

JDAでは、AIエージェントを以下の2種類に分類する。

#### プロセスAIエージェント

**役割：**

- 業務の進行を自動化する
- タスクの実行・連携を担う
- 判断結果を受けて次の処理を進める

**対象：**

- 定型業務の自動実行
- データの収集・整理
- 通知・連携処理

#### 判断AIエージェント

**役割：**

- 判断材料を収集・整理する
- 選択肢を生成・提示する
- 過去のJLogを参照し類似判断を提示する
- 判断ログを分析する

**対象：**

- 判断支援
- 選択肢提示
- 判断傾向分析

---

### 9.4 関係性

プロセスエージェントと判断エージェントは以下の構造で連携する。

プロセスAIエージェント（何をするか）  
↓  
判断AIエージェント（どう決めるか）  
↓  
Human Judgement（最終判断）  
↓  
JLog（記録）  
↓  
学習（なぜそう決めたか）

---

### 9.5 Enterprise World Modelとの接続

この構造が継続的に循環することで、  
企業固有の意思決定モデル（Enterprise World Model）が形成される。

判断ログの蓄積により、

- プロセスエージェントは業務最適化を学習する  
- 判断エージェントは意思決定精度を向上させる  

---

## 10. 位置づけ

・BI：データ  
・BPM：プロセス  
・DDD：モデル  
・KM：知識  

JDAは意思決定を第一級の対象とするアーキテクチャである。

---

## 11. Venture Judgement

Ventureは探索性の軸であり、すべての判断は連続体上に存在する。

---

## 12. 長期ビジョン

Enterprise World Modelの構築

---

## 最終定義

JDAは、

判断を抽出し  
判断を評価し  
判断をログ化し  
判断から学習する  

ことで、

企業の意思決定能力を進化させるアーキテクチャである。

さらに、判断ログの蓄積により、  
組織とAIが同一の意思決定構造から学習できる状態が成立する。

---

## 変更履歴

| Version | 内容 |
|--------|------|
| v1.2 | 初期構造 |
| v1.3 | Case / State / Learning構造追加 |

---

## License & Citation

Copyright (c) 2026 Shun Takeda（B-AS）  
CC BY 4.0 — <https://creativecommons.org/licenses/by/4.0/>

引用：Takeda, Shun. Judgement-Driven Architecture. B-AS, 2026.  
<https://github.com/judgement-driven/Judgement-Driven-Architecture>
