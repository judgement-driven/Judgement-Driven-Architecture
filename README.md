# Judgement-Driven Architecture（判断ドリブンアーキテクチャ）

> This repository contains the original Japanese version of the  
> Judgement-Driven Architecture（JDA） theory and method.
>
> English version:  
> <https://github.com/judgement-driven/Judgement-Driven-Architecture-EN>
>
> 判断を残す。組織が賢くなる。  
> 企業はデータではなく「判断」で動いている。

---

## One-line definition

JDA（Judgement-Driven Architecture）は、企業活動に存在する判断を抽出・設計・実行・記録・評価・学習することで、組織の意思決定能力を継続的に進化させるアーキテクチャである。

---

## なぜJDAを考えたのか

多くの業務は、業務フローとして記述される。

しかし実際の仕事は、そのフローの通りにきれいには流れない。

実際の仕事は、都度発生する「判断」によって流れが変わる。

- この案件を進めるか
- この企業を対象にするか
- この例外を許容するか
- どちらを優先するか
- ここで保留するか
- 誰に確認するか

業務は、こうした判断の積み重ねで進んでいる。

しかし多くの組織では、その判断が次のような状態になっている。

- 属人化している
- 判断理由が残らない
- 判断材料が共有されない
- 妥当性が検証されない
- AIが学習できない

企業にはすでに多くのデータが存在する。

事実データ：

- 売上
- 受発注情報
- 顧客情報
- 請求情報

行動データ：

- クリック
- 閲覧
- 購買
- 接触履歴

しかし企業活動には、もう一つ重要なデータがある。

> 判断データ

である。

AI時代において、AIモデルはコモディティ化していく。

競争力は、

- どのAIを使うか

ではなく、

- 何を学習させるか
- どの判断を記録しているか
- どの判断材料を改善できるか

に移っていく。

JDAは、その判断データを組織の学習資産に変えるためのアーキテクチャである。

---

## 判断の定義

JDAにおいて、判断とは、

> 状態を確定させる行為

である。

判断は、単なるif分岐ではない。

判断は、業務上の状態を変え、次の行動や次の判断を決める。

```text
状態A
↓
判断
↓
状態B
```

JDAでは、判断を以下の4側面から扱う。

- Proceed（進行）
- Validity（妥当性）
- Accountability（責任）
- Venture（探索性）

---

## JDAとは何か

JDAは、判断を第一級の設計対象として扱うアーキテクチャである。

JDAでは、業務を単なるプロセスやタスクの集合として見ない。

業務の中に存在する判断を抽出し、その判断を次のように扱う。

```text
発見する
↓
評価する
↓
設計する
↓
記録する
↓
実行する
↓
学習する
```

JDAの目的は、AIにいきなり判断を任せることではない。

まず、人間がどこで判断しているかを明らかにする。

次に、その判断に必要な材料を整理する。

そして、判断結果・判断理由・判断材料・状態遷移をログとして残す。

そのうえで、AIは判断材料の生成や整理を支援する。

JLog / VLog が蓄積されることで、AIは過去判断を再現し、将来的には一部判断を委譲できる可能性が生まれる。

---

## JDAの基本構造

JDA v1.6では、Business Journey（BJ）をスコープとしてJudgement Point（JP）を発見し、そのJPを設計・記録・実行・学習の中心単位として扱う。

```text
BJ（発見スコープ）→ JP（判断点）
JP → JSC / JDC（設計）
JP → JLog / VLog（記録）
JP → Learning Cycle（学習）
```

この構造により、JDAは業務フローそのものではなく、業務の中にある判断を継続的に改善する。

---

## Business Journey（BJ）

Business Journey（BJ）は、判断を内包する業務の意味単位である。

BJは、業務フローそのものではない。

BJは、Phase1 DiscoveryでJudgement Point（JP）を発見するためのスコープである。

例：

- BJ01 新規クライアント獲得
- BJ02 提案作成
- BJ03 制作進行
- BJ04 請求処理
- BJ05 入金確認・回収

---

## Judgement Point（JP）

Judgement Point（JP）は、業務の中で実際に発生している判断点である。

JPは、原則として次の形式で表現する。

```text
〜するか？
```

例：

- この企業を対象にするか？
- この案件を優先するか？
- この企業に接触するか？
- アタリと判断するか？
- この入金はどの請求か？
- 保留するか？

JPは、単なる作業ではない。

その答えによって、後続の状態・行動・判断が変わるものをJPとして扱う。

---

## Case / Proposal

JDAでは、判断対象を Case として扱う。

```text
Case = Entity × Context
```

実装上は、Caseを具体化した実行単位として Proposal を用いる場合がある。

例：BJ01 新規クライアント獲得

```text
Entity = Company
Context = Campaign
Proposal = Company × Campaign
```

Proposalは、以下の中心単位となる。

- State管理
- JP実行
- JLog記録
- VLog評価
- Learning対象

---

## Judgement Journey（JJ）

Judgement Journey（JJ）は、JPの実行連鎖である。

JPはPhase1 DiscoveryでBJをスコープとして発見される。

ただし、発見後のJPはBJに従属し続けるのではなく、判断資産として扱われる。

JJは、Phase5 Implementation以降で、JPがハーネス上で実行され、Proposal / Case の状態・属性・判断材料が更新されていく中で形成・観測される。

つまり、

```text
BJ = JPを発見するための業務スコープ
JP = 判断点
JJ = JPが実行されることで形成される判断実行視点のJourney
```

である。

---

## JSC

JSC（Judgement State Chart）は、判断による状態遷移を設計するための成果物である。

JSCでは、処理手順ではなく状態を扱う。

```text
before_state
↓
JP
↓
after_state
```

JDAでは、判断を状態遷移として扱う。

そのため、保留・例外・再判断も状態として設計する。

---

## JDC

JDC（Judgement Design Canvas）は、判断の中身を設計するための成果物である。

JDCでは、以下を整理する。

- 判断の定義
- 判断対象
- 判断材料
- 判断条件
- 判断観点
- 判断結果
- 判断主体
- 判断責任
- 判断確定方式
- 出力

JDCは、判断を実装可能・記録可能・学習可能にするための設計キャンバスである。

---

## JLog / VLog

JDAでは、判断を2種類のログとして記録する。

```text
JLog = 判断時の記録
VLog = 判断後の妥当性評価
```

JLogには、以下を記録する。

- 誰が判断したか
- 何を判断したか
- どの状態で判断したか
- どの判断材料を使ったか
- どの条件・観点で判断したか
- なぜ判断したか
- 判断後にどの状態へ遷移したか

VLogには、以下を記録する。

- 判断結果は妥当だったか
- 判断材料は適切だったか
- 判断理由は妥当だったか
- 実際の結果はどうだったか
- 次回どう改善するか

JLogとVLogが揃うことで、判断は学習可能になる。

---

## Judgement Injection

Judgement Injection は、JDA v1.6における重要な実装概念である。

従来の業務システムでは、判断ロジックはコードや画面に埋め込まれやすい。

JDAでは、JPをコードに埋め込むのではなく、定義として管理し、実行基盤に注入する。

```text
JP定義
↓
execute_jp
↓
State遷移
↓
JLog保存
```

これにより、JP定義と実行基盤を分離できる。

判断をコードではなく、設計対象・運用対象・学習対象として扱えるようになる。

---

## Learning Cycle

JDAのLearning Cycleは、判断を段階的に学習可能にする。

```text
判断材料学習
↓
判断再現学習
↓
判断委譲
```

### Stage0：Learning Foundation

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
評価基盤を作ることがStage0の目的である。

### Stage1：Judgement Material Learning

まず、AIは判断そのものではなく、判断材料の生成・整理を支援する。

例：

- 類似Caseを出す
- 過去判断を示す
- 判断材料を要約する
- 判断理由の入力を補助する
- 不足情報を提示する

### Stage2：Judgement Reproduction Learning

JLog / VLog が蓄積されると、AIは過去判断の再現を支援できるようになる。

```text
AI → Suggested Judgement
Human → Confirm
```

### Stage3：Judgement Delegation

十分なログと妥当性評価が蓄積された判断は、条件付きでAIへ委譲できる可能性がある。

ただし、判断委譲にはAccountability（責任）の設計が必要である。

---

## JDA Method v1.6

JDA Method v1.6 は、以下のフェーズで構成される。

```text
Phase0 Foundation
↓
Phase1 Discovery
↓
Phase2 JULIA
↓
Phase3 Design
↓
Phase4 Log
↓
Phase5 Implementation
↓
Phase6 Learning
```

---

### Phase0 Foundation

判断ドメインと業務全体像を把握し、Phase1 Discoveryの対象となるBusiness Journey（BJ）を定義する。

主な成果物：

- JDA-BMC
- Business Journey一覧
- 対象BJ

---

### Phase1 Discovery

対象BJをスコープとして、実際に発生しているJudgement Point（JP）を抽出する。

Phase1では、JJを設計しない。

主な成果物：

- JP一覧
- JP精査記録
- JP統合・除外理由メモ

---

### Phase2 JULIA

JULIA（Judgement Log Impact Assessment）は、Phase1で抽出したJPを評価し、どの判断を優先的に扱うかを決定する工程である。

JULIAでは、以下の4軸で評価する。

- ROI
- Business Impact
- Automation Potential
- Learning Value

Phase2 JULIAは、

```text
どのJPに投資するか
```

を決める工程である。

---

### Phase3 Design

Phase2で選定されたJPに対して、判断の構造・状態遷移・判断材料・責任・実行単位を設計する。

主な成果物：

- JSC（Judgement State Chart）
- JDC（Judgement Design Canvas）
- Case / Proposal 定義
- 判断材料定義
- 判断責任定義

---

### Phase4 Log

Phase3で設計した判断を、JLog / VLogとして記録可能にする。

主な成果物：

- JLog定義
- VLog定義
- ログ項目一覧
- 記録タイミング定義
- 妥当性評価方法

---

### Phase5 Implementation

JDAの実行基盤を設計・実装する。

Phase5では、Judgement Injectionにより、JP定義と実行基盤を分離する。

主な要素：

- ハーネス
- execute_jp
- State管理
- Proposal / Case
- JLog / VLog
- Operational Bridge

---

### Phase6 Learning

JLog / VLogをもとに、判断材料・判断条件・判断観点・判断閾値を改善する。

Phase6では、以下の3段階で学習する。

```text
Judgement Material Learning
↓
Judgement Reproduction Learning
↓
Judgement Delegation
```

---

## Implementation Pattern

JDA v1.6では、実装の進め方として Judgement Slice Implementation を定義する。

Judgement Slice Implementation とは、

> JULIAで選定された主要JPを中心に、  
> 判断材料提示・判断入力・状態遷移・JLog / VLog収集を先に実装し、  
> 前後工程はOperational Bridgeで補完しながら、  
> 必要に応じて周辺JPの実行環境を追加していく実装パターン

である。

JDA実装では、最初から全てを作り込まない。

```text
判断ログが取れる最小構造で先に現場へ出す
```

ことを優先する。

JJは、このようなPhase5以降の実装・運用の中で、JP実行連鎖として形成・観測される。

---

## AIとの関係

JDAは、AI導入理論ではない。

JDAは、AIが判断支援し、学習できる構造を設計する理論である。

初期段階では、AIは判断しない。

AIは判断材料を提示する。

```text
AI = 判断材料支援
Human = 判断主体
JLog / VLog = 学習資産
```

AIに判断を委譲するのは、JLog / VLogが蓄積され、妥当性が確認された後である。

---

## 長期ビジョン

JDAの長期ビジョンは、Enterprise World Model（企業世界モデル）の構築である。

企業は、業務データだけでなく、判断データを蓄積することで、自社固有の判断構造を学習できる。

その結果、企業は以下を持つことができる。

- 自社固有の判断履歴
- 自社固有の判断材料
- 自社固有の判断基準
- 自社固有の判断モデル

JDAは、そのための判断アーキテクチャである。

---

## Repository structure

```text
.
├── core/
│   └── JDA_core_v1.6.md
│
├── method/
│   ├── 00_overview.md
│   ├── 01_phase0_foundation.md
│   ├── 02_phase1_discovery.md
│   ├── 03_phase2_julia.md
│   ├── 04_phase3_design.md
│   ├── 05_phase4_log.md
│   ├── 06_phase5_implementation.md
│   ├── 06a_judgement_slice_implementation.md
│   ├── 07_phase6_learning.md
│   └── JDA_BMC_definition.md
│
├── implementation/
│   └── BJ01/
│
└── tools/
    └── jda_phase_template.html
```

※ 実際のディレクトリ構成は、今後変更される可能性がある。

---

## Current status

JDA Core v1.6 and JDA Method v1.6 are currently organized around:

- Judgement as state transition
- Business Journey as JP discovery scope
- JP as reusable judgement asset
- Judgement Injection
- JLog / VLog
- Judgement Slice Implementation
- Learning Cycle

---

## License

Copyright (c) 2026 Shun Takeda（B-AS）

This project is licensed under the  
Creative Commons Attribution 4.0 International License（CC BY 4.0）.

You are free to:

- Share — copy and redistribute the material in any medium or format
- Adapt — remix, transform, and build upon the material for any purpose, even commercially

Under the following condition:

- Attribution — You must give appropriate credit to the original author.

License text:

<https://creativecommons.org/licenses/by/4.0/>

---

## Citation

If you use this theory in research, articles, presentations, or other materials, please cite it as follows:

Judgement-Driven Architecture（判断ドリブンアーキテクチャ）  
Author: Shun Takeda（B-AS）  
GitHub Repository  
<https://github.com/judgement-driven/Judgement-Driven-Architecture>

### BibTeX

```bibtex
@misc{JDA2026,
  title={Judgement-Driven Architecture},
  author={Takeda, Shun},
  year={2026},
  howpublished={GitHub Repository},
  organization={B-AS},
  url={https://github.com/judgement-driven/Judgement-Driven-Architecture}
}
```