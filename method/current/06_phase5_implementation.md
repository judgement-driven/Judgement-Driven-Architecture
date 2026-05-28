# JDA Method v1.6 — Phase5 Implementation

---

# 1. 本フェーズの目的

Phase5 Implementation は、Phase3で設計した判断構造（JSC / JDC）および、Phase4で定義したログ設計（JLog / VLog）をもとに、

**判断を実行可能なシステムとして構築する工程**である。

v1.6におけるPhase5は、単に画面や機能を実装する工程ではない。

Phase5は、

> JPを共通実行基盤（ハーネス）上で実行し、  
> Stateを遷移させ、  
> JLogを蓄積し、  
> Learning Cycleへ接続する工程

である。

---

# 2. 本フェーズの位置づけ

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

本フェーズは、

> 判断を実際に動かし、ログを蓄積する状態を作る工程

である。

また、Phase5では、JPが実行されることで、Case / Proposal の状態・属性・判断材料が更新されていく。

このJP実行連鎖によって、Judgement Journey（JJ）が形成される。

---

# 3. 基本方針

Phase5では、判断を個別に実装するのではなく、

> 共通実行基盤（execute_jp を中心とするハーネス）上で判断を実行する構造

を採用する。

---

## 3.1 基本原則

- JPを画面ごとに個別実装しない
- 判断ロジックをUIに書かない
- Stateを中心に実装する
- JLog / VLog をLearning前提で蓄積する
- JPは外部から注入される
- ハーネスは注入されたJPを実行する
- 不要な複雑性は先に入れない

---

# 4. JDA版ハーネスエンジニアリング

## 4.1 定義

ハーネスエンジニアリングとは、
AIエージェントが動作する実行環境全体を設計する実践である。

ルール・ツール・フィードバックループを含む、
モデル以外のすべての構造を設計対象とする。

JDA版ハーネスエンジニアリングはその一形態であり、
判断（JP）の実行・記録・学習に特化した実行環境を設計する。

JDA版の特徴は、Judgement Injectionによって
JP定義と実行基盤を分離している点にある。

これにより、JPはコードではなく定義として管理され、
ハーネスを変更せずにJPを追加・更新できる。

---

## 4.2 目的

- 判断実行の統一
- JLog / VLog の一貫した蓄積
- 判断の再現性の確保
- State遷移の一貫性確保
- Learning Cycle の成立
- 将来的な判断再現・判断委譲への接続

---

## 4.3 従来との違い

従来：

```text
JPごとに個別実装
```

JDA版ハーネス：

```text
JPを共通基盤で実行
```

---

# 5. Judgement Injection

## 5.1 定義

Judgement Injectionとは、

> 状態遷移を伴う判断（JP）を、  
> 共通実行基盤（ハーネス）に対して  
> 外部定義として注入し、実行可能な形で組み込む実装方式

である。

---

## 5.2 基本構造

```python
jp = injected_jp
execute_jp(proposal, jp, input_data, actor)
```

JPはハーネスが取りに行くものではない。

JPは、UI・アプリケーション層・設定・外部定義などから注入される。

ハーネスは、注入されたJPを実行する。

JP自動選択・JP探索は現時点では行わない。

---

## 5.3 将来拡張

将来的に必要が生じた場合、以下のような機能を追加する可能性はある。

- JP適用妥当性確認
- JP自動選択
- JPルーティング
- Stateに基づく実行可能JP判定
- transition_map に基づくガード節

ただし、これらは現時点ではPhase5の必須要素ではない。

不都合が発生した場合に、必要最小限で追加する。

---

# 6. 実行モデル

## 6.1 実行単位

判断の実行単位は、Caseを具体化したインスタンスである。

本実装では、これを **Proposal** として扱う。

---

## 6.2 Case と Proposal

```text
Case = Entity × Context
```

Proposalとは、Caseを実行系に具体化したインスタンスである。

Proposalは以下を持つ。

- 状態
- 入力
- 判断結果
- JLog
- VLog
- Learning対象となる履歴

---

## 6.3 テーブル構造

```text
cases        判断対象の基礎情報
campaigns    企画・文脈情報
proposals    実行単位
jlog         判断ログ
vlog         妥当性ログ
```

---

## 6.4 状態管理

StateはProposalに紐づく。

```text
- current_state
- state_version
```

`state_version` は、同時更新や不整合を防ぐための楽観ロックに用いる。

---

# 7. JP実行

## 7.1 実行関数

```python
# JPは外部から注入される
jp = injected_jp

# ハーネスがJPを実行する
execute_jp(proposal, jp, input_data, actor)
```

---

## 7.2 execute_jp の責務

`execute_jp` は、以下を統一的に行う。

```text
1. state取得
2. state_version確認
3. jpに従って判断実行
4. state遷移
5. JLog保存
6. 結果返却
```

---

## 7.3 execute_jp の擬似コード

```python
def execute_jp(proposal, jp, input_data, actor):
    # 1. state取得
    current_state = proposal.current_state

    # 2. state_version確認
    state_version = proposal.state_version

    # 3. jpに従って判断実行
    decision = jp.execute(
        proposal=proposal,
        input_data=input_data,
        actor=actor,
        current_state=current_state,
    )

    # 4. state遷移
    next_state = jp.transition(
        current_state=current_state,
        decision=decision,
    )

    proposal.current_state = next_state
    proposal.state_version += 1

    # 5. JLog保存
    save_jlog(
        proposal=proposal,
        jp=jp,
        actor=actor,
        input_data=input_data,
        decision=decision,
        before_state=current_state,
        after_state=next_state,
    )

    # 6. 結果返却
    return {
        "proposal": proposal,
        "jp": jp,
        "decision": decision,
        "before_state": current_state,
        "after_state": next_state,
    }
```

---

## 7.4 設計ポイント

- JPはコードではなく定義として管理する
- JPはハーネスが取りに行かず、外部から渡される
- 実行ロジックは共通化する
- 状態遷移はJPに依存する
- JLog保存は必ず実行経路に含める
- Learningを前提に、判断材料と判断理由を残す

---

# 8. Judgement Journey（JJ）の形成

## 8.1 JJとは何か

JJとは、JPの実行連鎖によって形成される判断実行構造である。

Phase5では、複数のJPがProposalに対して実行されることで、JJが形成される。

---

## 8.2 JJはフェーズではない

JJは独立した工程ではない。

JJは、Implementationの中でJP実行構造として形成され、Executionを通じて観測される。

---

## 8.3 JJが表すもの

JJは、ハーネス上でCase / Proposalの状態・属性・判断材料がどのように更新されていくかを表す。

```text
Proposal
↓
execute_jp
↓
State更新
↓
属性追加
↓
判断材料更新
↓
次のexecute_jp
```

---

## 8.4 BJとの違い

BJは業務視点のJourneyである。

JJは判断実行視点のJourneyである。

多くの場合、BJとJJは近い構造を持つ。

ただし、JP共有・Learning・Proposal進化が進む場合、JJはBJを超えた独立構造として扱われる。

---

# 9. 登録経路

## 9.1 企画起点

```text
campaign選択
→ 検索
→ proposal生成
```

---

## 9.2 キーワード起点

```text
企業検索
→ JP03（企画マッチング）
→ proposal生成
```

---

## 9.3 企業DB起点

```text
企業選択
→ JP03
→ proposal生成
```

---

# 10. UI設計

UIは判断を実行するためのインターフェースである。

```text
UIはハーネスを呼び出すだけ
```

---

## 10.1 原則

- 判断ロジックはUIに書かない
- 状態に応じた操作のみ提供する
- 入力は最小限にする
- JP実行結果をJLogに残す
- UIはProposalの現在状態を表示する
- UIは次に実行可能な判断を分かりやすく提示する

---

# 11. AIの役割

v1.6では、AIの役割を段階的に捉える。

---

## 11.1 支援段階

現時点では、AIは主に判断支援を担当する。

役割は以下。

- 判断材料の収集
- 選択肢生成
- 情報整理
- 判断理由の整理
- JLog入力補助

判断は人間が行い、JLogに記録される。

---

## 11.2 再現段階

JLog / VLog が蓄積されると、AIは過去の判断をもとに判断再現を試みる。

```text
AI → Suggested Decision → Human Confirm
```

---

## 11.3 委譲段階

十分なJLog / VLogが蓄積され、判断の妥当性が確認できる場合、判断は段階的にAIへ委譲される。

```text
AI → Decision
Human → Review
```

---

# 12. Learningへの接続

Phase5の実装は、Phase6 Learning に接続される必要がある。

そのため、Phase5では、以下を必ず実装または設計対象に含める。

- JLog
- VLog
- State履歴
- 判断材料
- 判断理由
- Actor
- before_state
- after_state
- decision

---

## 12.1 Learning対象

Phase5で蓄積されたログは、以下の更新に使われる。

- Condition
- Perspective
- Threshold
- 判断材料
- JP定義
- 状態遷移

---

# 13. 成功条件

Phase5の成功条件は以下である。

- 判断が実行できる
- 状態が遷移する
- JLogが蓄積される
- VLogが評価可能である
- Proposal単位で現在状態を追跡できる
- UIがハーネスを通じてJPを実行できる
- Learning Cycleに接続できる
- Condition / Perspective / Threshold 更新へ接続できる
- JJがProposal上のJP実行連鎖として観測できる

---

# 14. よくある失敗

## 14.1 JPを個別実装する

❌ 画面ごとに実装  
⭕ ハーネスで統一

---

## 14.2 UIにロジックを書く

❌ UI依存  
⭕ 実行基盤に集約

---

## 14.3 Stateを軽視する

❌ フラグ管理  
⭕ 状態遷移管理

---

## 14.4 ログを後回しにする

❌ 動けばいい  
⭕ 学習前提で設計

---

## 14.5 先に複雑なJP選択機構を作る

❌ 使う前からJP自動選択やルーティングを作る  
⭕ 必要になるまで、JP注入 + execute_jp に留める

---

# 15. 次工程

→ Phase6 Learning

本フェーズで構築された基盤は、

```text
判断
↓
JLog
↓
VLog
↓
Learning
```

のループを成立させる中核となる。

---

# 16. 変更履歴

| Version | 内容 |
|---|---|
| v1.3 | 初版 |
| v1.4 | JDA版ハーネスエンジニアリングの導入 / 実行単位をProposalに変更 / execute_jp共通基盤の定義 / Judgement Injection概念追加 |
| v1.5 | Judgement Injection / JP共有構造 / ハーネス実装検証 |
| v1.6 | 判断実行 + 学習アーキテクチャへ拡張 / resolve_jp削除 / JJ形成 / AI支援・再現・委譲段階の反映 |