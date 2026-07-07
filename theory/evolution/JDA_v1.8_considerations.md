# JDA v1.8 検討事項（Considerations）

本書は、JDA Method v1.7策定後に検討すべき事項を整理したものである。

ここに記載された内容は、現時点ではJDAの正式仕様ではない。

今後の実装・検証を通じて有効性が確認されたもののみ、将来のJDA CoreおよびJDA Methodへ取り込むことを前提とする。

---

# 1. JSC拡張

## 概要

現在のJSC（Judgement State Chart）は、
Targetを中心とした状態遷移を定義している。

今後は、状態遷移だけでは表現できない判断知識を
JSCへ取り込む可能性を検討する。

候補

- Perspectives
- Hypotheses
- 状態遷移理由
- 想定観測事項
- 状態前提

ただし、実装で有効性が確認されるまでは正式仕様としない。

ステータス

検討中

---

# 2. Threshold

## 概要

Thresholdはv1.7では正式概念から除外した。

しかし将来的に、

- AI判断
- Confidence
- 自動委譲

などを扱う際に必要になる可能性がある。

検討事項

- Conditionsで十分表現できるか
- Thresholdは独立概念か
- 実装詳細として扱うべきか
- Confidenceとの関係

ステータス

検討中

---

# 3. JP自動推薦

現在はJPを人が選択して実装する。

将来的には、

- JP推薦
- JPルーティング
- 実行可能JP探索

などをAIが支援する仕組みを検討する。

ステータス

研究段階

---

# 4. JSC自動進化

現在のState Chartは人が設計する。

将来的にはAIが

- 状態追加提案
- 遷移提案
- 欠落状態検出
- 行き止まり状態検出

などを行える可能性を検討する。

ステータス

研究段階

---

# 5. JDC自動進化

現在のJDCは人が設計する。

将来的にはAIが

- Data Sources提案
- Conditions提案
- Perspectives提案
- 判断理由提案

などを行える可能性を検討する。

ステータス

研究段階

---

# 6. Learning品質評価

Learning Cycle自体を評価する仕組みを検討する。

候補

- Learning成熟度
- JLog品質
- VLog品質
- Data Sources品質
- Delegation成熟度

ステータス

研究段階

---

# 7. Judgement Confidence

現在JDAは判断結果を記録する。

将来的には

- 人の確信度
- AIの確信度
- 確信度の変化

などを管理する可能性を検討する。

ステータス

検討中

---

# 8. Enterprise World Model

現在は判断を記録・学習することを目的としている。

将来的には

- 組織知識
- 業務知識
- 判断知識

を統合したEnterprise World Modelを構築する可能性がある。

ステータス

長期研究

---

# 9. BJ横断学習

現在LearningはJP・JJ単位が中心となっている。

今後は

- BJ横断パターン
- 共通JP
- 共通Data Sources
- 共通Perspectives

などを抽出する仕組みを検討する。

ステータス

研究段階

---

# 10. AIによる自己改善提案

Learning Cycleの結果をもとにAIが

- JSC改善
- JDC改善
- UI改善
- Operational Bridge改善

を提案する仕組みを検討する。

最終的な採用判断は人が行う。

ステータス

長期研究

---

# 11. Perspective / Hypothesis Framework

現在はPerspectiveをJDCで定義している。

今後、

- Perspective
- Hypothesis
- Observation
- Evidence

の関係を整理し、
JSCとの役割分担を再設計する可能性がある。

ステータス

検討中

---

# 変更履歴

| Version | 内容 |
|----------|------|
| v1.8-draft | JDA Method v1.7策定後の検討事項を整理 |