# JDA Core v1.3

TYPE: Core Definition  
PROJECT: JDA Method  
VERSION: v1.3  
DATE: 2026-04-10  
PURPOSE: JDAの構造を「状態遷移と学習構造」として再定義する

---

## 1. 概要

JDAは、組織の意思決定を構造化・記録・学習可能にするためのアーキテクチャである。

### 再定義

Before（v1.2）  
JDA = 判断の連鎖

After（v1.3）  
JDA = Caseの状態遷移を通じて、意思決定を構造化・記録・学習可能にするアーキテクチャ

---

## 2. 基本構造

Case → JP → Judgement → State Change → Case

※ 状態遷移はグラフ構造をとりうる。分岐・並列・収束を含む。

---

## 3. 概念定義

### 3.1 Case

判断対象を一意に特定する単位

Case = Entity × Context

#### 同一性条件

- Entity
- Context
- 判断前提

これらが変化し、判断前提が変わる場合は別Caseとする

#### ライフサイクル

- 開始：初回状態生成
- 進行：状態遷移
- 終了：最終状態
- 再開：再遷移または新Case生成

---

### 3.2 State

Caseに紐づく状態（履歴管理）

State:

- case_id
- state_name
- entered_at
- entered_by_jp
- entered_by_jlog_id

---

### 3.3 Judgement

状態を確定させる行為

- 条件・観点・理由を持つ
- JLogとして記録される
- 状態遷移を引き起こす

---

### 3.4 Judgement Point（JP）

状態遷移を定義する単位

JP:

- trigger_state
- decision_options
- transition_map

---

## 4. Interface

### 4.1 JP Interface

Input:

- state_before
- data_inputs

Output:

- state_after
- decision
- JLog

---

### 4.2 Judgement Interface

Input:

- state_before
- data_inputs
- condition_data
- perspective_data

Process:

- option_generation
- evaluation
- comparison

Output:

- decision
- reason
- state_after
- jlog_id
- status（success / pending / rejected）

---

### 4.3 関係

JP InterfaceはJudgement Interfaceを内包する

---

## 5. 実行モデル

### 5.1 状態依存実行

判断はstate_beforeに依存して実行される

---

### 5.2 並列実行

JDAは楽観ロックモデルを採用する

- state_versionを参照
- 不一致時は再判断

---

### 5.3 分散前提

- Stateは整合性管理対象
- JLogは順序保証が必要
- Learning更新は非同期

Learning更新は実行中のCaseには適用しない  
更新は次のCase生成時から有効とする

---

## 6. Learning

JDAは学習構造を内包する

JDA = State Machine + JLog + Learning Loop

---

### 6.1 Learning Loop

JLogとVLogの差分をもとに以下を更新する

- Condition
- Perspective
- Decision Criteria

---

### 6.2 遷移関数の学習

FSM：

- transition_mapは固定

JDA：

- transition_mapは更新される

遷移関数自体が学習対象である

---

### 6.3 更新対象

JPの以下が更新されうる

- trigger_state
- decision_options
- transition_map

---

## 7. JSC（Judgement State Control）

JSCは状態管理および遷移制御の補助レイヤーとする

- JPが遷移定義の主構造を担う
- JSCは制御・整合性の補助を担う

※ JSCの完全再定義はv1.4で実施する

---

## 8. 全体構造

JDAは以下の統合構造である

State Machine  

+ JLog  
+ VLog  
+ Learning Loop  

---

## 9. 意図

本構造により、

- 状態遷移による意思決定の構造化
- 判断ログによる記録可能性
- Learning Loopによる適応性

が実現される

さらに、

判断ログの蓄積により、組織とAIが同一の意思決定構造から学習できる状態が成立する

また、

CaseはBJをドメイン適用した際の判断対象単位であり、BJの概念を置き換えるものではない
