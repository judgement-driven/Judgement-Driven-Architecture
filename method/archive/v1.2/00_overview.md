# JDA Method v1.2 Overview

---

## 1. 概要

Judgement Driven Architecture（JDA）は、  
企業活動を「業務」ではなく「判断構造」として捉え、  
判断の抽出・構造化・委譲・学習を通じて  
組織の意思決定能力を向上させるための方法論である。

---

## 2. 基本思想

・企業活動はプロセスではなく判断の連鎖である  
・業務改善ではなく判断能力の強化を目的とする  
・AIは行動ではなく判断支援・判断代替に使う  
・判断ログが組織学習の基盤となる  

---

## 3. フェーズ構造

JDAは以下のフェーズで構成される。

### phase0 foundation

・業務と判断ドメインの整理  
・Business Journey（BJ）の抽出  

---

### phase1 discovery

・BJを判断視点で再解釈  
・Judgement Point（JP）の抽出  
・Judgement Journey（JJ）の把握  

---

### phase2 julia

・JPの評価  
・優先順位付け  
・投資判断  

---

### phase3 design

・判断構造の設計（JSC）  
・判断委譲の設計（JDC）  

---

### phase4 log

・判断ログ設計  
・学習ログ設計  

---

### phase5 implementation

・システム実装  
・運用設計  

---

### phase6 learning

・ログを用いた学習  
・判断精度の改善  
・循環の継続  

---

### 3.1 3層構造との対応関係

```text
phase0 foundation     → 導入準備（BJ使用）
--------------------------------
phase1 discovery      → Discovery Layer（BJをJJに再解釈）
phase2 julia          → Investment Layer（JJ・JP使用）
--------------------------------
phase3 design         → Learning Layer①（JJ・JP使用）
phase4 log            → Learning Layer②
phase5 implementation → Learning Layer③
phase6 learning       → Learning循環
```

---

### 3.2 フェーズ進行条件

各フェーズは以下の状態になったときに次へ進む。

phase0 → phase1
- Business Journeyが漏れなく列挙されている  
- 対象BJが1つ決まっている  

phase1 → phase2
- Judgement Pointが具体的に抽出されている（5〜10個以上）  
- JP一覧が作成されている  

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

## 4. 全体の流れ

```text
BMC
↓
Business Journey（BJ）
↓
（判断視点で再解釈）
↓
Judgement Journey（JJ）
↓
Judgement Point（JP）
↓
JULIA（評価）
↓
Design（JSC / JDC）
↓
Log
↓
Learning
```

---

## 5. 判断構造

JDAでは以下の概念を使い分ける。

### Business Journey（BJ）

・業務単位のまとまり  
・Phase0 Foundationで使用する  
・「何の業務か」を整理するための概念  
・例：BJ01 新規クライアント獲得  

---

### Judgement Journey（JJ）

・判断単位の連鎖  
・Phase1 Discovery以降で使用する  
・BJを判断視点で再解釈したもの  
・例：JJ01 アタリ判断の連鎖  

---

### Judgement Point（JP）

・最小単位の判断  
・「〜するか？」の形式  
・顧客状態 × 企業行動の交点で発生する  

---

### Judgement Chain

・BJを並べた結果として見える企業活動全体の判断の連鎖  
・明示的に設計するものではなく  
・BJとJJが揃った時点で自然に可視化される  

---

### 関係性

```text
BJ（業務単位）← Foundation
↓ 判断視点で再解釈
JJ（判断単位）← Discovery以降
└ JP（最小判断）

BJ01 → BJ02 → BJ03 → ...
↓
Judgement Chain（結果として見える）
```

---

## 6. 顧客と企業の二重構造

JDAでは、判断は単独ではなく  
顧客と企業の関係の中で発生すると捉える。

・顧客：状態（未認知、興味、検討、意思決定など）  
・企業：行動（接触、提案、調整、実行など）  

判断はこの2つの相互作用として現れる。

---

## 6.5 JP抽出の起点

JPは以下の交点で発生する。

顧客状態 × 企業行動 = JP

・顧客と企業が交わる列：交差判断（JP）  
・企業のみの列：企業内部判断（JP）  
・顧客のみの列：顧客内部状態（JPなし）  

---

## 7. JDAの本質

JDAは業務改善手法ではない。

判断を中心に据えることで、

・属人化の可視化  
・判断基準の外在化  
・AIによる判断支援  
・組織的な学習  

を実現するためのアーキテクチャである。

---

## 8. 進め方の原則

### 8.1 いきなり全体最適を狙わない
- 1つのJourneyから開始する  

### 8.2 作業ではなく判断を見る
- フローではなく判断を対象とする  

### 8.3 正解を求めない
- 判断を抽出することが目的  

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

JDAは意思決定そのものを扱うアーキテクチャである。

---

## 10. 次工程

本Overviewの次は

→ phase0 foundation（導入準備）

に進む。

その後、

→ phase1 discovery（判断抽出）

へ進む。

---
