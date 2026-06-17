# JDA Core v1.6

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

として再定義する。

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

```
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

JPによって引き起こされる  
状態遷移モデル。

---

## Judgement Definition Canvas（JDC）

判断材料・条件・観点・責任主体を定義する設計モデル。

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

JDA v1.6では、  
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

# 5. Judgement ROI と JULIA

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

## 5.2 JULIA

JULIA（Judgement Impact Assessment）は、

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

# 6. Judgement Dimensions

JDAでは判断を以下の4軸で扱う。

---

## Proceed Judgement

進めるか？

---

## Validity Judgement

妥当か？

---

## Accountability Judgement

誰が責任を持つか？

---

## Venture Judgement

未知・探索を許容するか？

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

## 7.4 Proposal中心実行

実行単位はCaseそのものではなく、  
Caseを具体化したProposalである。

---

# 8. 動的判断材料（Hypothesis Injection）

従来システムが扱えなかった：

- 直感
- 違和感
- 温度感
- 兆候

を、

> Dynamic Judgement Material

として第一級オブジェクト化する。

---

## 8.1 構造

```text
Facts
+
Hypothesis
↓
Judgement
```

---

## 8.2 学習対象

HypothesisもJLog / VLogと結合され、  
将来的な学習対象となる。

---

# 9. Learning Cycle

## 9.1 支援段階

```text
AI → Options → Human
```

AIは：

- 情報整理
- 選択肢生成
- 判断材料提示

を行う。

判断責任は人間が持つ。

---

## 9.2 再現段階

```text
AI → Suggested Decision → Human Confirm
```

過去JLogから、  
AIが判断再現を試みる。

---

## 9.3 委譲段階

```text
AI → Decision
Human → Review
```

十分なJLog / VLog蓄積後、  
判断は段階的にAIへ委譲される。

---

## 9.4 学習対象

Learning Loopでは：

- Condition
- Perspective
- Threshold
- Hypothesis Pattern

が更新対象となる。

### Threshold

Thresholdとは、  
判断を成立・遷移させるための  
境界条件・しきい値を指す。

例：

- スコア何点以上でアタリとするか
- どの温度感で接触優先とするか
- どの条件で保留へ遷移するか

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