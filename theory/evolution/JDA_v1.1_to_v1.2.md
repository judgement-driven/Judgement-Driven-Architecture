# JDA v1.1 → v1.2 変更経緯

---

## TYPE

Research Note

---

## PROJECT

JDA Core Evolution

---

## TOPIC

JDA v1.1からv1.2への構造定義の変更

---

## DATE

2026-04-03

---

## 背景

JDA v1.1では判断構造を以下のように定義していた。

```text
Judgement Chain
→ Judgement Journey
→ Judgement Point
```

実証（文化アディックへの適用）を進める中で、
この構造に対して複数の違和感が発生した。

---

## 問題意識

### 1. Judgement JourneyとBusiness Journeyの混在

- 業務単位（Business Journey）と  
  判断単位（Judgement Journey）が  
  同じ「Journey」という言葉で扱われていた  

- 「J01」という命名が  
  BJなのかJJなのか曖昧であった  

---

### 2. 構造の一貫性の欠如

- Judgement Chain（判断）の下に  
  Business Journey（業務）が来る構造は  
  概念として矛盾していた  

- 判断の構造の中に業務概念が混入していた  

---

### 3. JP抽出方法の未定義

- JPが「顧客状態 × 企業行動」の交点で  
  発生するという理解はあった  

- しかし、それがmethodとして  
  明示されていなかった  

---

### 4. Chain定義の混乱

- Chainの定義が不明確であった  

- 実際の実証では、Chainを定義する前に  
  Business Journey（BJ）を先に作成していた  

- その結果、

```text
Chain → Journey → JP
```

という構造に対して強い違和感が発生した  

- 特に、

「Chainが判断の連鎖なのに、その下に業務（BJ）が来るのはおかしいのではないか？」

という根本的な疑問が生まれた  

- この違和感は、Chainを「判断構造」として捉える一方で、  
  BJを「業務構造」として扱っていたことによる概念の混在に起因する  

---

## 議論の経緯

実証を進める中で、以下の順序で議論が深まった。

1. JDA-BMCでBusiness Journeyを列挙  
2. Chain定義が曖昧なままJ01のDiscoveryを開始  
3. 顧客ジャーニー × 企業ジャーニーの重ね合わせ図を設計  
4. 「ChainはJudgementなのに、その下がBusinessなのは矛盾では？」という問題意識が明確化  
5. 「Chain → Journey → JP」という直列構造への違和感が顕在化  
6. BJ（業務単位）とJJ（判断単位）を分離する方針に決定  

---

## 結論

以下の定義に整理した。

---

### Business Journey（BJ）

- 業務単位のまとまり  
- Phase0 Foundationで使用  
- 例：BJ01 新規クライアント獲得  

---

### Judgement Journey（JJ）

- 判断単位の連鎖  
- Phase1 Discovery以降で使用  
- 例：JJ01 アタリ判断の連鎖  

---

### 関係性

```text
BJ（業務単位）
↓ 再解釈
JJ（判断単位）
```

- BJは業務構造として定義される  
- Discovery以降で判断構造として再解釈される  

---

## v1.2での変更点

- core 5章「判断構造」の定義更新  
- methodでBJ/JJの使い分けを明示  
- phase0でBJ使用を明示  
- phase1でBJをJJに再解釈するステップを追加  
- phase2以降でJJ使用に統一  

---

## 意義

本変更はバグ修正ではなく理論の補強である。

実証を通じて概念の精緻化が行われた。

特に以下の点が重要である：

- Chainの定義が曖昧なまま進めると構造矛盾が発生する  
- 業務構造（BJ）と判断構造（JJ）は分離すべきである  
- Discoveryは「業務を判断に変換する工程」である  

これはJDAの以下の原則に従った進化である。

- 実装しながら学習する  
- 判断構造を現場から抽出する  
- 理論は固定ではなく更新される  

---

## 補足

本変更により、以下が明確になった。

- 業務（Business）と判断（Judgement）の分離  
- Journeyという言葉の多義性の解消  
- JP抽出の起点（顧客状態 × 企業行動）の明文化  
- Discoveryフェーズの役割の明確化  
- Chainは前提ではなく「再解釈の結果として明確になる」可能性がある  

---

### Judgement Chainの再定義

v1.2において、Judgement Chainの位置づけを以下に変更する。

**v1.1までの定義**
- Chainは判断構造の起点として明示的に設計するもの

**v1.2での再定義**
- ChainはBJを並べた結果として自然に見えるもの
- 明示的に設計するステップは不要
- BJとJJが揃った時点で可視化される

```text
BJ01 → BJ02 → BJ03 → ...
↓
この並びがChainとして見える
```

**名称について**
- 「Chain（連鎖）」という名称は維持する
- 直感的に意味が伝わる
- JDAの概念として定着している

**結論**
ChainはDiscoveryの入力ではなく
Discoveryの結果として現れる概念である。

---