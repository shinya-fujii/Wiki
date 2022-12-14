# Git 基本的な CLI 操作

## 目次

---

- [トランクベース開発の流れ](#トランクベース開発の流れ)
- [基本操作](#基本操作)

## 参考

---

[git diff で作ったパッチはどうやって当てるのか](#https://beyondjapan.com/blog/2019/08/git-diff/)

<br>([目次へ](#目次))
<br>

## トランクベース開発の流れ

---

1. 差分確認
   修正内容(ファイル)を確認する

   ```
   git status

   # エイリアスを設定している場合
   git st
   ```

1. リモートとの差分をなくす

   ```
   # 基本
   git pull origin {ブランチ名}

   # 省略系(ローカルの作業ブランチと紐づく、リモートブランチが設定されてたら省略できる)
   git pull

   # fetch後にマージではなくリベースする
   git pull --rebase
   ```

1. リモートとのファイルの差分を確認
   リモートとの差分を取り込んだことでの差分を確認。

   ```
   git status
   ```

1. 修正したファイルの変更点を確認

   ```
   git deff

   git deff --cached
   ```

1. ステージング
   特に falcon の方は不要なファイルが差分として現れるためディレクトリを指定する

   ```
   # ステージングするディレクトリの指定
   git add {ディレクトリ名}
   git add /hoge

   # 変更点をすべて含む場合
   git add .
   ```

1. コミットする
   ```
   コミットメッセージは特に決まりはない。
   git comit -m "{チーム名}：{修正内容}"
   ```
1. プッシュする前に自分の修正分以外に差分やコンフリクトがないか確認

   ```
   # リベースをしている。
   git pull -r

   # 手元に作業ファイルがある場合にオートスタッシュしている
   git pull -r --au
   ```

1. プッシュ
   ```
   git push
   ```

<br>([目次へ](#目次))
<br>

## 基本操作

---

### diff の扱い方

ペアプロの開発では途中で作業をしたままにすると、
途中の状態を共有できないので、作業途中の差分をファイルとして共有する

##### 差分を出力

```
git diff --cached
```

##### パッチファイルとして出力

```
git diff --cached > {ファイル名}

git diff > .patch
```

##### パッチファイル取り込み

```
git apply test.patch
```

<br>([目次へ](#目次))
<br>
