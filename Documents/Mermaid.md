# Mermaid 記法について

---

### 参考リンク

<!-- [参考タイトル](参考リンク) -->

- [チートシート](https://jojozhuang.github.io/tutorial/mermaid-cheat-sheet/)
- [mermaid.js の記法を覚えて、楽しく図を描く。](https://qiita.com/t_o_d/items/ac5b04419252f768a535)

---

### 目次

<!-- - [ページ内タイトル](#ページ内リンク) -->

- [基本的な記法について](#基本的な記法について)
- [図の種類](#図の種類)
- [ノードの記法](#ノードの記法)
- [リレーションの記法](#リレーションの記法)
- [サブグラフの作成](#サブグラフの作成)
- [スタイル・アクション](#スタイルアクション)
- [シーケンス図の作成](#シーケンス図の作成)

---

### 基本的な記法について

- コードブロックを表す` 「```」 `の後に`mermaid` とつけることで
  マーメイド記法を利用することができる

```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;

%% this is a comment

```

- コメントは`%%`の後に記載

```mermaid
%% this is a comment

graph LR;
    A-->B;
```

<br>[【目次へ】](#目次)

---

### 図の種類

| コード | 種類       | 表示                     |
| ------ | ---------- | ------------------------ |
| TB     | TOP→BOTTOM | mermaid graph TB; A-->B; |

```mermaid
graph TB;
    A-->B;
```

| コード | 種類       | 表示                     |
| ------ | ---------- | ------------------------ |
| TD     | TOP→BOTTOM | mermaid graph TD; A-->B; |

```mermaid
graph TD;
    A-->B;
```

| コード | 種類       | 表示                     |
| ------ | ---------- | ------------------------ |
| BT     | BOTTOM→TOP | mermaid graph BT; A-->B; |

```mermaid
graph BT;
    A-->B;
```

| コード | 種類       | 表示                     |
| ------ | ---------- | ------------------------ |
| RL     | RIGHT→LEFT | mermaid graph RL; A-->B; |

```mermaid
graph RL;
    A-->B;
```

| コード | 種類       | 表示                     |
| ------ | ---------- | ------------------------ |
| LR     | LEFT→RIGHT | mermaid graph LR; A-->B; |

```mermaid
graph LR;
    A-->B;
```

<br>[【目次へ】](#目次)

---

### ノードの記法

```mermaid
graph TD;
    id;
    id1[通常のテキスト]
    id2(角がまるくなる)
    id3((円形))
```

```mermaid
graph TD;
    id4([楕円])
    id5>フラッグ型]
    id6{菱型}
    id7{{六角型}}
    id8[(円柱)]
```

<br>[【目次へ】](#目次)

---

### リレーションの記法

```mermaid
graph TD;
    A-->B;
    C---D;
    E-- テキストA ---F;
    G---|テキストB|H;
    E-->| テキストA |F;
    G--テキストB-->H;

    I-.->J;
    K-.-L;
    M-.テキスト.-N;
    O==>P;
    Q==テキスト==>R;
```

<br>[【目次へ】](#目次)

---

### サブグラフの作成

```mermaid
graph TB
    c1-->a2
    subgraph one
    a1-->a2
    end
    subgraph two
    b1-->b2
    end
    subgraph three
    c1-->c2
    end
```

<br>[【目次へ】](#目次)

---

### シーケンス図の作成

```mermaid
sequenceDiagram
    participant Alice
    participant John
    Alice->>John: Hello John, how are you?
    John-->>Alice: Great!
```

- **アクターの出現順序を指定して、参加者を別の順序で表示できる。**

```mermaid
sequenceDiagram
    participant John
    participant Alice
    Alice->>John: Hello John, how are you?
    John-->>Alice: Great!
```

- **エイリアスの作成**

```mermaid
sequenceDiagram
    participant A as Alice
    participant J as John
    A->>J: Hello John, how are you?
    J-->>A: Great!
```

#### テキスト

- 右側

```mermaid
sequenceDiagram
    participant John
    Note right of John: Text in note
```

- 左側

```mermaid
sequenceDiagram
    participant John
    Note left of John: Text in note
```

- 横断

```mermaid
sequenceDiagram
    participant John
    Note over John: Text in note
```

<br>[【目次へ】](#目次)

---

### スタイル・アクション

- **クリックでのイベント**

```mermaid
graph LR
    A-->Google
    click A callback "クリック"
    click Google "https://www.google.co.jp/" "リンク指定"
```

- **スタイルの変更**

```mermaid
graph LR
    start(開始)-->stop(完了)
    style start fill:#f9f,stroke:#333,stroke-width:4px
    style stop fill:#ccf,stroke:#f66,stroke-width:2px,stroke-dasharray: 5, 5
```

<br>[【目次へ】](#目次)

---

### ガントチャート

```mermaid
gantt
    title A Gantt Diagram
    dateFormat  YYYY-MM-DD
    section Section
    First Task       :a1, 2018-07-01, 30d
    Another Task     :after a1, 20d
    section Another
    Second Task      :2018-07-12, 12d
    Third Task       : 24d
```

```mermaid
gantt
    dateFormat  YYYY-MM-DD
       title Adding GANTT diagram functionality to mermaid

       section A section
       Completed task            :done,    des1, 2018-01-06,2018-01-08
       Active task               :active,  des2, 2018-01-09, 3d
       Future task               :         des3, after des2, 5d
       Future task2              :         des4, after des3, 5d

       section Critical tasks
       Completed task in the critical line :crit, done, 2018-01-06,24h
       Implement parser and jison          :crit, done, after des1, 2d
       Create tests for parser             :crit, active, 3d
       Future task in critical line        :crit, 5d
       Create tests for renderer           :2d
       Add to mermaid                      :1d

       section Documentation
       Describe gantt syntax               :active, a1, after des1, 3d
       Add gantt diagram to demo page      :after a1  , 20h
       Add another diagram to demo page    :doc1, after a1  , 48h

       section Last section
       Describe gantt syntax               :after doc1, 3d
       Add gantt diagram to demo page      :20h
       Add another diagram to demo page    :48h
```

<br>[【目次へ】](#目次)

---
