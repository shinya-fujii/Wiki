# 【Laravel でのテストコードの書き方】 - API -

---

### 参考リンク

<!-- [参考タイトル](参考リンク) -->

- [Laravel で俺が使いそうな Assert メソッドをまとめてみた](https://qiita.com/taku-0728/items/8cc6bb2ce9ec54168686)

---

### 目次

<!-- - [ページ内タイトル](#ページ内リンク) -->

- [ディレクトリ構成](#ディレクトリ構成)
- [ファイル作成](#ファイル作成)
- [Http リクエストのシュミレーション](#Httpリクエストのシュミレーション)
- [暗黙の型変換を許可しない](#暗黙の型変換を許可しない)
- [ミドルウェアの無効化](#ミドルウェアの無効化)
- [サンプル](#サンプル)
- [アサーションメソッド一覧](#アサーションメソッド一覧)
  - [レスポンス](#レスポンス)
  - [セッション](#セッション)
  - [認証](#認証)
  - [ブラウザ](#ブラウザ)
  - [データベース](#データベース)

---

### ディレクトリ構成

- Laravel にはデフォルトでテスト用のディレクトリが用意されている。

  `Unit` : 関数、クラス単位のテスト
  `Feature` : 機能単位のテスト
  `TestCase`: テスト基底クラス

テストを実行する`php artisan test`コマンドでは
ユニットテストクラスでは Laravel の機能は使わない。
フィーチャーテストクラスでは Laravel の機能を使ったテストになる。

```
tests
|---Feature
|   |---ExampleTest.php
|
|--Unit
|    |---ExampleTest.php
|
|---CreateApplication.php
|---TestCase.php
```

<br>[【目次へ】](#目次)

---

### ファイル作成

- 下記の Api ディレクトリ内にファイルを作成する

```
# フィーチャーテストファイル作成
php artisan make:test HelloIndexPageTest

# ユニットテストファイル作成
php artisan make:test HelloIndexPageTest --unit
```

<br>[【目次へ】](#目次)

---

### Http リクエストのシュミレーション

call メソッドを使うことで、擬似的なリクエストを送ることができる。
各引数には以下の値を受け取るようになっている。

```
public functioin call(
    $method,            // HTTPメソッド
    $uri,               // URI
    $parameter = [],    // 送信するパラメーター
    $cookie = [],       // Cookie
    $files = [],        // アップロードファイル
    $server = [],       // サーバーパラメーター
    $content = null,    // RAWリクエストボディ
);
```

- GET

```
$response = $this->call('GET', 'api/v1/get', [
    'class' => 'momo',
    'no' => '99',
]);
```

- POST

```
$response = $this->call('POST', 'api/v1/get', [
    'email' => 'momo@test.com',
    'password' => 'aoigjoaiugoa',
]);
```

- 同様に json メソッドを用いて json 形式でリクエストを送ることもできる。

```
$response = $this->getJson('GET', 'api/v1/get');
```

<br>[【目次へ】](#目次)

---

### 暗黙の型変換を許可しない

- declare(strict_types=1); で PHP の strict モードの設定をする

テストをする時はファイルの先頭に以下の記載をする。

PHP はデフォルトで自動的に型変換をしてくれるが、
この設定をすることで型を厳格にチェックしてくれる。

```
declare(strict_types=1);
```

<br>[【目次へ】](#目次)

---

### ミドルウェアの無効化

- HTTP リクエスト送信時にルーティングなどで設定されているミドルウェアを無効化することができる。

```
$this->withoutMiddleware(TeaPotMiddleware::class)
    ->getJson('api/live)
```

上記の場合は指定したミドルウェアを無効にするが、
全てのミドルウェアを無効にする場合は引数なしで実行するか
`use Illuminate\Foundation\Testing\WithoutMiddleware` のようにトレイトを use する。

<br>[【目次へ】](#目次)

---

### サンプル

・アサーションメソッドはチェーンできる

```
public function testMustEnterEmailAndPassword()
{
    $this->json('POST', 'api/login')
        ->assertStatus(422)
        ->assertJson([
            "message" => "The given data was invalid.",
            "errors" => [
                'email' => ["The email field is required."],
                'password' => ["The password field is required."],
            ]
        ]);
}
```

<br>[【目次へ】](#目次)

---

## アサーションメソッド一覧

#### レスポンス

| メソッド            | 意味                                                             |
| ------------------- | ---------------------------------------------------------------- |
| assertCookie        | レスポンスが指定したクッキーを持っている                         |
| assertCookieMissing | レスポンスが指定したクッキーを持っていない                       |
| assertDontSee       | 指定した文字列がレスポンスに含まれていない                       |
| assertSee           | 指定した文字列がレスポンスに含まれている                         |
| assertDontSeeText   | 指定した文字列がレスポンステキストに含まれていない               |
| assertSeeText       | 指定した文字列がレスポンステキストに含まれている                 |
| assertForbidden     | レスポンスが forbidden ステータスコードを持っている              |
| assertHeader        | レスポンスに指定したヘッダが存在している                         |
| assertHeaderMissing | レスポンスに指定したヘッダが存在していない                       |
| assertJson          | レスポンスが指定した JSON データを持っている                     |
| assertJsonCount     | レスポンス JSON が、指定したキーのアイテムを、期待値分持っている |
| assertJsonFragment  | レスポンスが指定した JSON の一部を含んでいる                     |
| assertJsonMissing   | レスポンスが指定した JSON の一部を含んでいない                   |
| assertJsonStructure | レスポンスが指定の JSON 構造を持っている                         |
| assertLocation      | レスポンスの Location ヘッダが、指定した URI を持つ              |
| assertNotFound      | レスポンスが not found のステータスコードを持っている            |
| assertOk            | レスポンスが 200 のステータスコードを持っている                  |
| assertRedirect      | クライアントが指定した URI へリダイレクトする                    |
| assertStatus        | クライアントのレスポンスが指定したコードである                   |
| assertSuccessful    | レスポンスが成功のステータスコードである                         |
| assertViewHas       | レスポンスビューが指定したデータを持っている                     |
| assertViewHasAll    | レスポンスビューが指定したリストのデータを持っている             |
| assertViewIs        | ルートにより、指定したビューが返される                           |
| assertViewMissing   | レスポンスビューが指定したデータを持っていない                   |

#### セッション

| メソッド                 | 意味                                                     |
| ------------------------ | -------------------------------------------------------- |
| assertSessionHas         | セッションが指定したデータを持っている                   |
| assertSessionHasAll      | セッションが指定したリストの値を持っている               |
| assertSessionHasErrors   | セッションが指定したフィールドに対するエラーを含んでいる |
| assertSessionHasErrorsIn | セッションが指定したエラーを持っている                   |
| assertSessionHasNoErrors | セッションがエラーを持っていない                         |
| assertSessionMissing     | セッションが指定したキーを持っていない                   |

#### 認証

| メソッド                 | 意味                             |
| ------------------------ | -------------------------------- |
| assertAuthenticated      | ユーザーが認証されている         |
| assertGuest              | ユーザーが認証されていない       |
| assertAuthenticatedAs    | 指定したユーザーが認証されている |
| assertCredentials        | 指定した認証情報が有効である     |
| assertInvalidCredentials | 指定した認証情報が無効である     |

#### ブラウザ

| メソッド                 | 意味                                                          |
| ------------------------ | ------------------------------------------------------------- |
| assertTitle              | ページタイトルが指定した文字列と一致する                      |
| assertTitleContains      | ページタイトルに、指定したテキストが含まれている              |
| assertUrlIs              | クエリ文字列を除いた、現在の URL が指定した文字列と一致する   |
| assertHostIs             | 現在の URL のホストが、指定したホストと一致する               |
| assertHostIsNot          | 現在の URL のホストが、指定したホストと一致しない             |
| assertPortIs             | 現在の URL ポートが、指定したポートと一致する                 |
| assertPortIsNot          | 現在の URL ポートが、指定したポートと一致しない               |
| assertPathBeginsWith     | 現在の URL パスが指定したパスで始まる                         |
| assertPathIs             | 現在のパスが指定したパスである                                |
| assertPathIsNot          | 現在のパスが指定したパスではない                              |
| assertRouteIs            | 現在の URL が指定した名前付きルートの URL と一致する          |
| assertQueryStringHas     | 指定したクエリ文字列パラメータが存在している                  |
| assertQueryStringMissing | 指定した文字列パラメータが存在しない                          |
| assertInputValue         | 指定した入力フィールドが、指定値を持っている                  |
| assertInputValueIsNot    | 指定した入力フィールドが、指定値を持っていない                |
| assertChecked            | 指定したチェックボックスが、チェック済みである                |
| assertNotChecked         | 指定したチェックボックスが、チェックされていない              |
| assertRadioSelected      | 指定したラジオフィールドが選択されている                      |
| assertRadioNotSelected   | 指定したラジオフィールドが選択されていない                    |
| assertSelected           | 指定したドロップダウンで指定値が選択されている                |
| assertNotSelected        | 指定したドロップダウンで指定値が選択されていない              |
| assertDialogOpened       | 指定したメッセージを持つ、JavaScript ダイアログが開かれている |
| assertEnabled            | 指定したフィールドが、enabled である                          |
| assertDisabled           | 指定したフィールドが、disabled である                         |
| assertFocused            | 指定したフィールドに、フォーカスがある                        |
| assertNotFocused         | 指定したフィールドから、フォーカスが外れている                |

#### データベース

| メソッド              | 意味                                   |
| --------------------- | -------------------------------------- |
| assertDatabaseHas     | 指定したデータが、テーブルに存在する   |
| assertDatabaseMissing | 指定したデータが、テーブルに含まれない |
| assertSoftDeleted     | 指定したデータが、論理削除されている   |

---

<br>[【目次へ】](#目次)
