# 正規表現

---

### 参考リンク

<!-- [参考タイトル](参考リンク) -->

- [基本的な正規表現一覧](https://murashun.jp/article/programming/regular-expression.html)
- [入力チェック（正規表現含む）](https://phpjavascriptroom.com/?t=js&p=formcheck)

---

### 目次

<!-- - [ページ内タイトル](#ページ内リンク) -->

- [使用頻度の高い正規表現式](#使用頻度の高い正規表現式)
  - [空白](#空白)
  - [Email アドレス](#email-アドレス)
  - [パスワード](#パスワード)
  - [URL](#url)
  - [ドメイン名](#ドメイン名)
  - [固定電話番号](#固定電話番号)
  - [携帯電話番号](#携帯電話番号)
  - [IP 電話番号](#ip-電話番号)
  - [フリーダイヤル](#フリーダイヤル)
  - [日付](#日付)
  - [郵便番号](#郵便番号)

---

### 使用頻度の高い正規表現式

---

##### 空白

・空白、スペース、改行に一致
`/\s/`

・空白以外
`/\S/`

##### Email アドレス

`^\w+([-+.]\w+)_@\w+([-.]\w+)_\.\w+([-.]\w+)\*$`

/^[a-zA-Z0-9_+-]+(\.[a-zA-Z0-9_+-]+)_@([a-zA-Z0-9][a-za-z0-9-]_[a-zA-Z0-9]\*\.)+[a-zA-Z]{2,}$/

##### パスワード

・8 文字以上 30 文字以内の英数字
【PHP】
`/\A[a-z\d]{8,30}+\z/i`

【JS】
`/^[a-z\d]{8,30}$/i`

・半角英字,半角記号それぞれ 1 種類づつ含む、8 文字以上 30 文字以内

【PHP】
`/\A(?=.*?[a-z])(?=.*?\d)[a-z\d]{8,30}+\z/i`

【JS】
`/^(?=._?[a-z])(?=._?\d)[a-z\d]{8,30}$/i`

・半角英数字、半角記号それぞれ１種類ずつ

【PHP】
`/\A(?=\d{0,99}+[a-z])(?=[a-z]{0,99}+\d)[a-z\d]{8,100}+\z/i`

【JS】
`/^(?=\d{0,99}[a-z])(?=[a-z]{0,99}\d)[a-z\d]{8,300}$/i`

・半角英数字、数字それぞれ１種類ずつ

【JS】
^(?=._?[a-zA-Z])(?=._?\d)[a-zA-Z\d]{8,}$

##### URL

`^https?://([\w-]+\.)+[\w-]+(/[\w-./?%&=]\*)?$`

##### ドメイン名

`^[a-zA-Z0-9][a-zA-Z0-9-]{1,61}[a-zA-Z0-9]\.[a-zA-Z-]{2,}$`

##### 固定電話番号

`^0\d(-\d{4}|\d-\d{3}|\d\d-\d\d|\d{3}-\d)-\d{4}$`

##### 携帯電話番号

`^0[789]0-\d{4}-\d{4}$`

##### IP 電話番号

`^050-\d{4}-\d{4}$`

##### フリーダイヤル

`^(0120|0800)-\d{3}-\d{3}$`

##### 日付

- (YYYY-MM-DD 形式)
  `^\d{4}-\d\d-\d\d$`

##### 郵便番号

`^\d{3}-\d{4}$`

<br>[【目次へ】](#目次)

---

### コンテンツ２

```

```

---

<br>[【目次へ】](#目次)

### コンテンツ３

```

```

<br>[【目次へ】](#目次)
