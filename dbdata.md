---
sort: 2
---

# 本章でやること

ここでは、[データベースの利用](https://2025web1.github.io/07-database/) で学んだプログラムをオブジェクト指向を利用したコードに書き換えます。

既知のプログラムをオブジェクト指向に書き換えることで、以下の考え方を理解することが目的です。

- クラスによる「責任の分離」の理解
  - `dbdata.php` ・・・データベースの接続やSQL文の実行を担当
  - `dbphp.php` ・・・データベースの操作を担当
- インスタンス化の意義
  - `new` によってオブジェクトを作成することで、「クラスの設計図」をもとに「インスタンス（オブジェクト）という実態」を作成することができる
  - プログラムを手順ではなく、モノ（オブジェクト）の作成と操作として捉えることができる
- 再利用性と拡張性の感覚を得る
  - クラスを用いたり、継承したりすることでコードの再利用性が高まり、機能追加がしやすい構造であることを理解する
  
## クラス構成

データベースの基本事項に関するクラス `DbData` とそれを継承して実際にデータを操作するクラス `DbPhp` を定義します。
なお、PHPのクラスは **１つのPHPファイルに１つのPHPクラスの定義を記述する** というルールが一般的であり、これに準じて記述していきます。

これから作成する2つのファイル`dbdata.php`、`dbphp.php`については、`public`ディレクトリ内に、`classes`というディレクトリを作成し、その中に作成していってください。

それではまず、クラス `DbData` を定義するPHPファイル `dbdata.php` を以下に示します。

## DbDataクラス

**classes/dbdata.php**

```php
<?php
// DbDataクラスの宣言 ・・・①
class DbData
{
  // PDOオブジェクト用のプロパティ(メンバ変数)の宣言 ・・・②
  protected $pdo;

  // コンストラクタ
  // 「__construct」の「̲̲__」は「_(アンダースコア)」を2つ記述する ・・・③
  public function __construct()
  {
    // PDOオブジェクトを生成する
    $user = 'sampleuser';
    $password = 'samplepass';
    $host = 'db';
    $dbName = 'SAMPLE';
    $dsn = 'mysql:host=' . $host . ';dbname=' . $dbName . ';charset=utf8';
    try {
      $this->pdo = new PDO($dsn, $user, $password); // ④
    } catch (Exception $e) {
      // 接続できなかった場合のエラーメッセージ
      exit('データベースに接続できませんでした：' . $e->getMessage());
    }
  }

  // SELECT文実行用のquery( )メソッド ・・・このメソッドはユーザー定義関数
  protected function query($sql, $array_params) // ⑤
  {
    $stmt = $this->pdo->prepare($sql);
    $stmt->execute($array_params);
    // PDOステートメントオブジェクトを返すので
    // 呼び出し側でfetch( )、またはfetchAll( )で結果セットを取得
    return $stmt;
  }

  // INSERT、UPDATE、DELETE文実行用のメソッド ・・・このメソッドもユーザー定義関数
  protected function exec($sql, $array_params) // ⑥
  {
    $stmt = $this->pdo->prepare($sql);
    // 成功:true、失敗:false
    $stmt->execute($array_params);
  }
}

/*
ファイル内の言語がphpのみで構成されている(HTMLを含まない)場合、最後の ?> の記述は不要です。
*/
```

**【解説】**

①: `class DbData {`

PHPでクラス宣言を行う場合の書き方です。
ここで宣言したクラス内でプロパティ(メンバ変数)やコンストラクタ、メソッドを定義します。

②: `protected $pdo;`

PHPのアクセス修飾子は、以下の３つです。(Javaはこれ以外にアクセス修飾子を宣言しない「デフォルト」があります)

- `public`: クラス内、クラス外のどこからでもアクセス可能
- `protected`: 同じクラス、及び子クラスからアクセス可能
- `private`: 同じクラス内からのみアクセス可能

今回はクラス`DbData`を継承した子クラス`DbPhp`でこの `$pdo` プロパティ（メンバ変数）を利用するので、`protected` を使用します。

③: `public function __construct( ) {  }`

「コンストラクタ」とは、クラスからオブジェクトが `new` によって作成される時に自動的に呼び出されるメソッド(のようなもの)です。
コンストラクタは他のクラスから利用できるようにアクセス修飾子は `public` を使用して宣言します。
ここでは、データベースphpに接続するために `$dsn、$user、$password` の初期値を設定し、PDOオブジェクトを生成しています。

④: `$this->pdo = new PDO( $dsn, $user, $password );`

クラスのプロパティを定義し、そのクラスのメソッド内で参照したい場合、単に `$プロパティ名` では使用できません。
クラス内のプロパティにアクセスするためには、**疑似変数 `$this`** を使用し、`$this->プロパティ名` で使用します。(「->」を「シングルアロー」、「アロー演算子」という。)

例えば、プロパティとして `$name` を定義し、そのプロパティに値をセットする `setName( )` というメソッドを定義した場合、以下のように記述します。

```php
$name = ""; // プロパティとして宣言したメンバ変数

public function setName( $name ) { // この$nameはメソッドsetName( )の仮引数
  $this->name  =  $name;  // 「$this->name」がプロパティで宣言した「$name」 
} 　                      // 右辺にある「$name」はこのメソッドsetName( )の仮引数
```

⑤: `protected function query ( $sql, $array_params ) {  ｝`

この`DbData`クラスを継承したサブクラスで「SELECT文」を実行するときに呼び出すメソッドとして定義します。
継承したサブクラスでも利用できるようにアクセス修飾子 `protected` で宣言します。
２つの引数の内容は以下のとおりです。

- `$sql`: プレースホルダー(「?」)を利用して定義したSQL文
- `$array_params`: プレースホルダー(「?」)に代入する値を定義した配列データです。
実行結果のPDOステートメントオブジェクトを`return`文で返し、呼び出した側で `fetch( )` または `fetchAll( )` を使用して個々のデータの処理を行います。

⑥: `protected function exec ( $sql, $array_params ) {　}`

この`DbData`クラスを継承したサブクラスで「UPDATE文」、「INSERT文」、「DELETE文」を実行するときに呼び出すメソッドとして定義します。

この `exec( )` メソッドも継承したサブクラスでも利用できるようにアクセス修飾子 `protected` で宣言します。
２つの引数の内容は、⑤の`query( )` メソッドと同じです。

実行結果は、成功した場合の「true」または失敗した場合の「false」であるので、呼び出した側で必要に応じて処理を記述します。
