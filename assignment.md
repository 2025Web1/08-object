---
sort: 6
---

# 課題について

この課題では前章で作成したクラス`DbPhp`を利用するphpファイルのコードを以下に示します。

## 条件付きSELECT文



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
    // (穴埋め)もしも送られてきたuidのデータが空なら、uidを求めるフォームを表示
    if (                    ) {
    ?>
        <form action="dbselect1.php" method="POST">
            <input type="text" name="uid">
            <input type="submit" value="検索">
        </form>
    <?php
    } else {
        // (穴埋め)uidをキーにして、受け取ったuidを代入
        $uid = 

        // (穴埋め)DbPhpクラスのオブジェクト生成し、selectPerson( )メソッドをよびだす
        




        // (穴埋め)抽出した結果に応じた画面を表示する
        // 結果が空ならば、該当するユーザがいない旨を表示
        if (           ) {
            echo 
        } else {
            // (穴埋め)結果があれば、uidとnameを表示
            echo 
        }
    }
    ?>
</body>

</html>
```

1. `person`テーブルにデータのあるユーザーIDを入力し、「検索」ボタンを押した時<br>
→該当する`uid`と`name`が表示される
![](./images/obj_select1_display1.png)
![](./images/obj_select1_display2.png)

1. `person`テーブルにデータのないユーザーIDを入力し、「検索」ボタンを押した時<br>
→該当するデータが無い旨のメッセージが表示される
![](./images/obj_select1_display3.png)
![](./images/obj_select1_display4.png)

1. ユーザーIDを入力せず、「検索」ボタンを押した時<br>
→検索フォームが表示のまま
![](./images/obj_select1_display5.png)
![](./images/obj_select1_display6.png)
