# JDA Method v1.7 — Phase6 Learning

---

# 1. 本フェーズの目的

Phase6 Learning は、Phase5で蓄積された JLog / VLog をもとに、

- Data Sources
- 判断構造
- 判断基準
- 判断実行

を改善し、

> 判断を継続的に進化させる工程

である。

JDAにおけるLearningは、
単なるAIモデル学習ではない。

JDAのLearningとは、

> 組織とAIが、判断ログをもとに判断能力を改善していく循環構造

である。

---

# 2. 本フェーズの位置づけ

```text
Discovery（判断抽出）
↓
JULIA（Judgement Scorecard）（判断選定）
↓
Design（判断設計）
↓
Log（ログ設計）
↓
Implementation（実装）
↓
Learning（学習） ← 本フェーズ
```

---

本フェーズは、

> 判断を改善し続けるための循環工程

である。

---

# 3. 学習の基本構造

```text
判断
↓
JLog
↓
VLog
↓
分析
↓
改善
↓
次サイクル
```

JDAは、

```text
このLearning Cycleを回し続けるアーキテクチャ
```

である。

---

# 4. JDAにおけるLearning

## 4.1 JDAのLearning対象

JDAでは、
以下をLearning対象として扱う。

- Data Sources
- Conditions
- Perspectives
- 状態遷移
- 判断理由
- 判断パターン
- JP依存構造
- JJ実行構造

---

## 4.2 Learningの本質

JDAにおけるLearningの本質は、

```text
AIモデルを学習すること
```

ではない。

本質は、

```text
組織の判断構造を改善すること
```

である。

---

## 4.3 Learningの主体

Learningは：

- 人間
- 組織
- AI

の共同構造として行われる。

---

# 5. Learningの段階

JDAでは、
Learningを段階的に進める。

---

## 5.0 Stage0 — Learning Foundation

Learningを成立させるための前提として、
まずVLogを継続的に取得できる状態を構築する。

Stage0では判断モデルを改善しない。

目的は、妥当性評価（VLog）が自動的に取得できる基盤を作ることである。

```text
JP実行
↓
状態確定（JSCに従う）
↓
後続状態追跡
↓
VLog生成
```

Stage0が成立して初めて、Stage1以降のLearningが始まる。

---

## 5.1 Stage1 — Judgement Material Learning（判断材料学習）

現段階で最も重要なLearning。

AIは、

```text
判断そのもの
```

ではなく、

```text
Data Sources生成
```

を改善する。

---

### 対象

- 類似案件抽出
- 優先順位
- 顧客マッチ
- 類似度
- Data Sources整理
- 過去事例提示
- アタリ率分析

---

### 構造

```text
AI → Data Sources
Human → Decision
```

---

### 目的

- 判断精度向上
- 判断速度向上
- 判断負荷低減

---

## 5.2 Stage2 — Judgement Reproduction Learning（判断再現学習）

JLog / VLog が十分蓄積された場合、
AIは過去判断を再現し始める。

---

### 構造

```text
AI → Suggested Judgement
Human → Confirm
```

---

### 対象

- 過去類似Case
- 過去JP結果
- 状態遷移パターン
- Perspectives
- 判断理由

---

### 目的

- 判断再現
- 属人性低減
- 判断安定化

---

## 5.3 Stage3 — Judgement Delegation（判断委譲）

十分なLearning後、
判断は段階的にAIへ委譲される。

---

### 構造

```text
AI → Judgement
Human → Review
```

---

### 特徴

- 人間は監督者になる
- VLogが重要になる
- Accountability Judgement が必要になる

---

# 6. VLogによる妥当性評価

## 6.1 VLogの役割

VLogは、

> 判断が妥当だったか

を評価するためのログである。

---

## 6.2 妥当性評価の対象

JDAでは、
妥当性を以下の観点で評価する。

---

### ① 結果妥当性

```text
判断結果が望ましい成果につながったか
```

---

### ② プロセス妥当性

```text
Data Sources・Perspectives・理由が適切だったか
```

---

### ③ 修正妥当性

```text
人間がAI判断を修正した理由は妥当だったか
```

---

## 6.3 初期段階

初期段階では、
複雑な評価モデルを作らない。

まずは：

- 妥当
- 微妙
- 不妥当
- 未評価

程度でもよい。

---

## 6.4 妥当性評価の方法

妥当性評価の具体的方法は、
JPごとに異なる。

そのため、

```text
妥当性の考え方はMethodで定義し、
具体的評価方法は実装・運用で決定する
```

方針を採用する。

---

## 6.5 VLogの重要性

VLogは単なる評価ログではない。

特に：

```text
人間がAI判断を覆した理由
```

は重要なLearning対象となる。

これは：

- AI失敗パターン
- Data Sources不足
- Conditions不足
- Perspectives不足

を発見するための重要情報となる。

---

# 7. Learning対象

## 7.1 更新対象

Learningによって、
以下が更新対象となる。

- Conditions
- Perspectives
- Data Sources
- 判断理由
- transition_map
- decision_options
- UI
- AIプロンプト
- JP依存構造

---

## 7.2 JP定義更新

将来的には、

```text
JP定義そのもの
```

がLearning対象となる可能性がある。

ただし現時点では、
JP自体の自動更新は行わない。

---

## 7.3 非同期更新

Learningによる更新は、
非同期で行われる。

実行中Proposalには適用せず、

```text
次Proposal生成時
```

から適用する。

---

# 8. JJ（Judgement Journey）とLearning

## 8.1 JP単体学習

従来のLearningは、
JP単体改善が中心だった。

---

## 8.2 JJ学習

JDAでは、

```text
JP連鎖そのもの
```

もLearning対象となる。

---

## 8.3 JJで見るもの

- どのJP順序が成功しやすいか
- どこで保留が発生するか
- どのJPで離脱しやすいか
- どのData Sourcesが後続JPへ影響するか

---

## 8.4 Learning対象としてのJJ

JJは：

- 判断実行構造
- Proposal進化構造
- Learning構造

として扱われる。

---

# 9. 組織学習とAI学習

## 9.1 組織学習

組織学習とは：

- 判断基準共有
- 判断理由共有
- 属人性低減
- 判断構造改善

である。

---

## 9.2 AI学習

AI学習とは：

- JLog学習
- VLog評価
- 判断再現
- Data Sources生成改善

である。

---

## 9.3 関係性

```text
組織が判断構造を明確化
↓
AIがLearning
↓
組織へ還元
```

---

## 9.4 JDAの特徴

JDAにおけるLearningは、

```text
AI単体では成立しない
```

JLog・VLog・組織による妥当性評価を前提として、
Learning Cycleが成立する。

Learningの前提は、

```text
JLog / VLog
```

である。

---

# 10. Learningの進め方

## 10.1 一度に広げない

まず：

```text
1つのJPを深く改善
```

する。

---

## 10.2 小さく回す

- 小規模実装
- 小規模Learning
- 小規模改善

を高速に回す。

---

## 10.3 実装から学ぶ

最初から完全なLearning構造を作らない。

```text
実装
↓
ログ観測
↓
違和感発見
↓
必要なら概念化
```

で進化させる。

---

## 10.4 不要な複雑性を入れない

現時点で必要ない：

- JP自動更新
- 自律JP生成
- 完全自動評価

は先に導入しない。

---

# 11. 例（BJ01）

以下はBJ01（新規クライアント獲得）における実装例である。
対象BJによって具体的な内容は異なる。

---

## Stage1 — Judgement Material Learning

```text
AI → 類似企業提示
Human → 営業判断
```

---

## Stage2 — Judgement Reproduction Learning

```text
AI → アタリ予測
Human → 確認
```

---

## Stage3 — Judgement Delegation

```text
AI → 優先営業対象決定
Human → レビュー
```

---

## 11.1 実装状況（2026-08-19時点、JP01）

BJ01のJP01（ネタ候補採用判断）で、**Stage0 — Learning Foundationを実装・本番反映した**（実装セッション31）。

- 営業パイプライン側（`brands`/`company_history`、シェアリスト取り込みを含む）で企業の`customer_state`（アタリ／クライアント／NG）が更新されるたびに、JP01で「ネタ採用」済みの同一企業のVLog（後続状態・妥当性）を自動生成・更新する仕組みを追加（`JDA-Bunka-Ad-App-V2` commit `677d096`）
- これにより、JP01の運用（シェアリストの通常アップロード）を続けるだけで、追加の手入力なしにVLogが継続的に蓄積される状態になった
- Stage1（Judgement Material Learning：VLog傾向をAIプロンプトへ反映する等）はまだ未着手。10.3節の方針（実装→ログ観測→違和感発見→必要なら概念化）に従い、まずVLogがある程度蓄積されるのを待ってから着手する予定
- 詳細な実装経緯はProjectの `claude/bj01_jda-bunka-ad-app_handoff.md`（実装セッション31の節）を参照

---

# 12. 成功条件

- JLogが蓄積されている
- VLogが評価されている
- Data Sources精度が改善している
- 判断速度が改善している
- 判断再現が可能になっている
- Learning Cycleが継続している
- JJ上の改善点が観測できる

---

# 13. よくある失敗

## 13.1 ログだけ蓄積する

❌ 保存のみ  
⭕ 改善へ接続

---

## 13.2 いきなりAIへ委譲する

❌ 即自動化  
⭕ 段階的Learning

---

## 13.3 完璧な評価モデルを最初に作る

❌ 過剰設計  
⭕ 軽量VLogから開始

---

## 13.4 Learning前に複雑化する

❌ 自律構造先行  
⭕ 実装 → 観測 → 改善

---

# 14. 次工程

Learning結果をもとに：

```text
Discovery
↓
JULIA
↓
Design
```

へ戻る。

JDAは、

```text
循環する判断アーキテクチャ
```

である。

---

# 15. 変更履歴

| Version | 内容 |
|---|---|
| v1.3 | 初版 |
| v1.4 | Condition / Perspective / Decision Criteria 更新追加 |
| v1.5 | Learning Loop強化 / Delegation概念追加 |
| v1.6 | Judgement Material Learning追加 / JJ学習追加 / VLog妥当性評価拡張 / 段階的Learning構造へ再編 |
| v1.7 | Stage0をLearning Foundationへ変更 / Learning Cycle名称をCore v1.7（Judgement Material Learning・Judgement Reproduction Learning・Judgement Delegation）へ統一 / Thresholdを正式概念から除外 / 判断材料・Condition・PerspectiveをData Sources・Conditions・Perspectivesへ用語統一 / 「AI単体ではLearningできない」の表現を「JLog・VLog・組織による妥当性評価を前提としてLearning Cycleが成立する」へ修正 / Core v1.7・README v1.7・Phase3〜Phase5との整合 |
| （2026-08-19追記） | 11.1節「実装状況」追加：JP01でStage0（VLog自動蓄積）を実装・本番反映（バージョン番号自体は据え置き、実装セッション31の実証ログとして追記） |
