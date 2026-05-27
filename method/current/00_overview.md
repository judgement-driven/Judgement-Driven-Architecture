# JDA Method v1.6 Overview

---

# 1. 概要

Judgement Driven Architecture（JDA）は、  
企業活動を「業務」ではなく「判断構造」として捉え、

- 判断の抽出
- 判断の構造化
- 判断の実行
- 判断ログの蓄積
- 学習による継続改善

を通じて、

組織とAIの意思決定能力を向上させるための方法論である。

JDAは単なる業務改善手法ではない。

JDAは、

> 判断を実行可能かつ学習可能な構造として再設計するためのアーキテクチャ

である。

v1.6では、

- ハーネスエンジニアリング
- Judgement Injection
- execute_jp
- Learning Cycle

を統合し、

> 判断実行 + 学習アーキテクチャ

として再定義した。

---

# 2. 基本思想

- 企業活動はプロセスではなく、Case / Proposal の状態遷移として構造化される
- 業務改善ではなく、判断能力の強化を目的とする
- AIは行動ではなく、判断支援・判断再現・判断委譲に使う
- 判断ログ（JLog / VLog）が組織学習の基盤となる
- JPはBJに従属しない共通判断資産として扱う
- 実装は個別画面ではなく、共通JP実行基盤（ハーネス）上で行う

---

# 3. フェーズ構造

JDAは以下のフェーズで構成される。

---

## phase0 foundation

- 業務と判断ドメインの整理
- Business Journey（BJ）の抽出
- 判断対象領域の定義

---

## phase1 discovery

- BJをスコープとしてJPを発見
- JPの抽出
- 判断構造の把握

---

## phase2 julia

- JPの評価
- 優先順位付け
- 投資判断
- 実装対象の選定

---

## phase3 design

- 判断構造の設計（JSC）
- 判断委譲の設計（JDC）
- 状態遷移設計
- 判断条件・観点の整理

---

## phase4 log

- 判断ログ設計（JLog）
- 妥当性ログ設計（VLog）
- Learning対象の定義

---

## phase5 implementation

- 共通JP実行基盤（ハーネス）の構築
- execute_jpによる統一実行
- Judgement Injection
- Proposal中心実装
- 状態遷移実装
- UI実装
- Judgement Journey（JJ）の形成

---

## phase6 learning

- JLog / VLogを用いた学習
- 判断再現
- 判断委譲
- Condition / Perspective / Threshold の更新
- Learning Cycleの継続

---

# 4. 全体の流れ

```text
BMC
↓
Business Journey（BJ）
↓
Judgement Point（JP）抽出
↓
JULIA
↓
Design
↓
Implementation（JJ形成）
↓
Execution
↓
Log
↓
Learning
```

---

# 5. 判断構造

---

## Business Journey（BJ）

BJとは、
企業活動を業務視点で捉えたJourneyである。

BJは、

- 業務単位
- プロセス単位
- ビジネス活動単位

として定義される。

JDAでは、
BJをJP発見のスコープとして使用する。

---

## Judgement Journey（JJ）

JJとは、
JPの実行連鎖によって形成される
判断実行構造である。

JPはDiscovery時にBJをスコープとして発見されるが、
発見後はBJから独立した共通判断資産として扱われる。

JJは、
ハーネス上でCase / Proposalの
状態・属性・判断材料が
どのように更新されていくかを表す
判断実行視点のJourneyである。

JJは業務フローそのものではなく、
JP同士の依存・分岐・再利用関係によって形成される。

多くの場合、
BJとJJは近い構造を持つ。

ただし、

- JP共有
- execute_jp共通化
- Learning
- Proposal進化

が進む場合、
JJはBJを超えた独立構造として扱われる。

そのため、

- 業務視点を扱う場合はBJ
- 判断実行構造を扱う場合はJJ

を用いる。

---

## Judgement Point（JP）

JPとは、
状態を確定・遷移させる最小判断単位である。

JPは：

- 「〜するか？」
- 状態遷移を伴う
- JLog / VLogを持つ
- Learning対象となる

構造として定義される。

JPは特定BJに従属せず、
複数BJから共有されうる共通判断資産として扱う。

---

# 6. 実行単位

---

## Case

Caseとは、
判断対象単位である。

```text
Case = Entity × Context
```

---

## Proposal

Proposalとは、
Caseを実行系に具体化したインスタンスである。

Proposalは：

- 状態
- 判断
- ログ
- 学習

の中心単位となる。

---

# 7. JDAの本質

JDAは、

> 判断を実行可能かつ学習可能な構造として定義するアーキテクチャ

である。

JLog / VLog は単なる監査ログではない。

それらは：

- 判断再現
- 判断改善
- 判断委譲
- AI学習
- 組織学習

を可能にする学習資源である。

---

# 8. Learning Cycle

JDAでは、
以下のLearning Cycleを前提とする。

---

## 8.1 支援段階

```text
AI → Options → Human
```

AIは判断材料を提示し、
人間が判断する。

---

## 8.2 再現段階

```text
AI → Suggested Decision → Human Confirm
```

AIが過去JLogから
判断再現を試みる。

---

## 8.3 委譲段階

```text
AI → Decision
Human → Review
```

十分な学習後、
判断は段階的にAIへ委譲される。

---

# 9. 実装における補足（JDA版ハーネスエンジニアリング）

JDAは特定技術を前提としない。

しかし、
判断を実行可能かつ学習可能にするためには、
共通実行基盤が必要となる。

JDAではこれを：

> JDA版ハーネスエンジニアリング

と呼ぶ。

---

## 9.1 定義

JDA版ハーネスエンジニアリングとは、

> 状態遷移を伴う判断（JP）を、
> 再現可能かつ学習可能な形で実行するために、
> 状態・入力・実行・ログを統一する実装基盤

である。

---

## 9.2 Judgement Injection

Judgement Injectionとは、

> JPをハーネス内部で取得するのではなく、
> 外部定義として注入する実装方式

である。

---

## 9.3 execute_jp

```python
jp = external_resolve(...)
execute_jp(proposal, jp, input_data, actor)
```

execute_jp は：

- state取得
- 判断実行
- state遷移
- JLog保存

を統一的に行う。

---

# 10. 進め方の原則

---

## 10.1 いきなり全体最適を狙わない

- 1つのBJから開始する

---

## 10.2 作業ではなく判断を見る

- フローではなく判断を対象とする

---

## 10.3 正解を求めない

- 判断抽出が重要

---

## 10.4 作りながら学習する

- 小さく作る
- 早く回す
- 実装から学ぶ

---

# 11. 本Methodの位置づけ

| 分野 | 対象 |
|------|------|
| BPM | 業務フロー |
| BI | データ |
| AI | モデル |
| JDA | 判断 |

---

# 12. 変更履歴

| Version | 内容 |
|---|---|
| v1.3 | Case / JSC / JDC / JLog / VLog 導入 |
| v1.4 | Learning Loop / Delegation / Venture Judgement 強化 |
| v1.5 | Judgement Injection / JP共有構造の検討 |
| v1.6 | 判断実行 + 学習アーキテクチャへ拡張 / JJ再定義 / execute_jp統合 |