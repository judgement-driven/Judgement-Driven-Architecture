# JDA Method v1.7 — Phase3 Design

---

# 1. 本フェーズの目的

Phase3 Design は、Phase2 JULIAで選定された Judgement Point（JP）に対して、  
**判断の構造・状態遷移・判断材料・責任・実行単位を設計する工程**である。

Phase3では、JPを単なる業務上の分岐として扱わない。

JPは、

```text
状態を確定させる判断
```

であり、Phase5 Implementation で execute_jp によって実行され、  
JLog / VLog に接続される構造として設計する。

本フェーズの目的は、

```text
判断を実装可能・記録可能・学習可能な構造に変換すること
```

である。

---

# 2. 本フェーズの位置づけ

```text
Discovery（BJをスコープとしてJP抽出）
↓
JULIA（JP評価・投資判断）
↓
Design（判断設計） ← 本フェーズ
↓
Log（ログ設計）
↓
Implementation（実装）
↓
Learning（学習）
```

---

> 本フェーズは  
> **判断を実装可能な構造に変換する工程**である

---

# 3. 成果物

Phase3 Design の成果物は以下である。

- Judgement State Chart（JSC）
- Judgement Design Canvas（JDC）
- Case / Proposal 定義
- 判断材料定義
- 判断責任定義
- Phase4 Log への引き渡しメモ
- Phase5 Implementation への引き渡しメモ

---

# 4. 設計の基本思想

JDAでは、判断を単なる分岐ではなく、

> 状態遷移として扱う

---

## 4.1 従来の考え方

```text
if 条件A → 処理X
else → 処理Y
```

---

## 4.2 JDAの考え方

```text
状態A
↓
JPによる判断
↓
状態B
```

判断は、

```text
状態を確定する行為
```

である。

---

## 4.3 v1.7における設計観点

v1.7では、判断設計を以下に接続する。

- Proposal / Case
- State
- execute_jp
- JLog
- VLog
- Judgement Slice Implementation
- Learning Cycle

そのためPhase3では、JSC / JDC を作るだけでなく、

```text
この判断をどう実行し、
何を記録し、
何を学習可能にするか
```

を意識して設計する。

---

# 5. 実行単位の定義

## 5.1 Case

Caseとは、判断対象単位である。

```text
Case = Entity × Context
```

Caseは、JPに入力される判断対象の基本単位であり、  
JSC / JDC / JLog / VLog の起点となる。

---

## 5.2 Proposal

Proposalとは、Caseを実行系に具体化したインスタンスである。

Phase5 Implementation では、Proposalが以下の中心単位となる。

- State管理
- JP実行
- JLog記録
- VLog評価
- Learning対象

---

## 5.3 CaseとProposalの関係

```text
Case
↓
Proposal
↓
State
↓
JP
↓
JLog / VLog
```

Core理論上は Case と表現するが、  
実装上は Proposal として扱われる場合がある。

例：BJ01 新規クライアント獲得

```text
Entity = Company
Context = Campaign
Proposal = Company × Campaign
```

---

## 5.4 設計時に定義すること

Phase3では、対象JPごとに以下を定義する。

- 判断対象は何か
- Case / Proposal の単位は何か
- Stateはどこに紐づくか
- 同一Entityに複数Contextがあり得るか
- 他Proposalを参照する必要があるか

---

# 6. ステップ1：Judgement State Chart（JSC）

## 6.1 目的

JSC（Judgement State Chart）は、  
判断対象（Target）が取り得る状態と、Judgement Point（JP）による状態遷移を定義する  
State Chartである。

JSCでは、

```text
現在状態
↓
JP
↓
判断結果
↓
遷移先状態
```

を明確にする。

---

## 6.2 JSCの構成

最低限、以下を定義する。

- 対象JP
- 管理単位（Case / Proposal）
- 現在状態
- 判断結果
- 遷移先状態
- 保留・例外状態
- 再判断ループ
- 後続JPへの接続

---

## 6.3 JSCで書くべきこと

JSCでは、処理手順ではなく状態を書く。

```text
未評価
↓
JPによる判断
↓
対象 / 対象外 / 保留
```

のように、判断前後の状態を明確にする。

---

## 6.4 例（入金消込）

```text
未処理
↓（入金紐付け判断）
保留
↓（追加確認）
確定
↓（差異確認）
差異対応
↓
消込完了
```

---

## 6.5 ポイント

- 状態で考える
- フローで書かない
- 分岐ではなく遷移で書く
- 中間状態を明示する
- 保留・例外を状態として扱う
- 後続JPへの接続を意識する
- execute_jpで扱える粒度にする

---

# 7. ステップ2：Judgement Design Canvas（JDC）

## 7.1 目的

JDCは、判断の中身を設計するための成果物である。

JSCが、

```text
判断前後の状態
```

を扱うのに対して、JDCは、

```text
その判断を何を材料に、誰が、どの観点で、どの責任で行うか
```

を整理する。

---

## 7.2 定義

Judgement Design Canvas（JDC）とは、

> 判断を構成する要素を整理し、  
> 判断材料提示・判断入力・状態遷移・JLog / VLog に接続するための設計フレーム

である。

---

## 7.3 v1.7における構造変更

v1.6までのJDCは、Proceed（進行） / Validity（妥当性） / Accountability（責任） / Venture（探索性）の4軸で構成されていた。

v1.7では、この4軸をJDCの正式構造としては採用しない。

概念そのものを捨てたわけではなく、各設計観点は、以下のJDC構成要素へ整理された。

```text
Purpose / Subject / Data Sources / Conditions / Perspectives /
Decision / Actor / Accountability / Output（9要素） ← v1.7で正式採用
```

旧4軸の内容は、以下のように9要素へ整理されている。

```text
Proceed（判断結果・遷移先状態・次アクション・後続JP）
→ Decision / Output に整理

Validity（判断材料・データソース・判断条件・判断観点・判断理由・妥当性評価の手がかり）
→ Data Sources / Conditions / Perspectives に整理（判断理由・妥当性評価はJLog / VLogに接続）

Accountability（判断主体・判断責任者・承認者・レビュー者・エスカレーション先・AI提案時の最終責任）
→ Actor / Accountability に整理

Venture（定型・半定型・非定型、探索性の判定）
→ v1.7ではJDCの正式構造としては採用しない。履歴として残し、必要に応じてv1.8以降で再検討する
```

---

# 8. JDCの構成（9要素）

JDCは以下の9つの要素で構成される。

- Purpose
- Subject
- Data Sources
- Conditions
- Perspectives
- Decision
- Actor
- Accountability
- Output

---

# 9. JDC詳細項目

以下はJDCを実際に記入するための項目一覧である。

---

## 9.1 Purpose

このJPが何のために存在するか、対象JPが何を決める判断なのかを定義する。

例：

```text
この企業を対象とするか？
この案件を優先するか？
この入金はどの請求か？
```

---

## 9.2 Subject

判断対象を定義する。

例：

- Company
- Campaign
- Proposal
- Case
- 入金データ
- 問い合わせ
- 顧客

重要なのは、  
判断がどの単位に対して行われるかを明確にすることである。

---

## 9.3 Data Sources

判断に使用する情報を定義する。

例：

- 基本情報
- 履歴情報
- 入力データ
- AI補強情報
- 類似Case
- 過去判断
- 現場メモ

---

## 9.4 Conditions

客観的に判定可能な条件を定義する。

例：

```text
金額が一致しているか
媒体読者層と一致しているか
過去に拒否履歴がないか
```

条件は、ルール化しやすい判断材料である。

---

## 9.5 Perspectives

経験・文脈・暗黙知に依存する観点を定義する。

例：

```text
担当者の温度感
今提案する意味があるか
過去類似案件との感覚的近さ
顧客側の負荷感
```

観点は、すぐにはルール化できないが、  
JLog / VLogを通じて将来的に学習対象となる。

---

## 9.6 Decision

判断結果として取り得る選択肢を定義する。

例：

- 対象
- 対象外
- 保留
- 高優先
- 中優先
- 低優先
- 接触可
- 接触不可

判断結果は、JSCの遷移先状態に接続する。

---

## 9.7 Actor

誰が判断を行うかを定義する。

例：

- 営業担当
- 経理担当
- 制作担当
- マネージャー
- AI
- システム

現段階では、多くの場合、

```text
AIが判断する
```

ではなく、

```text
AIが判断材料を提示し、人間が判断する
```

と捉える。

### 判断確定の形態

判断がどのように確定されるかは、Actorの運用形態として捉える。

例：

- 人が最終判断
- AI提案 + 人確認
- ルール判定 + 人確認
- 自動確定
- エスカレーション

初期段階では、

```text
AI提案 + 人判断
```

を基本とし、  
JLog / VLog が蓄積された後に判断再現・判断委譲を検討する。

---

## 9.8 Accountability

判断の責任を誰が持つかを定義する。

判断主体と判断責任者は一致する場合もあるが、必ずしも同じではない。

例：

```text
判断主体：営業担当
判断責任：営業担当者

判断主体：AI提案 + 人間確認
判断責任：人間の確認者

判断主体：担当者
判断責任：営業責任者
```

AIが判断材料を提示する場合でも、  
判断責任は人間または組織に残る。

AI提案時の最終責任も、必ずここで明記する。

判断責任に関わる役割として、以下も対象に含める。

- 承認者
- レビュー者
- エスカレーション先

---

## 9.9 Output

判断の出力を定義する。

例：

- 次アクション
- 優先度
- 接触可否
- campaign候補
- 注意喚起
- 保留理由
- 遷移先状態
- 後続JPへの入力情報

出力は、後続JP・UI・JLog・VLogに接続する。

---

# 10. 例外処理の設計

## 10.1 目的

通常判断だけでなく、例外判断を構造として定義する。

例外を処理の外に出すのではなく、  
状態として扱う。

---

## 10.2 例（入金消込）

例外ケース：

- 金額が一致しない
- 名義が不明
- 複数候補が存在

---

## 10.3 JSCでの表現

```text
未処理
↓（入金紐付け判断）
保留（例外）
↓（追加確認）
確定
↓（差異確認）
差異対応
↓
消込完了
```

---

## 10.4 JDCでの設計例（例外）

- Data Sources：入金情報・請求情報
- Conditions：金額不一致
- Perspectives：名義類似・過去履歴
- Decision：保留
- Actor：経理
- Accountability：経理担当者
- Output：保留理由、次確認対象

---

## 10.5 ポイント

- 例外を状態として持つ
- 例外を潰さない
- 保留を状態として扱う
- 再判断の流れを設計する
- 例外理由をJLogに残せるようにする
- 後からVLogで評価できるようにする

---

# 11. Phase4 Logへの接続

Phase3で設計した内容は、Phase4 LogでJLog / VLogとして記録設計される。

---

## 11.1 JLogに接続する項目

JDCで定義した判断材料・条件・観点・結果・主体・責任は、  
そのままJLogの記録項目として使用する。

これに加えて、以下をJLogに含める。

- before_state
- after_state

Phase4 Logでは、これらに加えて、  
判断時点で利用可能であった情報全体を Judgement Snapshot として記録する。

```text
Exploration Context
↓
Candidate Snapshot
↓
Decision Context
↓
Judgement
```

---

## 11.2 VLogに接続する項目

JDC / JSCの以下は、VLogの評価項目に接続される。

- 判断結果
- 判断理由
- 判断材料
- 期待された結果
- 実際の結果
- 差異
- 妥当性評価
- 修正理由

---

## 11.3 設計時の注意

Phase3の時点で、すべてのログ項目を確定する必要はない。

ただし、

```text
後から判断理由を再現できるか
後から妥当性を評価できるか
```

を意識して設計する。

---

# 12. Phase5 Implementationへの接続

Phase3で設計したJSC / JDCは、Phase5で実装される。

---

## 12.1 execute_jpへの接続

Phase5では、JPは execute_jp によって実行される。

Phase3では、execute_jpで扱えるように、以下を明確にする。

- 対象JP
- before_state
- decision_options
- transition_map
- after_state
- actor
- data_sources
- reason

---

## 12.2 Judgement Slice Implementationへの接続

JULIAで選定された主要JPは、  
Judgement Slice Implementation の起点となる。

Phase3では、そのJPを運用可能にするために必要な以下を設計する。

- 判断材料提示
- 判断入力
- 判断理由入力
- 状態遷移
- JLog収集
- VLog評価準備
- Operational Bridgeとの接続

---

## 12.3 AIの位置づけ

Phase3では、AIをいきなり判断主体として設計しない。

初期段階では、AIは主に以下を担当する。

- 判断材料の収集
- 判断材料の整理
- 類似Caseの提示
- 過去履歴の要約
- 判断理由入力の補助

AIによる判断再現・判断委譲は、  
Phase6 LearningでJLog / VLogが蓄積された後に検討する。

---

# 13. 設計のポイント

## 13.1 条件と観点を分ける

```text
条件：ルール化できる
観点：経験・文脈
```

この分離により、  
判断材料学習・判断再現学習の対象が明確になる。

---

## 13.2 すべて自動化しない

最初は人判断でよい。

重要なのは、自動化することではなく、

```text
判断材料
判断理由
判断結果
状態遷移
```

を記録可能にすることである。

---

## 13.3 判断主体と判断責任を分ける

```text
誰が判断するか
誰が責任を持つか
```

を必ず定義する。

AIが判断材料を提示する場合でも、  
責任は人間または組織側に残る。

---

## 13.4 保留を設計する

判断できない状態を例外として消さない。

```text
保留
再確認
追加情報待ち
エスカレーション
```

を状態として設計する。

---

## 13.5 後から評価できるように設計する

Phase3の時点で、以下を意識する。

- 判断理由を残せるか
- 判断材料を残せるか
- 期待結果を定義できるか
- 後から妥当性評価できるか
- VLogを書けるか

---

# 14. 例（入金紐付け）

---

## 14.1 JSC

```text
未処理
↓（入金紐付け判断）
保留
↓（追加確認）
確定
↓（差異確認）
差異対応
↓
消込完了
```

---

## 14.2 JDC

| 項目 | 内容 |
|---|---|
| Purpose | この入金はどの請求か？ |
| Subject | 入金データ × 請求情報 |
| Data Sources | 入金金額、振込名義、請求一覧 |
| Conditions | 金額一致、請求日、請求額 |
| Perspectives | 名義揺れ、過去履歴、顧客特性 |
| Decision | A請求 / B請求 / 保留 |
| Actor | 経理担当 |
| Accountability | 経理担当者 |
| Output | 紐付け結果、保留理由、次確認対象 |

---

# 15. 成功条件

- 判断が構造として定義されている
- Case / Proposal単位が明確になっている
- 状態遷移が明確になっている
- 判断材料が定義されている
- 判断条件と判断観点が分けられている
- 判断主体と判断責任が定義されている
- JLog / VLog に接続できる
- execute_jp で扱える粒度になっている
- 実装可能なレベルになっている

---

# 16. よくある失敗

## 16.1 フローに戻る

❌ 手順を書く  
⭕ 状態を書く

---

## 16.2 条件だけ書く

❌ ルールのみ  
⭕ 観点も書く

---

## 16.3 主体を書かない

❌ 誰が判断するか不明  
⭕ 判断主体を定義する

---

## 16.4 責任を書かない

❌ AIが出したから責任不明  
⭕ 判断責任者を定義する

---

## 16.5 例外を消す

❌ 例外処理として外に出す  
⭕ 保留・例外を状態として設計する

---

## 16.6 ログに接続できない

❌ 設計だけで終わる  
⭕ JLog / VLogに接続できる粒度で設計する

---

## 16.7 実装を意識しすぎてUI設計になる

❌ 画面項目を作る  
⭕ 判断材料・判断入力・状態遷移を設計する

---

## 16.8 旧4軸で考えてしまう

❌ Proceed / Validity / Accountability / Ventureで整理しようとする  
⭕ Purpose / Subject / Data Sources / Conditions / Perspectives / Decision / Actor / Accountability / Outputの9要素で整理する

---

# 17. 次工程

→ Phase4 Log（ログ設計）

Phase4では、Phase3で設計したJSC / JDCをもとに、  
JLog / VLog の記録項目・記録タイミング・評価方法を設計する。

---

# 18. 変更履歴

| Version | 内容 |
|---|---|
| v1.3 | 初版 / JSC・JDC・Case定義を追加 / 判断を状態遷移として扱う設計を定義 |
| v1.6 | Case / Proposal の関係を明確化 / JDCをJudgement Design Canvasとして再定義 / 判断主体と判断責任を分離 / execute_jp・JLog・VLog・Judgement Slice Implementationへの接続を追加 / AIを判断材料支援から始める方針を明記 |
| v1.7 | JDCをフラット構造へ変更 / Proceed・Validity・Accountability・Venture構造をJDCの正式構造としては不採用とし、旧内容をDecision / Output / Data Sources / Conditions / Perspectives / Actor / Accountabilityへ整理 / JDCをPurpose・Subject・Data Sources・Conditions・Perspectives・Decision・Actor・Accountability・Outputの9要素へ再定義 / 「判断確定方式」を正式項目から除外し、Actorの運用形態として整理 / JSC定義をCore v1.7（判断対象＝TargetとState Chart表現）に統一 / Phase4 LogのJudgement Snapshotとの接続を明記 / Core v1.7・README v1.7との整合 |
