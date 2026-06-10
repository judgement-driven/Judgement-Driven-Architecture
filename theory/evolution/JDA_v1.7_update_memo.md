# JDA v1.7 更新候補メモ

作成日：2026-06-10

---

## 1. JSC定義の更新

### 現行（v1.6）
> JPによって引き起こされる状態遷移モデル

### 更新候補
> 判断対象（Target）が取り得る状態空間を定義するモデル。
> JPはTargetの次状態を確定する判断であり、JSCはその状態遷移の設計図である。

---

## 2. JPの再定義

### 現行（v1.6）
> 状態を確定させる最小判断単位

### 更新候補
> TargetのStateを確定させる判断。
> JP = State Transition Decision
> JP = 状態確定装置

---

## 3. JSCとJDCの役割分担の明確化

### 更新候補

```
JSC = State Space
　→ Targetが取り得る状態を定義する

JDC = Transition Logic
　→ どの条件でどの状態へ遷移させるかを定義する
```

### 実務上の関係
JSCとJDCは並行して作る。順序は固定ではなく、役割が違う。

```
JDCを書いて判断対象が見える
↓
JSCを書いて状態が見える
↓
JSCを書いていたらJDCの判断結果が足りないと気づく
↓
JDCを直す
```

---

## 4. Judgement-Driven State Machineの概念追加

### 背景
JSCは通常のステートマシンに近い構造を持つ。
ただしJDAでは、状態遷移が「条件」ではなく「判断（JP）」によって決まる。
その判断ロジックはJDCにある。

### 定義候補
```
普通のステートマシン
= 条件によって状態が遷移する

Judgement-Driven State Machine
= 判断（JP）によって状態が遷移する
= その判断ロジックはJDCにある
```

---

## 5. 核心一文

```
状態が判断を生み、判断が次の状態を確定する。
```

---

## 6. Stateの主語の明確化

```
StateはTarget（Proposal）が持つ
JPはTargetの次状態を確定する判断である
JSCはTargetの状態遷移を設計するモデル
JDCはJPの設計書
```

---

## 備考

- 第6弾note執筆前の壁打ちで発見
- Case/Proposalの言語化はv1.7またはPhase10実装時に整理予定
- 「JPは状態生成装置」は不採用。「状態確定装置」が正確。

---

## 7. 既存メモとの関係

以前のメモ「JDA v1.7 Considering — JSC拡張（遷移理由・Perspective・Hypothesis）」で、

> JSCとJDCの責務分離が曖昧なままv1.7に入ると混乱する

という論点が未解決のまま残っていた。

今日の議論で以下の整理が出た。

```
JSC = State Space（Targetが取り得る状態の定義）
JDC = Transition Logic（どの条件でどの状態へ遷移させるかの定義）
```

これにより、上記論点は一定の解決を見た。

ただし、Perspective / Hypothesis / 遷移理由をどちらに持たせるかは継続検討。
現時点の方向性：

```
遷移理由 → JDCのTransition Logicとして定義
Perspective / Hypothesis → JDCの観点・仮説として定義
JSCは状態空間（State Space）に留める
```

---

## 8. JULIA評価軸の更新候補

### 背景
GoogleのAIモードから提案された評価軸が実用的だったため、更新候補として記録。

### 現行評価軸
```
ROI
Business Impact
Automation Potential
Learning Value
```

### 更新候補
```
J = Judgement Financial Impact（判断の金銭的・事業的影響）
U = Urgency & Frequency（緊急度・頻度）
L = Latency（停滞度：判断に詰まるか）
I = Influence Scope（影響範囲）
A = Adaptive / Learning Value（学習価値）
```

### Jの定義補足
元々はJudgement Financial Impactとして設計された軸。
「financial / business impact」の両方を含む。
投資優先度評価の中心軸として位置づける。

### Latencyの定義
「判断に詰まる度合い」＝以下の状態で発生する停滞。

```
判断が属人的で再現できない
判断材料が揃わない
判断基準が曖昧
判断責任が不明確
```

ROIや重要度とは独立した軸として、今すぐ構造化すべき判断を特定するための指標になる。

### 備考
- 略称JULIAの元の意味（Judgement Log Impact Assessment）は変わる可能性あり
- 各軸の実用的なわかりやすさを優先する方針

