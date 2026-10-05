# Judgement-Driven Architecture（判断ドリブンアーキテクチャ）

> このリポジトリは、Judgement-Driven Architecture（JDA）の理論と方法論の日本語原版です。
>
> 英語版：  
> <https://github.com/judgement-driven/Judgement-Driven-Architecture-EN>
>
> 判断を残す。組織が賢くなる。  
> 企業はデータではなく「判断」で動いている。

---

## JDAの定義

JDA（Judgement-Driven Architecture）は、企業活動にある判断を抽出・設計・実行・記録・評価・学習することで、組織の意思決定能力の継続的な改善を目指すアーキテクチャである。

---

## 最新の安定版

現在のバージョン

- JDA Core v1.7
- JDA Method v1.7

このリポジトリでは、JDAの現行の安定版を公開しています。

---

## JDAを使うメリット

JDAがなくてもAI導入はできる。JDAは、どの判断に投資し、何を任せ、どう評価するかを明確にすることで、AI・システム導入が成果につながる確率を高めることを目指す。以下は設計上の狙いであり、効果の実証は継続中である。

1. **改善する判断を選べる。** 事業への影響、頻度・緊急性、判断の停滞、影響範囲、学習価値から、限られた開発・運用資源の投資先を決める。
2. **重要な判断から小さく実装できる。** BJで業務の範囲を捉え、JPを抽出する。業務全体の詳細フローを先に完成させることを前提とせず、選んだ判断に必要な材料取得・実行・後続アクションを具体化する。
3. **複雑さや例外を判断の対象として扱える。** 標準手順だけで処理できない状況についても、必要な材料、判断観点、責任、保留・引き継ぎ先を設計する。AIによる情報整理や判断支援を使いつつ、判断できない場合の扱いも残す。
4. **導入後に判断を検証し、改善できる。** 判断時点の材料・理由と、その後の結果・妥当性評価を結びつけ、判断材料や基準、AIへの委譲範囲を見直す。記録と知見をモデルの外に残すことで、技術を変更しても引き継ぎやすくする。

---

## 他の手法・技術との関係

JDAは、業務フローの可視化やECRS、AI・Agentなどの実行技術と組み合わせて使う。

```text
業務 → 判断 → 必要な材料・タスク → 実行フロー → システム
```

違いは、判断を独立した設計・記録・評価の単位として先に扱うことにある。タスクやフローの設計を省略するわけではない。ECRSで作業を改善しながら、判断には必要な材料・責任・記録を設ける。従来手法でも判断や例外は扱えるため、JDAだけが可能にするものとは位置づけない。

JDAは実行技術を選ぶ前の判断設計に加え、実行・記録・評価・学習までを扱う。判断ごとに、人・ルール・AIの役割を設計する。

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

AIや実行技術の選択に加え、どの判断を記録し、どの材料や基準を改善するかが重要になる。

判断データは、事実データ・行動データと結びつけることで、組織固有の判断を振り返る材料となる。

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
実行・記録する
↓
妥当性を評価する
↓
学習する
```

JDAの目的は、AIにいきなり判断を任せることではない。

まず、人間がどこで判断しているかを明らかにする。

次に、その判断に必要な材料を整理する。

そして、判断結果・判断理由・判断材料・状態遷移をログとして残す。

そのうえで、AIは判断材料の生成や整理を支援する。

JLog / VLogをもとに、AIによる過去判断の再現を試み、妥当性を検証する。その評価と責任設計を踏まえ、一部判断の委譲を検討する。ログの蓄積だけで再現や改善が成立するわけではない。

---

## JDAの基本構造

JDA v1.7では、Business Journey（BJ）をスコープとしてJudgement Point（JP）を発見し、そのJPを設計・記録・実行・学習の中心単位として扱う。

```text
BJ（発見スコープ）→ JP（判断点）
JP → JSC / JDC（設計）
JP → JLog / VLog（記録）
JP → Learning Cycle（学習）
```

この構造により、JDAは業務フローそのものではなく、業務の中にある判断を継続的に改善する。

---

## Business Journey（BJ）

Business Journey（BJ）は、企業活動をマクロに捉えた業務・プロセス単位であり、判断発見のスコープである。

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

Judgement Point（JP）は、状態を確定させる最小判断単位である。

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

Proposalは、Caseを実行系に具体化したインスタンスである。具体構造は対象BJごとに定義する。

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

JJは、Phase5 Implementation以降で、JPがJudgement Harness上で実行され、Proposal / Case の状態・属性・判断材料が更新されていく中で形成・観測される。

つまり、

```text
BJ = JPを発見するための業務スコープ
JP = 判断点
JJ = JPが実行されることで形成される判断実行視点のJourney
```

である。

---

## JSC

JSC（Judgement State Chart）は、判断対象（Target）が取り得る状態と、Judgement Point（JP）による状態遷移を定義するState Chartである。

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

- Purpose
- Subject
- Data Sources
- Conditions
- Perspectives
- Decision
- Actor
- Accountability
- Output

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



JLogでは、判断結果だけでなく、判断時点で利用可能であった情報全体を **Judgement Snapshot** として記録する。

```text
Exploration Context
↓
Candidate Snapshot
↓
Decision Context
↓
Judgement
```

この4層は処理順ではなく、判断時点の情報構造を表す。

材料が欠けたまま判断した場合は、欠損内容と把握できた理由（取得失敗・未取得・該当情報なし等）も残す。

JLogとVLogを対応づけることで、判断を検証する基盤ができる。結果だけで判断の良し悪しを決めず、判断時に利用できた材料や、その後の実行状況も確認する。例えば営業では、未接触・結果待ち・接触後の反応なしを区別して記録できるように設計することが望ましい。反応なしという結果だけで、判断が不妥当だったとは決めない。

---

## Judgement Injection

Judgement Injection は、JDAにおける重要な実装概念である。

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
Learning Foundation
↓
Judgement Material Learning
↓
Judgement Reproduction Learning
↓
Judgement Delegation
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

JLog / VLogをもとに、AIが過去判断の再現を試み、その妥当性を検証する。

```text
AI → Suggested Judgement
Human → Confirm
```

### Stage3：Judgement Delegation

十分なログと妥当性評価が蓄積された判断は、条件付きでAIへ委譲できる可能性がある。

ただし、判断委譲にはAccountability（責任）の設計が必要である。

---

## JDA Method v1.7

JDA Method v1.7 は、以下のフェーズで構成される。

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

対象BJをスコープとして、実ケースの顧客ジャーニーと企業ジャーニーを対応づけ、実際に発生しているJudgement Point（JP）を抽出・精査する。

Phase1では、JJを設計しない。

主な成果物：

- 顧客ジャーニー
- 企業ジャーニー
- JP一覧
- JP精査記録
- 必要に応じて、JP統合・除外理由メモ

2つのジャーニーはJP抽出のための中間成果物として保存する。Phase2へ渡す主成果物はJP一覧であり、詳細な業務フローの完成は前提としない。

---

### Phase2 JULIA

Phase2では、JULIA（Judgement Scorecard）を用いて、Phase1で発見したJP（Judgement Point）を評価し、どの判断を優先的に設計・実装・学習するかを決定する。
JDAでは、発見したJP、設計対象とするJP、実装対象とするJPを同一視しない。
限られたリソースの中で、どの判断へ投資するかを決定するためにJULIAを用いる。

JULIAでは、以下の5軸で評価する。

- Judgement Financial Impact
- Urgency & Frequency
- Latency
- Influence Scope
- Adaptive / Learning Value

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

Phase5では、判断設計とログ設計から必要なタスク・実行フローを具体化し、現場運用に必要な最小構成を実装する。Judgement Injectionにより、JP定義とJudgement Harness（実行基盤）を分離する。

材料取得・整理と後続アクションは、execute_jpの前後のタスクとして扱う。execute_jpは判断の実行・状態遷移・JLog記録を担う。判断状態とタスク実行状況は分離し、「採用済み・登録未完了」のような状況を追跡する。

主な要素：

- Judgement Harness
- execute_jp
- State管理
- Proposal / Case
- JLog / VLog
- Operational Bridge

---

### Phase6 Learning

JLog / VLogをもとに、Data Sources・Conditions・Perspectivesを見直す。

Phase6では、Stage0の評価基盤と、Stage1〜3の学習・委譲段階を扱う。

```text
Learning Foundation
↓
Judgement Material Learning
↓
Judgement Reproduction Learning
↓
Judgement Delegation
```

---

## 実装の進め方

JDA v1.7では、実装の進め方として Judgement Slice Implementation を定義する。

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

JDAは、人・ルール・AI・Agent等のいずれが判断を担う場合でも、その判断を支援・実行し、継続的に検証するための構造を設計する。AIを使わない判断も設計対象に含む。

初期の実証では、AIが判断材料を収集・整理し、人が判断する形から始める。文脈を含む情報や例外の材料整理にもAIを活用できる。ただし、情報不足や責任範囲を超える状況では、保留や人への引き継ぎが必要になる。

判断の実行主体（Actor）と判断責任（Accountability）は分けて設計する。承認者・エスカレーション先も明確にし、妥当性の検証を踏まえてAIへの委譲範囲を決める。

LearningはAIモデルの再学習だけを意味しない。組織が判断材料・条件・観点を見直すことと、AIによる判断再現・委譲の検証の両方を扱う。

---

## 長期ビジョン

JDAの長期ビジョンは、Enterprise World Model（企業世界モデル）の構築である。

企業は、業務データと判断データを結びつけ、評価と見直しを重ねることで、自社固有の判断構造を学習していくことを目指す。その過程で、以下を組織の資産として育てる。

- 自社固有の判断履歴
- 自社固有の判断材料
- 自社固有の判断基準
- 自社固有の判断モデル

JDAは、そのための判断アーキテクチャである。

---

## リポジトリ構成

```text
.
├── README.md
├── LICENSE
├── core/
│   └── JDA_core_v1.7.md
├── method/
│   ├── current/
│   │   ├── 00_overview.md
│   │   ├── 01_phase0_foundation.md
│   │   ├── 02_phase1_discovery.md
│   │   ├── 03_phase2_julia.md
│   │   ├── 04_phase3_design.md
│   │   ├── 05_phase4_log.md
│   │   ├── 06_phase5_implementation.md
│   │   └── 07_phase6_learning.md
│   └── archive/
├── theory/
│   └── evolution/
├── analysis/
└── tools/
    └── jda_phase_template.html
```

主要なファイルのみ掲載。Judgement Slice ImplementationはPhase5に統合されている。

### 読み始める場所

- [Core v1.7](core/JDA_core_v1.7.md)：正式な概念定義
- [Methodの全体像](method/current/00_overview.md)：進め方の全体像
- [Phase3 Design](method/current/04_phase3_design.md)：判断の設計
- [Phase4 Log](method/current/05_phase4_log.md)：記録と評価の設計
- [Phase5 Implementation](method/current/06_phase5_implementation.md)：タスク・実行フローと最小実装
- [Phase6 Learning](method/current/07_phase6_learning.md)：評価から改善への接続
- [v1.8 Considerations](theory/evolution/JDA_v1.8_considerations.md)：未確定の検討事項
- [公式サイト](https://judgement-driven.com/)：JDAの紹介

---

## 現在の状況

Core / Methodの安定版はv1.7。今回の判断設計からタスク・実行フローへの接続は、v1.7の明文化として扱う。v1.8の検討事項とは分ける。

Phase6の実証記録（2026-08-19時点）では、広告営業で採用した候補について、判断記録と後続の営業状態を結びつけ、VLogを自動蓄積する仕組み（Stage0）を実装・本番反映している。見送った候補の追跡は、この実証範囲に含まれない。判断基準の改善と効果の検証は今後の課題であり、改善効果が実証済みであることは意味しない。

現在の主要概念

- Business Journey (BJ)
- Judgement Point (JP)
- Case / Proposal
- Judgement Journey (JJ)
- Judgement Slice
- Operational Bridge
- Judgement State Chart (JSC)
- Judgement Design Canvas (JDC)
- Judgement Scorecard (JULIA)
- Judgement Snapshot
- JLog
- VLog
- Judgement Harness
- Judgement Injection
- Learning Cycle (Stage0–3)

---

## ライセンス

Copyright (c) 2026 Shun Takeda（B-AS）

このプロジェクトは、クリエイティブ・コモンズ 表示 4.0 国際ライセンス（CC BY 4.0）のもとで公開しています。

次の利用が認められています。

- 共有：媒体や形式を問わず、資料を複製・再配布できます。
- 改変：営利目的を含め、資料を編集・加工し、新たな作品の基礎として利用できます。

利用にあたっては、次の条件に従ってください。

- 表示：原著者の適切なクレジットを表示してください。

ライセンスの全文：

<https://creativecommons.org/licenses/by/4.0/>

---

## 引用方法

研究・記事・発表などで本理論を利用する場合は、以下のように出典を記載してください。

Judgement-Driven Architecture（判断ドリブンアーキテクチャ）  
著者：Shun Takeda（B-AS）  
GitHubリポジトリ  
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
