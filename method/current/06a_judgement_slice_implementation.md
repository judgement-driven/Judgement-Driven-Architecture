# JDA Method v1.6 — Implementation Pattern

## Judgement Slice Implementation

---

# 1. 目的

本ドキュメントは、JDA Method v1.6 における実装の進め方を定義する。

Phase5 Implementation は、主に以下を扱う。

- 共通JP実行基盤
- execute_jp
- Judgement Injection
- Proposal
- State
- JLog / VLog
- JJ形成

一方で、実際の開発では、

```text
JULIAで選定されたJPをどう実装可能にするか
どこまで作るか
未実装工程をどう扱うか
いつ現場運用に出すか
```

を判断する必要がある。

本ドキュメントでは、その実装パターンとして、

> Judgement Slice Implementation

を定義する。

---

# 2. なぜ必要か

JDAでは、最初からBJ全体を完全に実装することを目的としない。

理由は以下である。

- すべてのJPを最初から確定できない
- UIや入力負荷は実際に使わないと分からない
- 判断材料は実装・運用しながら精度が上がる
- JLog / VLog は運用して初めて価値が出る
- 妥当性評価はJPごとに実装・運用しながら固まる
- 不要な複雑性を先に入れると、Learning前に実装が重くなる

そのため、JDA実装では、

```text
Phase2 JULIAで選定された主要JPを中心に、
判断材料提示・判断入力・ログ収集を先に実装し、
現場運用からJLog / VLogを取得し、
必要な周辺JPを後から追加する
```

という進め方が有効である。

---

# 3. 基本構造

Judgement Slice Implementation とは、

> JULIAで選定された主要JPを中心に、  
> 判断材料提示・判断入力・状態遷移・JLog / VLog収集を先に実装し、  
> 前後工程はOperational Bridgeで補完しながら、  
> 必要に応じて周辺JPの実行環境を追加していく実装パターン

である。

なお、「JPを作る」とはJPそのものをシステム化することではなく、  
判断材料提示・入力・状態遷移・JLog収集を実装することを指す。

---

## 3.1 基本フロー

```text
Phase2 JULIAで主要JPを選定する
↓
対象JPを確認する
↓
対象JPに必要な判断材料を定義する
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

---

# 4. Judgement Sliceとは何か

Judgement Sliceとは、JDA実装における最小実行単位である。

通常の業務システムでは、機能や画面単位で実装範囲を切る。

一方、JDAでは、判断を中心に実装範囲を切る。

---

## 4.1 通常の実装単位

```text
画面
機能
業務フロー
```

---

## 4.2 JDAの実装単位

```text
JP
Proposal
State
JLog
VLog
```

---

## 4.3 Judgement Sliceの定義

Judgement Sliceは、JPそのものを作る単位ではない。

Judgement Sliceとは、

> 主要JPを現場で運用可能にするために、  
> 判断材料提示・判断入力・状態遷移・JLog / VLog収集を  
> 最小構成で実装する単位

である。

---

## 4.4 Judgement Sliceの構成

Judgement Sliceは、最低限以下を含む。

- 対象JP
- 対象Proposal
- current_state
- 判断材料
- input_data
- decision
- reason
- actor
- before_state
- after_state
- JLog
- VLog
- Operational Bridge接続

---

## 4.5 Vertical Sliceとの違い

Judgement Sliceは、Vertical Sliceに近い。

ただし、通常のVertical Sliceが機能を縦断するのに対して、  
Judgement Sliceは判断を縦断する。

```text
UI
↓
判断材料提示
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

---

# 5. Operational Bridge

## 5.1 定義

Operational Bridgeとは、

> 未実装JPや未実装工程を、現場運用で補完しながら、  
> JDAのハーネスに接続するための暫定的な接続層

である。

JDA実装では、最初からすべての工程をシステム化しない。

未実装部分は、既存運用や軽量ツールで補完し、  
主要JPの運用開始を優先する。

---

## 5.2 具体例

Operational Bridgeの具体例として、以下がある。

- Excel import/export
- CSV連携
- 手動入力
- 既存管理表
- メール運用
- 一時的なスプレッドシート
- 手動確認フロー

---

## 5.3 Excel import/export の位置づけ

Excel import/export は、Operational Bridge の一例である。

これは単なる便利機能ではなく、

```text
未実装JP
未実装工程
既存業務
```

を現場運用で補完するためのブリッジである。

ただし、ExcelはBJ01固有の実装手段であり、  
Method上はOperational Bridgeの具体例として扱う。

---

## 5.4 Operational Bridgeの目的

Operational Bridgeの目的は、  
すべてを作り込むことではない。

目的は、

```text
主要JPを先に現場へ出すこと
```

である。

そのため、Operational Bridgeは一時的であってよい。

ただし、運用を通じて重要性が高いと判断された場合、  
後から正式なJP実行環境または機能として実装する。

---

## 5.5 ハーネスとの接続

Operational Bridgeは、ハーネスを迂回するためのものではない。

Operational Bridgeは、  
未実装JP・未実装工程・既存運用を、  
ハーネスに接続するための暫定的な接続層である。

そのため、Operational Bridgeを使用する場合でも、  
判断は可能な限りハーネス上で実行し、  
State更新・JLog保存・VLog評価へ接続できるようにする。

```text
Operational Bridge
↓
Proposal / input_data
↓
execute_jp
↓
State / JLog / VLog
↓
Operational Bridge
```

Operational Bridgeの目的は、  
JDAの外で業務を完結させることではない。

目的は、既存運用をJDAの実行基盤に接続することである。

---

## 5.6 Bridge in / Bridge out

Operational Bridgeは、以下の2方向で設計する。

---

### Bridge in

既存データ・Excel・CSV・手動入力を、  
Proposal / input_data としてハーネスに入れる。

```text
Excel / CSV / 既存管理表
↓
import
↓
Proposal / input_data
↓
execute_jp
```

---

### Bridge out

execute_jp の結果・State・JLog / VLogを、  
Excel / CSV / 現場運用へ返す。

```text
execute_jp
↓
State / JLog / VLog
↓
export
↓
Excel / CSV / 現場運用
```

---

## 5.7 避けるべきOperational Bridge

以下は避ける。

```text
Excelで判断
Excelで状態更新
Excelで完結
あとでシステムへ転記
```

この形では、JLog / VLog が残らず、  
Learningにつながらない。

Operational Bridgeを使う場合でも、  
判断の実行・状態更新・ログ収集は、  
可能な限りハーネスへ接続する。

---

# 6. 実装順序

## 6.1 Step1：JULIAで選定された主要JPを確認する

最初に、Phase2 JULIAで選定された主要JPを確認する。

Judgement Slice Implementationでは、  
JPの優先順位そのものは決定しない。

優先順位はPhase2 JULIAで決定し、  
本パターンでは、そのJPをどの範囲で最小実装するかを決める。

---

## 6.2 Step2：判断材料を定義する

対象JPに対して、  
人間が判断するために必要な判断材料を定義する。

判断材料には、以下が含まれる。

- データソース
- 条件（Condition）
- 観点（Perspective）
- 過去履歴
- 類似Case
- AI補強情報
- 現場メモ
- 判断理由の入力項目

この段階で重要なのは、  
AIが判断することではなく、  
人間が判断しやすくなる材料を整えることである。

---

## 6.3 Step3：最小実行構造を作る

主要JPを運用可能にするために必要な最小構造を作る。

最低限必要なものは以下である。

- Proposal
- current_state
- execute_jp
- 判断材料提示
- 判断入力
- 判断理由入力
- JLog
- VLog
- UI
- import/export または既存運用との接続

---

## 6.4 Step4：前後工程をOperational Bridgeで補完する

主要JPの前後に未実装工程がある場合、  
すべてをシステム化しない。

まずはOperational Bridgeで補完する。

例：

```text
前工程：Excelから取り込む
主要JP：システム上で判断する
後工程：Excelへ出力して現場運用する
```

この場合でも、主要JPの判断は可能な限りハーネス上で実行し、  
JLog / VLogに接続する。

---

## 6.5 Step5：現場で運用する

小さく実装したら、現場に使ってもらう。

この段階で重要なのは、  
完成度ではなく、JLog / VLog が取れることである。

現場が判断できること、  
判断材料が提示されること、  
判断理由が残ることを優先する。

---

## 6.6 Step6：ログを見る

運用後、以下を観測する。

- どのJPが使われたか
- どのJPで止まったか
- 判断材料は足りたか
- 判断理由は残ったか
- VLogを書けるか
- 現場が入力を嫌がる箇所はどこか
- Operational Bridgeで十分か
- ハーネスに接続できているか

---

## 6.7 Step7：必要な周辺JPの実行環境だけ追加する

ログと運用の違和感から、  
必要な周辺JPの実行環境だけを追加する。

ここでも、判断そのものを作るのではなく、  
JPを運用可能にするための構造を追加する。

追加判断の基準は、

```text
そのJP実行環境を追加すると、
JLog / VLog が増え、
Learningにつながるか
```

である。

---

## 6.8 Step8：JJとして整理する

複数JPが連鎖して動き始めたら、  
それを Judgement Journey（JJ）として整理する。

JJは最初から完全に設計するものではない。

実装と運用の中で、  
JP実行連鎖として形成される。

---

# 7. 実装範囲の判断基準

実装範囲を広げるかどうかは、  
以下の4つで判断する。

---

## 7.1 この機能はJLog / VLogを増やすか？

JDA実装において重要なのは、  
機能数ではなく、判断ログが増えることである。

JLog / VLogが増えない機能は、  
Learningへの貢献が小さい。

---

## 7.2 この機能は主要JPの運用に必要か？

主要JPを現場で回すために必要な機能は実装する。

逆に、主要JPの運用に直接関係しない機能は後回しにする。

---

## 7.3 このJPにVLogを書けるか？

VLogを書けないJPは、  
妥当性評価ができない。

妥当性評価ができなければ、  
Learningに接続しにくい。

そのため、JPの実行環境を追加する場合は、

```text
後から妥当性を評価できるか
```

を確認する。

---

## 7.4 Operational Bridgeで逃がせないか？

正式実装する前に、  
Operational Bridgeで補完できないかを確認する。

Excel、CSV、既存管理表、手動確認で十分な場合は、  
最初から作り込まない。

ただし、Operational Bridgeで逃がす場合でも、  
ハーネスへの接続を意識する。

---

# 8. よくある失敗

## 8.1 最初からBJ全体を作ろうとする

❌ BJ全体を完全実装する  
⭕ 主要JPから始める

---

## 8.2 UIを作り込みすぎる

❌ 画面を整えることが目的になる  
⭕ 判断材料が提示され、判断が実行され、JLogが残ることを優先する

---

## 8.3 前後工程をすべてシステム化する

❌ 全工程を最初から作る  
⭕ Operational Bridgeで補完する

---

## 8.4 JLogだけで満足する

❌ 判断記録だけ残す  
⭕ VLogまで残せる構造にする

---

## 8.5 VLogを書けないJPを追加する

❌ 後から評価できない判断を増やす  
⭕ 妥当性評価できる判断から実装する

---

## 8.6 作るのが楽しくなって広げすぎる

❌ AI実装が速いので、必要以上に作り込む  
⭕ 実装範囲の判断基準に戻る

---

## 8.7 JULIAを無視して作り始める

❌ 作りやすいJPから作る  
⭕ JULIAで選定された主要JPから始める

---

## 8.8 Operational BridgeがJDAの外で完結する

❌ Excelや既存運用だけで判断・状態更新・完結する  
⭕ Operational Bridgeをハーネスに接続する

---

## 8.9 JPそのものをシステムが作ると誤解する

❌ JPを作る = 判断をシステムが作る  
⭕ JPを運用可能にする = 判断材料提示・入力・状態遷移・ログ収集を作る

---

# 9. Phase5との関係

Phase5 Implementation は、  
JDAにおける実行基盤を定義する。

Phase5の主対象は以下である。

- ハーネス
- Judgement Injection
- execute_jp
- Proposal
- State
- JLog / VLog
- JJ形成

一方、Judgement Slice Implementation は、  
その実行基盤を使って、

```text
どの範囲で実装するか
どこまで作るか
未実装工程をどう扱うか
どのように現場運用へ出すか
```

を定義する補助パターンである。

---

## 9.1 位置づけ

```text
06_phase5_implementation.md
↓
実行基盤の定義

06a_judgement_slice_implementation.md
↓
実装の進め方・範囲制御
```

---

## 9.2 関係性

```text
Phase2 JULIA
= どのJPに投資するか

Phase5
= どう実行するか

Implementation Pattern
= どう小さく作り始めるか
```

---

## 9.3 最終方針

JDA実装では、

```text
全部作ってから運用する
```

のではなく、

```text
判断ログが取れる最小構造で先に現場へ出す
```

ことを優先する。

そのために、Judgement Slice Implementation を用いる。

---

# 10. 変更履歴

| Version | 内容 |
|---|---|
| v1.6 | 初版 / Judgement Slice Implementation / Operational Bridge 定義 / JULIAとの責務分離 / ハーネス接続 / JP実行環境の明確化 |
