---
sort: 7
---

# 課題の解答

以下は、課題の解答例です。
課題に合格されなかった方は、こちらのコードを参考にしてください。

**obj_select1.php**

```php
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>obj_select1.php</title>
</head>

<body>
    <h1>条件付きSELECTの例</h1>
    <?php
    if (empty($_POST['uid'])) {
    ?>
        <form action="obj_select1.php" method="POST">
            <label for="uid">ユーザIDを入力してください：</label>
            <input type="text" name="uid" id="uid">
            <input type="submit" value="検索">
        </form>
    <?php
    } else {
        // 送信されてきた uid の値を受け取る
        $uid = $_POST['uid'];

        // DbPhpクラスのオブジェクト生成し、selectPerson( )メソッドをよびだす
        require_once __DIR__ . '/classes/dbphp.php';
        $dbPhp = new DbPhp();
        $person = $dbPhp->selectPerson($uid);

        // 抽出した結果に応じた画面を表示する
        if (empty($person)) {
            echo '該当するユーザがいませんでした(uid=' . $uid . ')';
        } else {
            echo 'uid=' . $person['uid'] . ', name=' . $person['name'] . '<br>';
        }
    }
    ?>
</body>

</html>
```
