# JDA Method v1.4 — Phase5 Implementation（改訂版）

## 1. 本フェーズの目的

Phase5 Implementationは、Phase3で設計した判断構造（JSC / JDC）および
Phase4で定義したログ設計（JLog / VLog）をもとに、

**判断を実行可能なシステムとして構築する工程**である。

---

## 2. 本フェーズの位置づけ

```text
Discovery（判断抽出）
↓
JULIA（判断選定）
↓
Design（判断設計）
↓
Log（ログ設計）
↓
Implementation（実装） ← 本フェーズ
↓
Learning（学習）
```

> 本フェーズは
> **判断を実際に動かし、ログを蓄積する状態を作る工程**である。

---

## 3. 基本方針

Phase5では、判断を個別に実装するのではなく、

**共通実行基盤（execute_jpを中心とするハーネス）上で判断を実行する構造**を採用する。

---

## 4. JDA版ハーネスエンジニアリング

### 4.1 定義

JDA版ハーネスエンジニアリングとは、

> 状態遷移を伴う判断（JP）を、
> 再現可能かつ学習可能な形で実行するために、
> 状態・入力・実行・ログを統一する実装基盤を設計することである。

本ハーネスは**Judgement Injection**を前提とする。

---

### 4.2 目的

- 判断実行の統一
- JLog / VLogの一貫した蓄積
- 判断の再現性の確保
- 学習ループの成立

---

### 4.3 従来との違い

従来：

```
JPごとに個別実装
```

JDA版ハーネス：

```
JPを共通基盤で実行
```

---

### 4.4 Judgement Injection

**定義**

Judgement Injectionとは、

> 状態遷移を伴う判断（JP）を、共通実行基盤（ハーネス）に対して
> 外部定義として注入し、実行可能な形で組み込む実装方式

である。

**従来との違い**

```python
# Before（Service Locator的）
def execute_jp(case_id, jp_id, input_data):
    jp = load_jp(jp_id)  # ハーネスが取りに行く
    jp.execute()

# After（Injection）
jp = resolve_jp(case, context)  # 外部で解決
execute_jp(case, jp, input_data)  # 渡す
```

**ポイント**

- JPはハーネスが「取りに行く」ものではない
- JPは外部から「渡される」もの
- ハーネスは実行に専念する
- 判断構造と実行構造が分離される

**メリット**

- JP定義がコードから分離される
- JP追加がハーネスを触らずにできる
- 将来のJSON定義・DB定義への移行が自然になる
- Learning LoopによるJP更新がハーネスに影響しない

---

## 5. 実行モデル

### 5.1 実行単位

判断の実行単位はCaseを具体化したインスタンスであり、
本実装ではこれを**Proposal**として扱う。

---

### 5.2 テーブル構造

```
cases        企業マスタ
campaigns    企画マスタ
proposals    実行単位（主役）
jlog         判断ログ
vlog         妥当性ログ
```

---

### 5.3 状態管理

StateはProposalに紐づく。

```
- current_state
- state_version（楽観ロック）
```

---

## 6. JP実行

### 6.1 実行関数

```python
# 呼び出し側：JPを外部で解決して渡す
jp = resolve_jp(proposal, context)
execute_jp(proposal, jp, input_data, actor)

# ハーネス定義：JPを受け取って実行する
def execute_jp(proposal, jp, input_data, actor):
    # 1. state取得（state_versionで楽観ロック）
    # 2. jpに従って判断実行（外部注入済み）
    # 3. state遷移
    # 4. JLog保存
    # 5. 結果返却
    pass
```

---

### 6.2 処理フロー

```text
1. resolve_jp（JPを外部で解決）
2. execute_jp（JPを渡して実行）
3. state取得
4. jpに従って判断実行
5. state遷移
6. JLog保存
7. 結果返却
```

---

### 6.3 設計ポイント

- JPはコードではなく**定義**として管理する
- JPはハーネスが取りに行かず、外部から渡される
- 実行ロジックは共通化する
- 状態遷移はJPに依存する

---

## 7. 登録経路

### 7.1 企画起点

```text
campaign選択
→ 検索
→ proposal生成
```

### 7.2 キーワード起点

```text
企業検索
→ JP03（企画マッチング）
→ proposal生成
```

### 7.3 企業DB起点

```text
企業選択
→ JP03
→ proposal生成
```

---

## 8. UI設計

UIは判断を実行するためのインターフェースであり、

```
UIはハーネスを呼び出すだけ
```

とする。

### 8.1 原則

- 判断ロジックはUIに書かない
- 状態に応じた操作のみ提供
- 入力は最小限

---

## 9. AIの役割

現時点ではAIは判断を実行しない。

役割は以下。

- 判断材料の収集
- 選択肢生成
- 情報整理

判断は人間が行い、JLogに記録される。

---

## 10. 成功条件

- 判断が実行できる
- 状態が遷移する
- JLogが蓄積される
- VLogが評価可能である
- 学習ループに接続できる

---

## 11. よくある失敗

### 11.1 JPを個別実装する

❌ 画面ごとに実装
⭕ ハーネスで統一

### 11.2 UIにロジックを書く

❌ UI依存
⭕ 実行基盤に集約

### 11.3 Stateを軽視する

❌ フラグ管理
⭕ 状態遷移管理

### 11.4 ログを後回しにする

❌ 動けばいい
⭕ 学習前提で設計

---

## 12. 次工程

→ Phase6 Learning

本フェーズで構築された基盤は、

```text
判断 → JLog → VLog → Learning
```

のループを成立させる中核となる。

---

## 変更履歴

| Version | 内容 |
|---------|------|
| v1.3 | 初版 |
| v1.4 | JDA版ハーネスエンジニアリングの導入 / 実行単位をProposalに変更 / execute_jp共通基盤の定義 / Judgement Injection概念追加 |