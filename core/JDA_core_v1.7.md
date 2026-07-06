# JDA Core v1.7 更新方針（最終版）

JDA Coreは、特定の実装や業務に依存しない、JDAの基本概念を定義する。

v1.7では、「実装で確認できた概念のみ」を正式採用し、未検証の内容は v1.8 Considerations へ移す。

---

## 1. バージョン更新

- JDA Core v1.6 → v1.7
- Introductionをv1.7の内容へ更新

追記内容

- Judgement Scorecard（JULIA）
- Judgement Snapshot
- Learning Stage0
- JSC / JDC の責務整理

---

## 2. JSC

JSCを正式に以下と定義する。

> JSC（Judgement State Chart）は、
> 判断対象（Target）が取り得る状態と、
> Judgement Point（JP）による状態遷移を定義する
> State Chartである。

State Spaceへの拡張は行わない。

Perspective・Hypothesis等の拡張は
v1.8 Considerationsへ移す。

---

## 3. JULIA

JULIAを正式に

> Judgement Scorecard

として定義する。

評価軸

- Judgement Financial Impact
- Urgency & Frequency
- Latency
- Influence Scope
- Adaptive / Learning Value

---

## 4. JLog

Judgement Snapshotを正式追加する。

Judgement Snapshotは、

判断時点で利用可能であった情報全体を保存する
論理構造である。

構造

Exploration Context

↓

Candidate Snapshot

↓

Decision Context

↓

Judgement

これは処理順ではなく、
判断時点の情報構造を表す。

実装側で名称が異なる場合は、
Coreを正式名称とし、
実装文書を順次合わせる。

---

## 5. Learning

Learning Stage0を正式追加する。

Stage0

Learning Foundation

↓

Stage1

Judgement Material Learning

↓

Stage2

Judgement Reproduction

↓

Stage3

Judgement Delegation

---

## 6. JSC / JDC

責務を明確化する。

JSC

状態と状態遷移を定義する。

JDC

その状態遷移を決定する
判断材料・条件・観点・主体・責任を定義する。

※ JSC/JDC責務の説明はCore内で一か所に統一する。

---

## 7. JDC構造

Proceed / Validity / Accountability / Venture の
4軸構造はv1.7で正式採用しない。

実装で確認されたフラットなJDC構造を採用する。

旧4軸構造は履歴として残し、
必要に応じてv1.8以降で再検討する。

---

## 8. Current Core Concepts

Current Core Concepts を以下へ更新する。

- Business Journey
- Judgement Point
- Judgement State Chart
- Judgement Design Canvas
- Judgement Scorecard
- Judgement Snapshot
- JLog
- VLog
- Judgement Harness
- Judgement Injection
- Learning Cycle（Stage0–3）

Proceed / Validity / Accountability / Venture は
Core Concept一覧から外す。

---

## 9. v1.8へ持ち越す項目

以下は正式採用せず、
JDA_v1.8_considerations.md へ移す。

- Perspective拡張
- Hypothesis拡張
- JSC拡張
- その他、BJ04以降で実証が必要な概念
