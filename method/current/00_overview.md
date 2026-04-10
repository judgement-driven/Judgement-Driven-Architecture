# JDA Method v1.3 Overview

---

## 1. 概要

Judgement Driven Architecture（JDA）は、  
企業活動を「業務」ではなく「判断構造」として捉え、  
判断の抽出・構造化・委譲・学習を通じて  
組織の意思決定能力を向上させるための方法論である。

---

## 2. 基本思想

・企業活動はプロセスではなく、Caseの状態遷移として構造化される  
・業務改善ではなく判断能力の強化を目的とする  
・AIは行動ではなく判断支援・判断代替に使う  
・判断ログが組織学習の基盤となる  

---

## 3. フェーズ構造

JDAは以下のフェーズで構成される。

### phase0 foundation
・業務と判断ドメインの整理  
・Business Journey（BJ）の抽出  

### phase1 discovery
・BJを判断視点で再解釈  
・Judgement Point（JP）の抽出  
・Judgement Journey（JJ）の把握  

### phase2 julia
・JPの評価  
・優先順位付け  
・投資判断  

### phase3 design
・判断構造の設計（JSC）  
・判断委譲の設計（JDC）  

### phase4 log
・判断ログ設計  
・学習ログ設計  

### phase5 implementation
・システム実装  
・運用設計  

### phase6 learning
・ログを用いた学習  
・判断精度の改善  
・循環の継続  

---

## 4. 全体の流れ

BMC  
↓  
Business Journey（BJ）  
↓  
Judgement Journey（JJ）  
↓  
Judgement Point（JP）  
↓  
JULIA  
↓  
Design  
↓  
Log  
↓  
Learning  

---

## 5. 判断構造

### Business Journey（BJ）
業務単位のまとまり  

### Judgement Journey（JJ）
判断単位の連鎖  

### Judgement Point（JP）
最小単位の判断  
「〜するか？」の形式  
顧客状態 × 企業行動の交点で発生する  
状態遷移を内包した判断構造である

---

## 6. 顧客と企業の二重構造

顧客状態 × 企業行動によって判断が発生する  

### Case（実行単位）

CaseはBJ内の判断対象を実行単位として具体化したものである。  
実装段階で登場する。  
詳細はCore v1.3を参照。  

---

## 7. JDAの本質

JDAは判断を構造化し、学習可能にするアーキテクチャである  

判断ログはJPの構造（状態遷移）を更新する学習資源となる  

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

---

## 9. 本Methodの位置づけ

| 分野 | 対象 |
|------|------|
| BPM | 業務フロー |
| BI | データ |
| AI | モデル |
| JDA | 判断 |

---

## 10. 変更履歴

v1.3: Case導入 / JP再定義 / Learning強化
