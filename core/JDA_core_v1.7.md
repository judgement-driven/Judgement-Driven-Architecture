# JDA Core v1.7

## Judgement-Driven Architecture

---

# 1. Introduction

AIの発展により、企業活動における自動化対象は「作業」から「判断」へと移行しつつある。

しかし多くのAI導入は、依然として業務プロセス単位で設計されるため、

- PoCの乱立
- ROI不明確化
- 属人判断の温存
- ログ不在による学習不能

といった問題に直面している。

JDA（Judgement-Driven Architecture）は、

> 企業活動の本質単位は「業務」ではなく「判断」である

という仮説に基づき、

- 判断を抽出し
- 構造化し
- 実行し
- ログとして蓄積し
- 人とAIが継続的に学習可能にする

ためのアーキテクチャである。

ここでいう「判断」とは、単なる経営意思決定ではなく、

> 状態を確定・遷移させる広義の意思決定

を指す。

v1.6では、v1.4までの「判断設計アーキテクチャ」を拡張し、

- Judgement Harness
- Judgement Injection
- 共通JP実行
- 学習サイクル

を統合した、

> 判断実行 + 学習アーキテクチャ

として再定義した。

v1.7では、

- Judgement Scorecard（JULIA）
- Judgement Snapshot
- Learning Stage0（Learning Foundation）
- JSC / JDC の責務整理

を反映し、判断の設計・実行・記録・学習を一貫したアーキテクチャとして整理した。

---

# 2. 基本仮説

## 仮説1

企業はデータそのものではなく、  
状態を確定させる「判断」によって動く。

---

## 仮説2

ビジネス成果の真因は、  
行動そのものではなく、  
その前段に存在する判断である。

---

## 仮説3

企業文化とは、  
組織内で累積された判断の総体である。

---

## 仮説4

JLog / VLog を継続的に蓄積することで、  
組織とAIは判断モデルを学習・改善できる。

---

## 仮説5

業務（BJ）は可変だが、  
判断（JP）は組織横断で共有・再利用可能な  
独立資産である。

---

# 3. 主要概念定義

## Judgement（判断）

複数の未来可能性から、  
一つの状態を確定させる行為。

---

## Business Journey（BJ）

企業活動をマクロに捉えた  
業務・プロセス単位。

BJは判断発見のスコープである。

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

状態を確定させる最小判断単位。

JPは特定BJに従属しない。

ただし、

- DiscoveryではBJをスコープとして発見され
- 実装では複数BJから参照される

共通判断資産として扱われる。

---

## Case

判断対象単位。

```text
Case = Entity × Context
```

---

## Proposal

Proposalとは、  
Caseを実行系に具体化したインスタンスである。

Proposalの具体構造は、  
対象BJごとに定義される。

例：

```text
Company × Campaign
```

---

## State

Case / Proposal が現在どの状態にあるかを示す。

StateはJPによって遷移する。

---

## Judgement State Chart（JSC）

JSC（Judgement State Chart）は、  
判断対象（Target）が取り得る状態と、  
Judgement Point（JP）による状態遷移を定義する  
State Chartである。

State Spaceへの拡張は行わない。  
Perspective・Hypothesis等による拡張は、v1.8 Considerationsで継続検討する。

---

## Judgement Design Canvas（JDC）

判断を構成する要素

```text
Purpose / Subject / Data Sources / Conditions / Perspectives /
Decision / Actor / Accountability / Output
```

を整理し、判断材料提示・判断入力・状態遷移・JLog / VLogに接続するための設計フレームである。

---

## Judgement Log（JLog）

誰が、  
何を、  
どの材料で、  
なぜ判断したか

を記録するログ。

---

## Validity Log（VLog）

判断結果が妥当だったかを  
事後評価するログ。

また、人間による判断修正理由も含む。

---

## Judgement Snapshot

JLogでは、判断結果だけでなく、判断時点で利用可能であった情報全体を  
Judgement Snapshot として記録する。

Judgement Snapshotは、以下の論理構造を持つ。

```text
Exploration Context
↓
Candidate Snapshot
↓
Decision Context
↓
Judgement
```

これは処理順ではなく、判断時点の情報構造を表す。

実装側で名称が異なる場合は、Coreを正式名称とし、実装文書を順次合わせる。

---

# 4. 判断構造

## 4.1 基本構造

```text
Entity
×
Context
↓
Case
↓
State
↓
JP
↓
State Change
↓
JLog / VLog
```

---

## 4.2 BJ-JP N:Mマッピング

JDAでは、  
BJとJPを完全分離する。

```text
BJ-A ─┐
       ├─► JP-X
BJ-B ─┘
```

同一JPは、  
複数BJから共有される。

---

## 4.3 JP共有の意味

これにより：

- 判断ロジックの重複を排除
- 判断品質を統一
- JLogスキーマ統一
- 組織横断学習

が可能になる。

---

# 5. Judgement Scorecard（JULIA）

JDAでは、

> どの判断に投資するか

を中心に設計する。

---

## 5.1 Judgement Investment

JDAでは、

「どの判断へ投資するか」

を中心に設計する。

JULIAは、
判断改善・判断実装・判断学習への投資優先度を決定するための仕組みである。

---

## 5.2 JULIA（Judgement Scorecard）

JULIAは、正式には

> Judgement Scorecard

として定義する。

JULIAという呼称は頭字語の正式展開ではなく、JDAにおける評価フレームワークの通称として用いる。

JULIA（Judgement Scorecard）は、
JP（Judgement Point）の重要度を評価し、
どの判断から改善・実装・学習を進めるべきかを決定するための評価フレームワークである。

JDAでは、

- 発見したJP
- 設計対象とするJP
- 実装対象とするJP

を同一視しない。

JULIAは、限られたリソースの中で、
どの判断へ投資するかを決定するために用いる。

評価軸：

- J = Judgement Financial Impact
- U = Urgency & Frequency
- L = Latency
- I = Influence Scope
- A = Adaptive / Learning Value

### Judgement Financial Impact

その判断が売上・利益・コスト・損失回避に
どれだけ影響するか。

### Urgency & Frequency

その判断がどれだけ頻繁に発生し、
どれだけ迅速な対応を求められるか。

### Latency

その判断が停滞した場合、
業務や意思決定全体にどの程度の
ボトルネックを生むか。

### Influence Scope

その判断結果が、
後続の判断や他部門へ
どれだけ影響を与えるか。

### Adaptive / Learning Value

その判断を記録・学習することで、
将来的な判断品質向上に
どれだけ寄与するか。

---

## 5.3 評価単位と実装単位

```text
評価単位：JP
実装単位：BJ
```

とする。

---

# 6. JSC / JDC の責務整理

JDAでは、JSCとJDCの責務を以下のように明確化する。

```text
JSC
=
状態と状態遷移を定義する。

JDC
=
その状態遷移を決定する
Data Sources・Conditions・Perspectives・Actor・Accountabilityを定義する。
```

v1.6までは、この責務をProceed（進行） / Validity（妥当性） / Accountability（責任） / Venture（探索性）という4軸（Judgement Dimensions）で表現していた。

v1.7では、この4軸をJDCの正式構造としては採用しない。各概念はDecision・Output・Data Sources・Conditions・Perspectives・Actor・Accountabilityへ整理された。

旧4軸構造は履歴として残し、必要に応じてv1.8以降で再検討する。

---

# 7. 実行モデル

## 7.1 Judgement Harness

Judgement Harnessとは、

> 判断を再現可能かつ学習可能に実行するため、  
> 状態・入力・実行・ログを統一する実行基盤

である。

JDAでは、JPを外部定義として管理し、Judgement Harness上で実行する。
Judgement Harnessは、Judgement InjectionによってJP定義と実行基盤を分離する。

---

## 7.2 Judgement Injection

Judgement Injectionとは、

> JPをJudgement Harness内部で取得するのではなく、  
> 外部で決定し、  
> 実行基盤へ注入する構造

である。

---

## 7.3 execute_jp

```python
jp = injected_jp
execute_jp(proposal, jp, data_sources, actor)
```

execute_jp は：

- state取得
- 判断実行
- state遷移
- JLog保存

を統一的に行う。

---

## 7.4 Proposal中心実行

実行単位はCaseそのものではなく、  
Caseを具体化したProposalである。

---

# 8. Dynamic Data Sources（Hypothesis Injection）

HypothesisやDynamic Data Sourcesは、v1.8 Considerationsで検討する。

v1.7では正式概念として扱わない。

---

# 9. Learning Cycle

## 9.0 Stage0：Learning Foundation

まず、VLogを継続的に取得できる状態を構築する。

```text
JP実行
↓
状態確定
↓
後続状態追跡
↓
VLog生成
```

Stage0では判断モデルを改善しない。

評価基盤を作ることがStage0の目的であり、Stage0が成立して初めてStage1以降のLearningが始まる。

---

## 9.1 Stage1：Judgement Material Learning（支援段階）

```text
AI → Options → Human
```

AIは判断を代替するのではなく、判断に必要なData Sourcesを収集・整理・加工・構造化し、人が判断できる状態を作る。

AIは以下を支援する。

- Data Sourcesの収集
- Data Sourcesの整理・加工
- 判断に有用な形への構造化
- 人への提示

判断責任は人間が持つ。

---

## 9.2 Stage2：Judgement Reproduction Learning（再現段階）

```text
AI → Suggested Decision → Human Confirm
```

過去JLogから、  
AIが判断再現を試みる。

---

## 9.3 Stage3：Judgement Delegation（委譲段階）

```text
AI → Decision
Human → Review
```

十分なJLog / VLog蓄積後、  
判断は段階的にAIへ委譲される。

---

## 9.4 学習対象

Learning Cycleでは：

- Conditions
- Perspectives

が更新対象となる。

※ Threshold（判断閾値）およびHypothesis Patternはv1.7では正式概念として扱わない。必要であればv1.8 Considerationsで再検討する。

---

# 10. AIエージェント

## Process AI Agent

作業・処理を実行するAI。

例：

- データ収集
- PDF解析
- メール送信

---

## Judgement AI Agent

判断材料整理・判断提案・判断再現を行うAI。

---

# 11. Enterprise World Model

JDAの長期目標は、

> 組織内の判断構造を継続学習し、  
> 企業固有の意思決定世界モデルを形成すること

である。

---

# 12. 最終定義

JDAとは、

> 判断を抽出し、  
> 構造化し、  
> 実行し、  
> ログとして蓄積し、  
> 学習によって継続改善する

ための、

> 判断実行 + 学習アーキテクチャ

である。

---

# 13. 変更履歴

| Version | 内容 |
|---|---|
| v1.0 | 初版 |
| v1.1 | Judgment Chain / JJ / JP 構造導入 |
| v1.2 | BJ / JJ / JP の役割整理 |
| v1.3 | Case / JSC / JDC / JLog / VLog 導入 |
| v1.4 | Learning Loop / Delegation / Venture Judgement 強化 |
| v1.5 | Judgement Harnessおよび Judgement Injection の検討開始（実装検証フェーズ） |
| v1.6 | 判断実行 + 学習アーキテクチャへ拡張 / BJ-JP N:M構造 / 共通JP実行 / Learning Cycle統合 |
| v1.7 | JULIAをJudgement Scorecardとして正式定義 / JSCを判断対象（Target）起点のState Chart定義へ統一 / Judgement Snapshot（Exploration Context・Candidate Snapshot・Decision Context・Judgement）を正式追加 / Learning CycleへStage0（Learning Foundation）を追加し、Stage1〜3をJudgement Material Learning・Judgement Reproduction Learning・Judgement Delegationへ統一 / JSC・JDCの責務整理を新設し、Proceed・Validity・Accountability・Venture（Judgement Dimensions）の4軸構造をJDCの正式構造から除外（履歴として保持、v1.8以降で再検討） / JDCをPurpose・Subject・Data Sources・Conditions・Perspectives・Decision・Actor・Accountability・Outputの9要素で再定義 / 「Judgement Definition Canvas」を「Judgement Design Canvas」へ名称修正 / 判断材料・Condition・PerspectiveをData Sources・Conditions・Perspectivesへ用語統一 / Thresholdを正式概念から除外 / execute_jpのパラメータをinput_dataからdata_sourcesへ統一 / Dynamic Data Sources（Hypothesis Injection）をv1.7正式概念から除外し、Hypothesis・Perspective/Hypothesis Framework・JSC拡張とあわせてv1.8 Considerationsへ移動 / Phase3〜Phase6・README v1.7との整合 |
