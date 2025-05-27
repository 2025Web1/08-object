---
sort: 3
---

# DbPhpクラス

次に、**クラス「DbData」を継承する、クラス「DbPhp」を定義するPHPファイル「dbphp.php」** を作成します。
このクラス「DbPhp」には、次の５つのメソッドを定義します。

① `selectAll( )` メソッド<br>
テーブルpersonのすべてのデータを抽出するメソッド

② `public function selectPerson(int $uid)` メソッド<br>
引数で指定されたユーザーID（$uid）のデータを抽出するメソッド

③ `public function insertPerson(string $name, int $cid, int $age)` メソッド<br>
引数で渡された氏名、カンパニーID、年齢の値で新規ユーザーを登録するメソッド

④ `public function updatePerson(int $uid, string $name)` メソッド <br>
引数で指定されたユーザーID($uid）の氏名を引数で渡された$nameの値に更新するメソッド

⑤ `public function deletePerson(string $name)` メソッド<br>
引数で指定された氏名（$name）のデータを削除するメソッド

**classes/dbphp.php**

```php
<?php
// DbDataクラスを利用するため
// 「DIR」の前後には2本のアンダースコア!!
require_once __DIR__ . '/dbdata.php';

// DBDataクラスを継承したDbPhpクラスの宣言
class DbPhp extends DbData
{
    // ①: テーブルpersonからすべてのデータを抽出する
    public function selectAll()
    {
        $sql = 'SELECT * FROM person';
        // 継承したDBDataクラスのquery( )メソッドを呼び出している
        // SQL文にプレースホルダはないので空の配列を渡している
        $stmt = $this->query($sql, []);
        $persons = $stmt->fetchAll();
        // 抽出した複数のデータを連想配列の形式で戻り値とする
        return $persons;
    }

    // ②:テーブルpersonから指定されたuidのデータを抽出する
    public function selectPerson($uid)
    {
        $sql = 'SELECT * FROM person WHERE uid = ?';
        // 継承したDBDataクラスのquery( )メソッドを呼び出している
        // SQL文のプレースホルダは1つだけだが配列の形式で渡す
        $stmt = $this->query($sql, [$uid]);
        $person = $stmt->fetch();
        // 抽出した1件のデータを連想配列の形式で戻り値とする
        return $person;
    }

    // ③:テーブルpersonに新規ユーザーを登録する
    public function insertPerson($name, $cid, $age)
    {
        $sql = 'INSERT INTO person (name, company_id, age) VALUES (?, ?, ?)';
        // 継承したDBDataクラスのexec( )メソッドを呼び出している
        // SQL文のプレースホルダの数だけ配列で渡す
        $this->exec($sql, [$name, $cid, $age]);
    }

    // ④:テーブルpersonのuidを指定し、氏名の値を更新する
    public function updatePerson($uid, $name)
    {
        $sql = 'UPDATE person SET name = ? WHERE uid = ?';
        // 継承したDBDataクラスのexec( )メソッドを呼び出している
        // SQL文のプレースホルダの数だけ配列で渡す
        $this->exec($sql, [$name, $uid]);
    }

    // ⑤:テーブルpersonの氏名を指定し、データを削除する
    public function deletePerson($name)
    {
        $sql = 'DELETE FROM person WHERE name = ?';
        // 継承したDBDataクラスのexec( )メソッドを呼び出している
        // SQL文のプレースホルダは1つだけだが配列の形式で渡す
        $this->exec($sql, [$name]);
    }
}
```

**【解説】**

`class DbPhp extends DbData {`

これにより`DbData`クラスで宣言した `$pdo` プロパティ、`query( )` メソッド、`exec( )` メソッドをこのクラスが利用できます。

なお、dbdata.php同様、ファイル内の言語がphpのみの場合、最後の `?>` の記述は不要です。

**dbdata.php, dbphp.phpのプログラムを作成しても、ブラウザ上での動作確認はまだできません。またGitHubにもpushはしないでください。動作確認、およびGitHubへのpushは、次章の「オブジェクト指向プログラミング②」にあるプログラムを作成する必要があります。**

## 付録: `require_once`とマジック定数 `__DIR__`

開発の規模が大きくなり、全体のコード量が膨大になると、それぞれのファイルに同じコードを記述するのは 非効率かつ保守性に問題がああります。
そこで、`require_once`を使い、共通的な処理を記述しているファイルを読み込むことで、効率化・保守性の向上を図る。

前述の「dbphp.php」では、次のように記述されている。

```php
require_once  __DIR__ . '/dbdata.php'; // 【注】__DIR__の「__」部分はアンダースコア2本!!
```

読み込むファイル名は絶対パスでも相対パスでもよいのだが、`__DIR__` を用いて絶対パスで指定します。
`__DIR__` はPHPのマジック定数（PHPの定義済み定数）の１つで、そのファイルの存在するディレクトリを返します。
なお、`__DIR__` の「`__`」部分はアンダースコア（ `_`）を２つ連続して記述するので注意してください。

[【参考ページ】PHPのマジック定数](http://php.net/manual/ja/language.constants.predefined.php)

ファイルを読み込む方法として他にも `require`、`include_once`、`include`などもありますが、下記の参考サイトで その違いなどを確認しておいてください。

[PHP requireとrequire_onceの違い](https://zeropuro.com/blog/?p=322)<br>
[PHP requireとincludeの違い](https://zeropuro.com/blog/?p=328)

|命令|特徴|
| - | - |
|require|指定されたファイルを何度でも読み込む。読み込めない場合、エラーとなる|
|require\_once|指定されたファイルを一度だけ読み込む。読み込めない場合、エラーとなる。|
|include|指定されたファイルを何度でも読み込む。読み込めない場合、警告となる|
|include\_once|指定されたファイルを一度だけ読み込む。読み込めない場合、警告となる。|
