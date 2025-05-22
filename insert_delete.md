---
sort: 5
---

# 作成したクラスの利用②

作成したクラス`DbPhp`を利用するphpファイルのコードを以下に示します。
なお、以下のコードも穴あきですので、穴を埋めてください。

## INSERT文

**obj_insert.php**

```php
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>obj_insert.php</title>
</head>

<body>
    <?php
    // (穴埋め)DbPhpクラスのオブジェクト生成し、insertPersonメソッドをよびだす
    // name = 深沢七郎, company_id = 3, age = 29
    



    // (穴埋め)登録後の全データを画面表示する
    



    ?>
</body>

</html>
```
![](./images/obj_insert_display.png)

## DELETE文

**obj_delete.php**

```php
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>obj_delete.php</title>
</head>

<body>
    <?php
    // (穴埋め)DbPhpクラスのオブジェクト生成し、deletePersonメソッドをよびだす
   



    // (穴埋め)登録後の全データを画面表示する
    



    ?>
</body>

</html>
```

![](./images/obj_delete_display.png)

## 【まとめ】オブジェクト指向プログラミング

本章を通して、以下のことを学びました。

- クラスによる「責任の分離」の理解
  - `dbdata.php` ・・・データベースの接続やSQL文の実行を担当
  - `dbphp.php` ・・・データベースの操作を担当
- インスタンス化の意義
  - `new` によってオブジェクトを作成することで、「クラスの設計図」をもとに「インスタンス（オブジェクト）という実態」を作成することができる
  - プログラムを手順ではなく、モノ（オブジェクト）の作成と操作として捉えることができる
- 再利用性と拡張性の感覚を得る
  - クラスを用いたり、継承したりすることでコードの再利用性が高まり、機能追加がしやすい構造であることを理解する

ただし、本章で学んだことは、あくまで「オブジェクト指向プログラミング」の一部に過ぎません。
本章を通じて、オブジェクト指向の考え方を深めたい人は、「[ちょうぜつ設計入門ーPHPで理解するオブジェクト指向の活用](https://gihyo.jp/book/2022/978-4-297-13234-7)」を参考にしてください。

## 付録: MVCモデル

Webアプリケーションの開発で「**MVCモデル**」というデザインパターンがよく利用されており、「Laravel」や「CakePHP」といったフレームワークにもこの「MVCモデル」の概念が取り入られています。

MVCとはModel・View・Controllerの略で、処理を３つの役割に分割して実装する手法です。<br>

![](./images/Aspose.Words.a4c93f43-ec41-42b5-b372-9be25bdbba96.013.jpeg)

- Controller: クライアントからのリクエストを直接受け取り処理を行う部分で、ModelやViewを「制御」する。
- Model: 処理のメインロジックやデータアクセスを担当する。
- View: 処理結果として画面表示（HTML出力）を担当する。

処理の流れとしては、以下のようになります。
Controllerが最も前面かつ全ての仲介に位置する。

1. Controllerがリクエスト情報を基にModelに処理を依頼
2. Modelはデータと連携して処理を行い、処理結果をControllerに返す
3. Controllerは返ってきた処理結果データをViewに渡す
4. Viewはデータを基にHTML出力処理を行う
