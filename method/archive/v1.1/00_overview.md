# JDA Method v1.1 — Overview

## 1. 本ドキュメントの目的

本ドキュメントは、Judgement-Driven Architecture（JDA）を  
**実務で適用するための手順（Method）** を定義するものである。

JDAは理論（core）だけでは価値を持たない。  
本Methodは、

- 業務に適用し
- 判断を抽出し
- ログとして蓄積し
- 学習につなげる

ための具体的な進め方を示す。

---

## 2. coreとmethodの関係

JDAは以下の2層で構成される。

| 層 | 内容 |
|----|------|
| core | 概念・構造（What） |
| method | 実務手順（How） |

---

### 2.1 core（理論）

JDA coreでは、企業活動を以下の3層構造で定義する。

```text
Discovery / Investment / Learning
```

---

### 2.2 method（実務）

JDA Methodは、この3層構造を  
**実務で実行するためのプロセス**である。

---

> JDA Methodは「業務改善手法」ではなく  
> **判断を学習可能にするための実装手順**である

---

## 3. Methodの全体構造

JDA Methodは7つのフェーズで構成される。

```text
phase0 foundation
phase1 discovery
phase2 julia
phase3 design
phase4 log
phase5 implementation
phase6 learning
```

---

### 3.1 3層構造との対応関係

```text
phase0 foundation     → 導入準備
--------------------------------
phase1 discovery      → Discovery Layer
phase2 julia          → Investment Layer
--------------------------------
phase3 design         → Learning Layer①
phase4 log            → Learning Layer②
phase5 implementation → Learning Layer③
phase6 learning       → Learning循環
```

---

### 3.2 フェーズ進行条件

各フェーズは以下の状態になったときに次へ進む。

---

phase0 → phase1  
- Business Journeyが漏れなく列挙されている

phase1 → phase2  
- Judgement Pointが具体的に抽出されている（5〜10個以上）

phase2 → phase3  
- 優先判断が1つ決定されている

phase3 → phase4  
- 判断構造（JSC / JDC）が定義されている

phase4 → phase5  
- ログ設計が運用可能な粒度で定義されている

phase5 → phase6  
- 実装が稼働している

---

### 3.3 フェーズの目安期間

初回適用時の目安は以下である。

- phase0：1〜2日
- phase1：1〜2日
- phase2：半日
- phase3：1〜2日
- phase4：1日

※小さく始めることを前提とする

---

## 4. 各フェーズの概要

---

### phase0 foundation（導入準備）

- 判断ドメインの定義
- 業務ジャーニーの列挙

---

### phase1 discovery（判断抽出）

- 判断構造の可視化
- Judgement Point抽出

---

### phase2 julia（判断評価）

- 判断の価値評価
- 優先順位決定

---

### phase3 design（判断設計）

- JSC / JDC設計

---

### phase4 log（ログ設計）

- JLog / VLog設計

---

### phase5 implementation（実装）

- システム化・AI接続

---

### phase6 learning（学習）

- 評価・改善・再設計

---

## 5. 判断の階層構造

JDAでは、企業活動を以下の階層で捉える。

```text
Judgement Chain
└ Judgement Journey
   └ Judgement Point
```

---

## 6. 顧客と企業の二重構造

JDAでは、判断を以下の2つのChainで捉える。

```text
顧客の判断Chain
企業の判断Chain
```

価値は、

> 顧客の判断と企業の判断が重なる点

において発生する。

---

※本視点は、JDA coreで定義された判断構造を  
実務に適用するために導入された補助概念である

---

## 7. 適用対象と前提条件

### 7.1 対象読者

- 業務改善担当者
- AI導入担当者
- マネージャー

---

### 7.2 前提条件

- ToBeを先に描かない
- 小さく始める
- 判断単位で扱う

---

## 8. 進め方の原則

---

### 8.1 いきなり全体最適を狙わない

- 1つのJourneyから開始する

---

### 8.2 作業ではなく判断を見る

- フローではなく判断を対象とする

---

### 8.3 正解を求めない

- 判断を抽出することが目的

---

### 8.4 作りながら学習する

- 小さく作り、早く検証する
- 完成してから使わない
- HVE（Hypervelocity Engineering：高速に試作と検証を繰り返す開発スタイル）を前提とする

---

## 9. 本Methodの位置づけ

| 分野 | 対象 |
|------|------|
| BPM | 業務フロー |
| BI | データ |
| AI | モデル |
| JDA | 判断 |

---

JDAは

> **意思決定そのものを扱うアーキテクチャ**

である。

---

## 10. 次工程

本Overviewの次は

→ phase0 foundation（導入準備）

に進む。

その後、

→ phase1 discovery（判断抽出）

へ進む。
