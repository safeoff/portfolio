# ポートフォリオ

このページでは、個人開発で作成したツールの説明、および  
前職での開発業務でのプロセス概要を説明することを目的とします。  

----

### 目次
* 個人開発
    * Web / クイズの問題集検索サイト
    * CLI / 将棋の問題集作成ソフト
    * GUI / パズドラのクローン
* 前職での開発プロセス概要（OSSへのプルリクエストをサンプルに）

----

## 個人開発
### Web / クイズの問題集検索サイト

https://umigamelog.netlify.com/  
ウミガメのスープというクイズがあり、匿名掲示板で行われた過去問を集約・検索可能な状態にしているサイトです。  
Hugoで生成した静的コンテンツからLambdaを呼ぶことで、AWS無料範囲での運用を行っています。  
Lambda採用前は、PHPおよびPythin CGI/MySQLでの運用をしていました。  

#### 使用技術：
サーバ、通信：AWS Lambda / AWS API Gateway / Ajax / Netlify / Hugo  
データベース：SQLite  
データ加工：Python / golang  

#### 構成図
![doc1](https://raw.githubusercontent.com/safeoff/portfolio/master/doc1.png)

----

### CLI / 将棋の問題集作成ソフト

https://github.com/safeoff/shogi-tactics  
将棋対戦サイトで行われた棋譜から、プレイヤーが指し手を間違えた局面を将棋ソフトで検討＆抽出し、AnkiWeb（単語帳サービス）に登録するツールです。  
↓の順で動作します。  
ユーザがアカウントと条件を指定＆実行  
↓  
将棋対戦サイトから棋譜をスクレイピング  
↓  
棋譜を将棋ソフトで検討  
↓  
抽出した局面を成形してAnkiWebに登録

#### 使用技術：
サーバ、通信：curl / Python / golang  
データ加工：Python  

#### 構成図
![doc2](https://raw.githubusercontent.com/safeoff/portfolio/master/doc2.png)

----

### GUI / パズドラのクローン

パズドラの対戦部分をリバースエンジニアリングしたGUIプログラムです。
初めにJavaで作成し、後に、GitHub Pagesで動作させるためにTypeScriptで書き直しました。

#### 使用技術（Java版）：
GUIツール：Java Swing
#### 使用技術（TypeScript版）：
GUIツール：canvas
パッケージ・資源管理：npm / webpack

#### Java版の動作サンプル
Linuxデスクトップ上でjar実行ファイルを動作させたものです。  
![doc3](https://raw.githubusercontent.com/safeoff/portfolio/master/doc.gif)

#### TypeScript版のデモページ
https://safeoff.github.io/puzzle/  
操作が可能です。  
こちらは盤面移動の再現までとなります。  
(初回起動時に画像が読み込まれない不具合があります。F5キーで更新してください)  

----

## 前職での開発プロセス概要

前職で担当していた開発プロセスの概要を説明します。  
ちょうど、最近個人でOSSへ送ったプルリクエストが、前職での開発プロセスと同じ流れでしたので、説明のためのサンプルとして引用します。  
実際の業務では、各段階をクライアントへ説明するための資料を詳細に作成していました。  
ID:kovetskiyさんがOSSリポジトリの所有者で、ID:safeoffがコントリビューター（私）です。  
#### プルリクエスト
https://github.com/kovetskiy/ankictl/pull/3  

#### 開発プロセス

1. ソフト変更内容

```
safeoff commented on 14 Dec 2019  
Hi, I've added --input option for add new card from optional argument.  
```

2. 解決すべき課題の設定
3. ソフト変更によって得られる効果

```
This is for call ankictl from other program. It was difficult with stdin.  
```

4. 効果/弊害の検査

```
safeoff commented on 14 Dec 2019  
Tested with subprocess of python, the modified version worked as expected.  
Looking forward to your confirmation.  
Thanks  
```

5. 設計レビュー
6. コードレビュー

```
kovetskiy commented on 16 Dec 2019  
Hi! First of all, thank you for your contribution, I understand your inconvenience using stdin, although it's a standard way for unix programs.  
  
a few nitpicks and good to go  
  
kovetskiy on 16 Dec 2019 Owner  
Could you please avoid using args here? Maybe just pass format string and dec string as two arguments.  

kovetskiy on 16 Dec 2019 Owner  
the function returns error if an error has occurred, could you handle that error here and call log.Fatal if it's not nil?  

kovetskiy on 16 Dec 2019 Owner  
I see you are a quite new for Go, there is a idiomatic way to handle that case which called comma-ok, so instead of checking for nil you can really check if map contains key  
if target, ok := args["--input"].(string); ok {  
then you will not need to unsafely type-cast it to string  
```

7. レビュー対応/再テスト

```
safeoff commented on 18 Dec 2019  
Thank you for the review!  
I corrected the points that were pointed out.  

safeoff commented on 18 Dec 2019  
I'm sorry, my environment's go fmt setting was off.  
Executed go fmt and newly committed.  
```

8. マージ

```
kovetskiy commented on 19 Dec 2019  
Thanks! Merged.  
```
