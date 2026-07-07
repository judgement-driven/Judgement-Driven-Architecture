# JDA Method v1.7 — Phase5 Implementation

---

# 1. 本フェーズの目的

Phase5 Implementation は、Phase3で設計した判断構造（JSC / JDC）および、Phase4で定義したログ設計（JLog / VLog）をもとに、

判断を実行可能なシステムとして構築する工程である。

Phase5は、単に画面や機能を実装する工程ではない。

Phase5は、

```text
JPを共通実行基盤（Judgement Harness）上で実行し、
Stateを遷移させ、
JLogを蓄積し、
Learning Cycleへ接続する工程
```

である。

また、Phase5では、判断を実行可能な基盤を作るだけでなく、

```text
JULIAで選定されたJPをどう実装可能にするか
どこまで作るか
未実装工程をどう扱うか
いつ現場運用に出すか
```

を判断しながら進める。

v1.7では、この進め方を **Judgement Slice Implementation** という実装パターンとして、Phase5の内部に統合して扱う。

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

```text
判断を実際に動かし、ログを蓄積する状態を作る工程
```

である。

また、Phase5では、JPが実行されることで、Case / Proposal の状態・属性・Data Sourcesが更新されていく。

このJP実行連鎖によって、Judgement Journey（JJ）が形成される。

---

# 3. 基本方針

Phase5では、判断を個別に実装するのではなく、

```text
共通実行基盤（Judgement Harness）上で判断を実行する構造
```

を採用する。

中心となる実行関数は execute_jp である。

## 3.1 基本原則

- JPを画面ごとに個別実装しない
- 判断ロジックをUIに書かない
- Stateを中心に実装する
- JLog / VLog をLearning前提で蓄積する
- JPは外部から注入される
- Judgement Harness は注入されたJPを実行する
- 不要な複雑性は先に入れない

---

# 4. Judgement Harness

## 4.1 定義

Judgement Harnessとは、

```text
状態・入力・実行・ログを統一し、
判断（JP）を再現可能かつ学習可能な形で運用するための実装基盤
```

である。

JDAでは、各画面や各機能に個別の判断ロジックを埋め込むのではなく、JPを外部定義として管理し、Judgement Harness 上で実行する。

Judgement Harness の特徴は、Judgement Injectionによって、JP定義と実行基盤を分離している点にある。

これにより、JPはコードではなく定義として管理され、Judgement Harness を変更せずにJPを追加・更新できる。

## 4.2 目的

- 判断実行の統一
- JLog / VLog の一貫した蓄積
- 判断の再現性の確保
- State遷移の一貫性確保
- Learning Cycle の成立
- 将来的な判断再現・判断委譲への接続

## 4.3 従来との違い

従来：

```text
JPごとに個別実装
```

JDA：

```text
JPをJudgement Harnessで実行
```

---

# 5. Judgement Injection

## 5.1 定義

Judgement Injectionとは、

```text
状態遷移を伴う判断（JP）を、
Judgement Harness に対して
外部定義として注入し、実行可能な形で組み込む実装方式
```

である。

## 5.2 基本構造

```python
jp = injected_jp
execute_jp(proposal, jp, data_sources, actor)
```

JPはJudgement Harnessが自動的に探索するものではない。

JPは、UI・アプリケーション層・設定・外部定義などから注入される。

Judgement Harness は、注入されたJPを実行する。

JP自動選択・JP探索は現時点では行わない。

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

# 6. 実行モデル（Case / Proposal / State）

## 6.1 実行単位

判断の実行単位は、Caseを具体化したインスタンスである。

本実装では、これを Proposal として扱う。

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

## 6.3 テーブル構造

```text
cases        判断対象の基礎情報
contexts     文脈情報（企画・案件・期間など）
proposals    実行単位
jlog         判断ログ
vlog         妥当性ログ
```

※ テーブル名は実装によって異なる。重要なのはこの構造の考え方である。

## 6.4 状態管理

StateはProposalに紐づく。

- current_state
- state_version

state_version は、同時更新や不整合を防ぐための楽観ロックに用いる。

## 6.5 Proposal生成の例（BJ01）

以下はBJ01（新規クライアント獲得）における実装例である。対象BJによって登録経路は異なる。

### 企画起点

```text
campaign選択
→ 検索
→ proposal生成
```

### キーワード起点

```text
企業検索
→ JP03（企画マッチング）
→ proposal生成
```

### 企業DB起点

```text
企業選択
→ JP03
→ proposal生成
```

---

# 7. execute_jp

## 7.1 実行関数

```python
# JPは外部から注入される
jp = injected_jp
# Judgement Harness がJPを実行する
execute_jp(proposal, jp, data_sources, actor)
```

## 7.2 execute_jp の責務

execute_jp は、以下を統一的に行う。

1. state取得
2. state_version確認
3. jpに従って判断実行
4. state遷移
5. JLog保存
6. 結果返却

## 7.3 execute_jp の擬似コード

```python
def execute_jp(proposal, jp, data_sources, actor):
    # 1. state取得
    current_state = proposal.current_state
    # 2. state_version確認
    state_version = proposal.state_version
    # 3. jpに従って判断実行
    decision = jp.execute(
        proposal=proposal,
        data_sources=data_sources,
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
        data_sources=data_sources,
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

## 7.4 設計ポイント

- JPはコードではなく定義として管理する
- JPはJudgement Harnessが探索せず、外部から渡される
- 実行ロジックは共通化する
- 状態遷移はJPに依存する
- JLog保存は必ず実行経路に含める
- Learningを前提に、Data Sourcesと判断理由を残す

---

# 8. Judgement Journey（JJ）の形成

## 8.1 JJとは何か

JJとは、JPの実行連鎖によって形成される判断実行構造である。

Phase5では、複数のJPがProposalに対して実行されることで、JJが形成される。

## 8.2 JJはフェーズではない

JJは独立した工程ではない。

JJは、Implementationの中でJP実行構造として形成され、Executionを通じて観測される。

## 8.3 JJが表すもの

JJは、Judgement Harness 上でCase / Proposalの状態・属性・Data Sourcesがどのように更新されていくかを表す。

```text
Proposal
↓
execute_jp
↓
State更新
↓
属性追加
↓
Data Sources更新
↓
次のexecute_jp
```

## 8.4 BJとの違い

BJは業務視点のJourneyである。

JJは判断実行視点のJourneyである。

多くの場合、BJとJJは近い構造を持つ。

ただし、JP共有・Learning・Proposal進化が進む場合、JJはBJを超えた独立構造として扱われる。

---

# 9. Judgement Slice Implementation

Phase5では、Judgement Harness・Judgement Injection・execute_jp・Proposal・State・JLog / VLog・JJ形成という実行基盤を扱う。

一方、実際の開発では、

```text
JULIAで選定されたJPをどう実装可能にするか
どこまで作るか
未実装工程をどう扱うか
いつ現場運用に出すか
```

を判断する必要がある。v1.7では、この実装パターンを **Judgement Slice Implementation** として、Phase5の内部で扱う。

```text
Phase2 JULIA
= どのJPに投資するか

Phase5（実行基盤）
= どう実行するか

Judgement Slice Implementation
= どう小さく作り始めるか
```

基本フローは以下である（詳細な手順は「11. 実装順序」で扱う）。

```text
Phase2 JULIAで主要JPを選定する
↓
対象JPを確認する
↓
対象JPに必要なData Sourcesを定義する
↓
最小限のProposal / State / JLog / VLogを実装する
↓
前後工程をOperational Bridgeで補完する
↓
現場で運用する
↓
JLog / VLogを観測する
↓
必要な周辺JPの実行環境を追加する
↓
JJとして整理する
```

## 9.1 なぜ必要か

JDAでは、最初からBJ全体を完全に実装することを目的としない。

理由は以下である。

- すべてのJPを最初から確定できない
- UIや入力負荷は実際に使わないと分からない
- Data Sourcesは実装・運用しながら精度が上がる
- JLog / VLog は運用して初めて価値が出る
- 妥当性評価はJPごとに実装・運用しながら固まる
- 不要な複雑性を先に入れると、Learning前に実装が重くなる

そのため、JDA実装では、

```text
Phase2 JULIAで選定された主要JPを中心に、
Data Sources提示・判断入力・ログ収集を先に実装し、
現場運用からJLog / VLogを取得し、
必要な周辺JPを後から追加する
```

という進め方が有効である。

## 9.2 Judgement Sliceとは何か

Judgement Sliceとは、JDA実装における最小実行単位である。

通常の業務システムでは、機能や画面単位で実装範囲を切る。

一方、JDAでは、判断を中心に実装範囲を切る。

### 通常の実装単位

```text
画面
機能
業務フロー
```

### JDAの実装単位

```text
JP
Proposal
State
JLog
VLog
```

### Judgement Sliceの定義

Judgement Sliceは、JPそのものを作る単位ではない。

Judgement Sliceとは、

```text
主要JPを現場で運用可能にするために、
Data Sources提示・判断入力・状態遷移・JLog / VLog収集を
最小構成で実装する単位
```

である。

なお、「JPを作る」とはJPそのものをシステム化することではなく、Data Sources提示・入力・状態遷移・JLog収集を実装することを指す。

## 9.3 通常のVertical Sliceとの違い

Judgement Sliceは、Vertical Sliceに近い。

ただし、通常のVertical Sliceが機能を縦断するのに対して、Judgement Sliceは判断を縦断する。

```text
UI
↓
Data Sources提示
↓
判断入力
↓
JP実行
↓
State遷移
↓
JLog
↓
VLog
↓
Learning
```

までを薄く通すことを重視する。

## 9.4 Judgement Sliceの最小構成

Judgement Sliceは、最低限以下を含む。

- 対象JP
- 対象Proposal
- current_state
- Data Sources
- decision
- reason
- actor
- before_state
- after_state
- JLog
- VLog
- Operational Bridge接続

JDA実装では、

```text
全部作ってから運用する
```

のではなく、

```text
判断ログが取れる最小構造で先に現場へ出す
```

ことを優先する。そのために、Judgement Slice Implementation を用いる。

---

# 10. Operational Bridge

## 10.1 定義

Operational Bridgeとは、

```text
未実装JPや未実装工程を、現場運用で補完しながら、
JDAのJudgement Harnessに接続するための暫定的な接続層
```

である。

JDA実装では、最初からすべての工程をシステム化しない。未実装部分は、既存運用や軽量ツールで補完し、主要JPの運用開始を優先する。

Operational Bridgeの目的は、すべてを作り込むことではない。目的は、

```text
主要JPを先に現場へ出すこと
```

である。そのため、Operational Bridgeは一時的であってよい。ただし、運用を通じて重要性が高いと判断された場合、後から正式なJP実行環境または機能として実装する。

Operational Bridgeは、Judgement Harnessを迂回するためのものではない。Operational Bridgeを使用する場合でも、判断は可能な限りJudgement Harness上で実行し、State更新・JLog保存・VLog評価へ接続できるようにする。

```text
Operational Bridge
↓
Proposal / Data Sources
↓
execute_jp
↓
State / JLog / VLog
↓
Operational Bridge
```

Operational Bridgeの目的は、JDAの外で業務を完結させることではない。目的は、既存運用をJDAの実行基盤に接続することである。

## 10.2 Bridge in / Bridge out

Operational Bridgeは、以下の2方向で設計する。

### Bridge in

既存データ・Excel・CSV・手動入力を、Proposal / Data Sources としてハーネスに入れる。

```text
Excel / CSV / 既存管理表
↓
import
↓
Proposal / Data Sources
↓
execute_jp
```

### Bridge out

execute_jp の結果・State・JLog / VLogを、Excel / CSV / 現場運用へ返す。

```text
execute_jp
↓
State / JLog / VLog
↓
export
↓
Excel / CSV / 現場運用
```

## 10.3 Excel / CSV / 既存運用との接続

Operational Bridgeの具体例として、以下がある。

- Excel import/export
- CSV連携
- 手動入力
- 既存管理表
- メール運用
- 一時的なスプレッドシート
- 手動確認フロー

Excel import/export は、Operational Bridge の一例である。これは単なる便利機能ではなく、

```text
未実装JP
未実装工程
既存業務
```

を現場運用で補完するためのブリッジである。ただし、ExcelはBJ01固有の実装手段であり、Method上はOperational Bridgeの具体例として扱う。

## 10.4 避けるべきOperational Bridge

以下は避ける。

```text
Excelで判断
Excelで状態更新
Excelで完結
あとでシステムへ転記
```

この形では、JLog / VLog が残らず、Learningにつながらない。

Operational Bridgeを使う場合でも、判断の実行・状態更新・ログ収集は、可能な限りハーネスへ接続する。

---

# 11. 実装順序

## 11.1 Step1：JULIAで選定された主要JPを確認する

最初に、Phase2 JULIAで選定された主要JPを確認する。

Judgement Slice Implementationでは、JPの優先順位そのものは決定しない。

優先順位はPhase2 JULIAで決定し、本パターンでは、そのJPをどの範囲で最小実装するかを決める。

## 11.2 Step2：Data Sourcesを定義する

対象JPに対して、人間が判断するために必要なData Sources・Conditions・Perspectivesを定義する。

含まれる項目の例：

- Data Sources（過去履歴、類似Case、AI補強情報、現場メモなど）
- Conditions
- Perspectives
- 判断理由の入力項目

この段階で重要なのは、AIが判断することではなく、人間が判断しやすくなる材料を整えることである。

## 11.3 Step3：最小実行構造を作る

主要JPを運用可能にするために必要な最小構造を作る。

最低限必要なものは以下である。

- Proposal
- current_state
- execute_jp
- Data Sources提示
- 判断入力
- 判断理由入力
- JLog
- VLog
- UI
- import/export または既存運用との接続

## 11.4 Step4：前後工程をOperational Bridgeで補完する

主要JPの前後に未実装工程がある場合、すべてをシステム化しない。

まずはOperational Bridgeで補完する。

例：

```text
前工程：Excelから取り込む
主要JP：システム上で判断する
後工程：Excelへ出力して現場運用する
```

この場合でも、主要JPの判断は可能な限りハーネス上で実行し、JLog / VLogに接続する。

## 11.5 Step5：現場で運用する

小さく実装したら、現場に使ってもらう。

この段階で重要なのは、完成度ではなく、JLog / VLog が取れることである。

現場が判断できること、Data Sourcesが提示されること、判断理由が残ることを優先する。

## 11.6 Step6：JLog / VLogを観測する

運用後、以下を観測する。

- どのJPが使われたか
- どのJPで止まったか
- Data Sourcesは足りたか
- 判断理由は残ったか
- VLogを書けるか
- 現場が入力を嫌がる箇所はどこか
- Operational Bridgeで十分か
- ハーネスに接続できているか

## 11.7 Step7：必要な周辺JPだけ追加する

ログと運用の違和感から、必要な周辺JPの実行環境だけを追加する。

ここでも、判断そのものを作るのではなく、JPを運用可能にするための構造を追加する。

追加判断の基準は、

```text
そのJP実行環境を追加すると、
JLog / VLog が増え、
Learningにつながるか
```

である。

## 11.8 Step8：JJとして整理する

複数JPが連鎖して動き始めたら、それを Judgement Journey（JJ）として整理する。

JJは最初から完全に設計するものではない。

実装と運用の中で、JP実行連鎖として形成される。

---

# 12. 実装範囲の判断基準

実装範囲を広げるかどうかは、以下の4つで判断する。

## 12.1 この機能はJLog / VLogを増やすか？

JDA実装において重要なのは、機能数ではなく、判断ログが増えることである。

JLog / VLogが増えない機能は、Learningへの貢献が小さい。

## 12.2 この機能は主要JPの運用に必要か？

主要JPを現場で回すために必要な機能は実装する。

逆に、主要JPの運用に直接関係しない機能は後回しにする。

## 12.3 このJPにVLogを書けるか？

VLogを書けないJPは、妥当性評価ができない。

妥当性評価ができなければ、Learningに接続しにくい。

そのため、JPの実行環境を追加する場合は、

```text
後から妥当性を評価できるか
```

を確認する。

## 12.4 Operational Bridgeで逃がせないか？

正式実装する前に、Operational Bridgeで補完できないかを確認する。

Excel、CSV、既存管理表、手動確認で十分な場合は、最初から作り込まない。

ただし、Operational Bridgeで逃がす場合でも、ハーネスへの接続を意識する。

---

# 13. UI設計

UIは判断を実行するためのインターフェースである。

```text
UIはJudgement Harnessを呼び出すだけ
```

## 13.1 原則

- 判断ロジックはUIに書かない
- 状態に応じた操作のみ提供する
- 入力は最小限にする
- JP実行結果をJLogに残す
- UIはProposalの現在状態を表示する
- UIは次に実行可能な判断を分かりやすく提示する

---

# 14. AIの役割

JDAでは、AIの役割を段階的に捉える。

## 14.1 Judgement Material Learning（支援段階）

現時点では、AIは主に判断支援を担当する。

役割は以下。

- Data Sourcesの収集
- 選択肢生成
- 情報整理
- 判断理由の整理
- JLog入力補助

判断は人間が行い、JLogに記録される。

## 14.2 Judgement Reproduction Learning（再現段階）

JLog / VLog が蓄積されると、AIは過去の判断をもとに判断再現を試みる。

```text
AI → Suggested Decision → Human Confirm
```

## 14.3 Judgement Delegation（委譲段階）

十分なJLog / VLogが蓄積され、判断の妥当性が確認できる場合、判断は段階的にAIへ委譲される。

```text
AI → Decision
Human → Review
```

---

# 15. Learningへの接続

Phase5の実装は、Phase6 Learning に接続される必要がある。

そのため、Phase5では、以下を必ず実装または設計対象に含める。

- JLog
- VLog
- State履歴
- Data Sources
- 判断理由
- Actor
- before_state
- after_state
- decision

## 15.1 Learning対象

Phase5で蓄積されたログは、以下の更新に使われる。

- Conditions
- Perspectives
- Data Sources
- JP定義
- 状態遷移

※ Thresholdは正式概念として扱わない。必要であればv1.8以降の検討事項とする。

---

# 16. 成功条件

Phase5の成功条件は以下である。

- 判断が実行できる
- 状態が遷移する
- JLogが蓄積される
- VLogが評価可能である
- Proposal単位で現在状態を追跡できる
- UIがJudgement Harnessを通じてJPを実行できる
- Learning Cycleに接続できる
- Conditions / Perspectives 更新へ接続できる
- JJがProposal上のJP実行連鎖として観測できる
- 主要JPがJudgement Sliceとして最小構成で運用できている
- Operational Bridgeを使う場合でもJudgement Harnessに接続できている

---

# 17. よくある失敗

## 17.1 JPを個別実装する

❌ 画面ごとに実装
⭕ Judgement Harnessで統一

## 17.2 UIにロジックを書く

❌ UI依存
⭕ Judgement Harnessに集約

## 17.3 Stateを軽視する

❌ フラグ管理
⭕ 状態遷移管理

## 17.4 ログを後回しにする

❌ 動けばいい
⭕ 学習前提で設計

## 17.5 先に複雑なJP選択機構を作る

❌ 使う前からJP自動選択やルーティングを作る
⭕ 必要になるまで、JP注入 + execute_jp に留める

## 17.6 最初からBJ全体を作ろうとする

❌ BJ全体を完全実装する
⭕ 主要JPから始める

## 17.7 UIを作り込みすぎる

❌ 画面を整えることが目的になる
⭕ Data Sourcesが提示され、判断が実行され、JLogが残ることを優先する

## 17.8 前後工程をすべてシステム化する

❌ 全工程を最初から作る
⭕ Operational Bridgeで補完する

## 17.9 JLogだけで満足する

❌ 判断記録だけ残す
⭕ VLogまで残せる構造にする

## 17.10 VLogを書けないJPを追加する

❌ 後から評価できない判断を増やす
⭕ 妥当性評価できる判断から実装する

## 17.11 作るのが楽しくなって広げすぎる

❌ AI実装が速いので、必要以上に作り込む
⭕ 実装範囲の判断基準に戻る

## 17.12 JULIAを無視して作り始める

❌ 作りやすいJPから作る
⭕ JULIAで選定された主要JPから始める

## 17.13 Operational BridgeがJDAの外で完結する

❌ Excelや既存運用だけで判断・状態更新・完結する
⭕ Operational Bridgeをハーネスに接続する

## 17.14 JPそのものをシステムが作ると誤解する

❌ JPを作る = 判断をシステムが作る
⭕ JPを運用可能にする = Data Sources提示・入力・状態遷移・ログ収集を作る

---

# 18. 次工程

→ Phase6 Learning

本フェーズで構築された基盤は、

```text
判断
↓
JLog
↓
VLog
↓
Learning Cycle
```

のループを成立させる中核となる。

---

# 19. 変更履歴

| Version | 内容 |
|---|---|
| v1.3 | 初版 |
| v1.4 | Judgement Harness の導入 / 実行単位をProposalに変更 / execute_jp共通基盤の定義 / Judgement Injection概念追加 |
| v1.5 | Judgement Injection / JP共有構造 / Judgement Harness実装検証 |
| v1.6 | 判断実行 + 学習アーキテクチャへ拡張 / resolve_jp削除 / JJ形成 / AI支援・再現・委譲段階の反映 / Judgement Slice Implementation・Operational Bridgeを別文書（06a_judgement_slice_implementation.md）として定義 |
| v1.7 | Phase5 ImplementationとJudgement Slice Implementationを統合 / 06a_judgement_slice_implementation.mdを廃止 / Judgement Slice ImplementationをPhase5内の実装パターンとして整理 / Operational BridgeをPhase5内に統合 / Learning Cycle名称をCore v1.7（Judgement Material Learning・Judgement Reproduction Learning・Judgement Delegation）へ統一 / 判断材料をData Sourcesへ、Condition・PerspectiveをConditions・Perspectivesへ用語統一 / Thresholdを正式概念から除外 / Core v1.7・README v1.7との整合 |
