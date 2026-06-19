# JDA v1.7 更新候補メモ

作成日：2026-06-10

---

## 1. JSC定義の更新

### 現行（v1.6）
> JPによって引き起こされる状態遷移モデル

### 更新候補
> 判断対象（Target）がJPによる判断の結果として
> 取り得る状態と、その遷移を定義する状態遷移図。

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
JSC = 状態遷移図
　→ Targetが取り得る状態と遷移を定義する

JDC = 判断ロジック
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

---

## 9. VLog評価のタイミング（直後状態と後続状態）

### 背景
note#9執筆時の壁打ちで発見。
Stage0（状態遷移からのVLog自動生成）を設計する際、
「どの時点の状態を比較するか」が曖昧だと機能しない。

### 整理

JP実行（execute_jp）により、Proposalは即座に
transition_mapに従った状態へ遷移する。
これを「直後状態」とする。

直後状態は判断の結果として確定したものであり、
それ自体に妥当性の差分は生まれない。

VLogが評価すべきは、直後状態に遷移した後、
Proposalがその後どう推移したか（後続状態）である。

```text
JP実行 → 直後状態（期待した遷移そのもの）
↓
時間経過・後続JP・外部要因
↓
後続状態（実際の結果）
↓
VLog：直後状態が想定していた「望ましい後続状態」に
　　　実際の後続状態が一致したか
```

### JSCへの影響

これにより、JSC（State Space）には、
各状態について単に「次にあり得る状態」だけでなく、

```text
その状態から見て、
「望ましい後続状態」と「望ましくない後続状態」
```

を区別できる定義が必要になる可能性がある。

ただし、これをJSC自体に持たせるか、
JDCのTransition Logic側に持たせるかは継続検討。

現時点の方向性：

```text
JSC：状態空間（State Space）のまま維持
JDC：「望ましい後続状態」の定義をTransition Logicの一部として持つ
　　　→ VLog評価ロジックの根拠となる
```

### 備考
- Stage0自動VLog生成の実装設計に直結する論点
- 「直後状態＝期待状態」という単純化は、note等の一般向け説明では問題ないが、
  実装・評価設計では区別が必要

## 10. Learning Stageの再定義

### Stage0 Learning Foundation

目的：
Learningを成立させるための評価基盤を構築する。

内容：

JP
↓
状態確定
↓
後続状態追跡
↓
VLog生成

を自動的に実現する。

Stage0では判断モデルを改善しない。

まず妥当性評価（VLog）が継続的に取得できる状態を作る。

### Stage1 Judgement Assistance

判断材料改善

### Stage2 Judgement Reproduction

判断モデル改善

### Stage3 Judgement Delegation

判断委譲改善

## 11. Actor Context（認証・判断主体）

Actor Contextは、JDAの実行基盤における共通要素である。

JDAにおいて、判断は必ず判断主体（Actor）に紐づく。

そのためPhase5 Implementationでは、execute_jpを実行する前提として、
認証済みユーザー情報をActor Contextとして取得し、
JLogに保存できる状態を共通基盤として用意する。

Actor Contextには最低限以下を含める。

- actor_id
- actor_name
- role

認証そのものはexecute_jpの内部責務ではない。
execute_jpは、認証済みのActor Contextを受け取り、
判断結果・状態遷移・JLogとともに保存する。

#### 認証レベル

Level 1：簡易認証
- ユーザー選択 + PIN
- 目的：JLogに判断主体を残す

Level 2：権限管理
- roleによる操作制御
- 目的：管理機能・削除・設定変更を制御する

Level 3：外部認証
- Google Workspace / OAuth 等
- 目的：本格運用・監査対応

## 12. JDC要素定義の再確認

### 背景

文化アディック実証において、JP01の再設計を行う中で、
JDCの各要素の意味を再確認する必要が生じた。

特に、

- 判断そのもの
- 判断材料
- 判断条件
- 判断観点
- 判断結果

が混同されやすいことが分かった。

そのため、v1.7候補としてJDCの要素定義を明確化する。

---

### JDCの基本構造

JDCは、JPを実行可能な判断として設計するためのモデルである。

JDCは以下の要素で構成する。

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

### Purpose

このJPが何のために存在するかを定義する。

---

### Subject

判断対象を定義する。

「何について判断するのか」を示す。

例：

- Proposal
- Company
- Search Candidate
- Production Asset

---

### Data Sources

判断に使用する材料を定義する。

Data Sourcesは判断そのものではなく、
判断を行うために参照される情報である。

例：

- 検索結果
- 企業情報
- AI補強情報
- 過去履歴
- 問い合わせフォーム有無

---

### Conditions

判断条件を定義する。

Conditionsは、比較的客観的に確認できる条件である。

例：

- 企業・ブランド・サービスである
- 問い合わせ導線がある
- 既存NGではない
- 重複していない

---

### Perspectives

判断観点を定義する。

Perspectivesは、経験・文脈・主観を含む評価観点である。

例：

- 雑誌との適合性
- 商品の魅力
- 話題性
- 広告出稿可能性
- 過去成功事例との類似性

---

### Decision

判断結果を定義する。

JPによって確定される判断のアウトプットである。

例：

- ネタ採用
- 保留
- 対象外
- アタリ
- 非アタリ
- 成約

---

### Actor

判断主体を定義する。

誰が判断するかを示す。

例：

- 営業担当
- 管理者
- AI提案 + 人間確認

---

### Accountability

判断責任を定義する。

判断の責任を誰が持つかを示す。

明示的な責任者がいない場合でも、
「特になし」または「運用上は営業担当」として記載する。

---

### Output

判断後に何が出力されるかを定義する。

状態遷移、JLog、後続アクション、登録更新などが含まれる。

---

### JP01再設計で得られた整理

JP01において、

検索結果、企業概要、問い合わせフォーム有無、AI補強情報は
JP01そのものではない。

これらはData Sourcesである。

JP01そのものは、

「この検索候補をネタ候補として採用するか」

という判断である。

つまり、

```text
Data Sources = 判断材料
JP = 判断
Decision = 判断結果
Output = 判断後の処理

---

## 13. JSCの表現見直し

### 背景

v1.7検討において、

JSC = State Space

という表現を使用していた。

しかし実際のJSCは、
状態の集合そのものではなく、

状態
↓
JP
↓
次状態

を表現するモデルである。

### 更新候補

JSC = State Chart

### 整理

State Space
= 取り得る状態の集合

State Chart
= 状態と遷移を表現するモデル

JDAにおけるJSCは後者に近い。

### 備考

Judgement-Driven State Machine
という考え方自体は維持する。

JSCはその状態遷移を表現するState Chartである。
