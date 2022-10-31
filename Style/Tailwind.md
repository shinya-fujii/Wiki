# 【TailwindCSS の導入】

---

### 参考

- [Tailwind CSS 公式](https://tailwindcss.com/docs/installation)
- ☆[Next.js に Tailwind CSS を導入してみたので作業メモ](https://www.ikkitang1211.site/entry/2020/10/18/211011)
- [Tailwind CSS の設定と実装方法 – JIT も解説](https://yoheiko.com/?p=2318)
- [【Tailwind 和訳】CORE CONCEPTS/Hover, Focus, & Other States](https://zenn.dev/unreact/articles/tailwindcss-core-concepts-hover-focus-other-states)
- [Tailwind on Rails](https://qiita.com/d0ne1s/items/5a2cf50167afd988e9f7)

---

### 目次

- [導入するメリット](#導入するメリット)
- [TaliwindCSS インストール](#taliwindcss-インストール)
- [Tailwind 設定ファイル作成](#tailwind-設定ファイル作成)
- [PurgeCSS で不要な CSS を削除する設定をする](#purgecss-で不要な-css-を削除する設定をする)
- [背景色のカスタム](#背景色のカスタム)
- [固定値を使いたい場合(jit モード)](#固定値を使いたい場合)
- [postcss の使い方](#postcss-の使い方)
- [tailwind.css を読み込み](#tailwindcss-を読み込み)
- [\_app.js の修正](#_appjs-の修正)
- [フォーマッターの導入](#フォーマッターの導入)
- [アニメーションのカスタマイズ](#アニメーションのカスタマイズ)
- [よく使うプロパティをまとめる](#よく使うプロパティをまとめる)
- [よく使うプロパティ](#よく使うプロパティ)

<br>[【目次へ】](#目次)

---

### 導入するメリット

**_TailwindCSS を導入することで以下のようなメリットがある_**

- クラス名を考える必要がなくなる
- クラス名の衝突がなくなり、BEM や CSS の設計から開放される
- デザインの修正による不要な CSS が残らなくなる
- どの要素にどのスタイルが当たっているか一眼でわかる
- 各要素毎のレスポンシブなレイアウト調整が簡単にできる

これだけでも十分使う価値がある。

<br>[【目次へ】](#目次)

---

### TaliwindCSS インストール

```
npm install -D tailwindcss
```

<br>[【目次へ】](#目次)

---

### Tailwind 設定ファイル作成

tailwind.config.js を作成

```
npx tailwindcss init
```

<br>[【目次へ】](#目次)

---

### PurgeCSS で不要な CSS を削除する設定をする

PurgeCSS はコンテンツから使われていない CSS を削除するための PostCSS 製のツール。
TailwindCSS だけでファイルサイズが大きいので、ビルド対象に含めるのは必要なものだけにする。

```
purge: ['./components/**/*.jsx', './pages/**/*.jsx'],
```

**上記の設定の場合は components か pages 配下に記述されているプロパティしか Tailwind から読み込まない<br>
※util 内やヘルパー関数などで生成したクラス名だと、
定義していないスタイルとして CSS を削除されるためスタイルが効かなくなる**

```
module.exports = {
  purge: ['./components/**/*.jsx', './pages/**/*.jsx'], <= 追記
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
```

theme でカスタムでスタイルも設定できる
下記の記述で bg-main とクラス名をつければ指定したカラーに設定できる
文字色などのサイト全体で統一したい場合は global.css などに記載した方が良い。

### 背景色のカスタム

```
theme: {
    backgroundColor: theme => ({
      'main'  : '#4DBA99',
      'sub'   : '#D1FFE1',
      'base'  : '#92F1D5',
      'accent': '#FF9900',
      'text'  : '#212e46',
    }),
    extend: {},
  },
```

---

### 固定値を使いたい場合

- [tailwindcss の jit モード覚書](https://zenn.dev/k_neko3/articles/7cda6d3ebcd437)

  tailwindCSS は基本的に em 単位の相対的なサイズになる。
  固定値(px)指定したい場合は設定で mode を jit にしないといけない。

```
# 固定値(px)のでの指定方法
class="w-[30px]"
```

```
module.exports = {
  mode: "jit",
  purge: ['./components/**/*.jsx', './pages/**/*.jsx'],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
```

変更しても CSS が反映されない場合は、再度ビルドをやり直した方がいいかもしれない。

<br>[【目次へ】](#目次)

---

### postcss の使い方

postcss は JS のプラグインで CSS を変換するツール
■[PostCSS まとめ](https://qiita.com/morishitter/items/4a04eb144abf49f41d7d)

- **postcss-preset-env をインストール**

```
 npm i -D postcss-preset-env
```

- **postcss.config.js を作成 して記述**

```
# 作成コマンド
touch postcss.config.js
```

```
module.exports = {
  plugins: ['tailwindcss', 'postcss-preset-env'],
}
```

<br>[【目次へ】](#目次)

---

### tailwind.css を読み込み

ユーザーエージェントのブラウザごとにデフォルトで当たるスタイルを無効化する必要がある。
読み込みの記述はその後に記述する。

```
# 作成コマンド
touch styles/tailwind.css
```

```
# 内容
@tailwind base;
@tailwind components;
@tailwind utilities;
```

TailwindCSS を読み込むファイルを修正すると、ビルドに時間がかかる

<br>[【目次へ】](#目次)

---

### \_app.js の修正

全部のファイルで適用する css ファイルを修正

```
import '../styles/globals.css' => import '../styles/tailwind.css';
```

<br>[【目次へ】](#目次)

---

### フォーマッターの導入

- 参考リンク
  [TailwindCSS で Prettier のプラグインを導入し、クラスの自動整形を行う](https://qiita.com/kyo212/items/94d7c5426fe264395c27)

プラグインの prettier の拡張機能で、tailwind のクラス名を自動で整形してくれる。

```
npm install -D prettier prettier-plugin-tailwindcss
```

<br>[【目次へ】](#目次)

---

### Tailwind CSS に定義がないプロパティの設定

【参考】[tailwindcss のプラグインを使って、文字に影をつける](https://qiita.com/oedkty/items/067434af65a5d121742d)

```
# /tailwind.config.json

module.exports = {
  theme: {
    extend: {}
  },
  variants: {},
  plugins: [
    function({ addUtilities }) {
      const newUtilities = {
        ".text-shadow": {
          textShadow: "0px 2px 3px darkgrey"
        },
        ".text-shadow-md": {
          textShadow: "0px 3px 3px darkgrey"
        },
        ".text-shadow-lg": {
          textShadow: "0px 5px 3px darkgrey"
        },
        ".text-shadow-xl": {
          textShadow: "0px 7px 3px darkgrey"
        },
        ".text-shadow-2xl": {
          textShadow: "0px 10px 3px darkgrey"
        },
        ".text-shadow-none": {
          textShadow: "none"
        }
      };

      addUtilities(newUtilities);
    }
  ]
};
```

<br>[【目次へ】](#目次)

---

### アニメーションのカスタマイズ

- **tailwind.config.js で継承させる**

```
  module.exports = {
    theme: {
      extend: {
        animation: {
         'spin-slow': 'spin 3s linear infinite',
        }
      }
    }
  }
```

`keyframes`セクションでアニメーションを自作し、`animation`で指定できるようになる

```
  module.extend: {
    animation: {
        wiggle: 'wiggle 1s ease-in-out infinite',
      }
    keyframes: {
      wiggle: {
        '0%, 100%': { transform: 'rotate(-3deg)' },
        '50%': { transform: 'rotate(3deg)' },
      }
    }
```

<br>[【目次へ】](#目次)

---

### よく使うプロパティをまとめる

@apply を使ってまとめる

```
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
.btn {
  @apply font-bold py-2 px-4 rounded bg-red-500 text-white hover:bg-red-700;
  width: 300px;
}
```

特定のクラスでプロパティをまとめて利用できる

```
<a href='#' class='btn'>ボタン</btn>
```

<br>[【目次へ】](#目次)

---

### よく使うプロパティ

```
padding -> p
margin  -> m　
width   -> w
height  -> h

例
padding:10px;     -> p-[10px]
padding-left:10%; -> pl-[10%]
margin:10px;      -> m-[10px]
margin-left:10%;  -> ml-[10%]
width:100px       -> w-[100px]
width:100%        -> w-full か w-[100%]
height:2em        -> h-[2em]
height:100vh      -> h-[100vh]

font-size:100px;  -> text-[100px]
font-family:bold; -> font-bold

color:red;     -> text-red-500
color:#92400E; -> text-[#92400E]

background-color:red;    -> bg-red-500
background-color:#92400E -> bg-[#92400E]

border:2px solid black; -> border-2 border-black border-solid
border-radius:100px;    -> rounded-[100px]

text-align:center; -> text-center
text-align:left    -> text-left

float:left;  -> float-left
float:right; -> float-right

opacity:0.1; -> opacity-10
opacity:1;   -> opacity:100
opacity:0.54 -> opacity-[54%]

display:none;         -> hidden
display:block;        -> block
display:inline;       -> inline
display:inline-block; -> inline-block
display:flex;         -> flex
display:grid;         -> grid

position:relative; ->relative
position:absolute; -> absolute
position:fixed;    -> fixed
position:static;   -> static
```

<br>[【目次へ】](#目次)
