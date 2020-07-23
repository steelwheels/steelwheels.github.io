# Cocoa Scripting Guide
本文書は、[Cocoa Scripting Guide](https://applescriptlibrary.files.wordpress.com/2013/11/cocoa-scripting-guide.pdf) の内容に関するメモです。
関連書籍: [AppleScript Overview](https://applescriptlibrary.files.wordpress.com/2013/11/applescript-overview.pdf)

## Introduction to Cocoa Scripting Guide
* スクリプト制御可能なアプリーケーションの実装をサポートするCocoaアプリケーションフレームワークを、Cocoa Scriptingと呼ぶ。
* スクリプト制御可能なアプリーケーションは、通常AppleScriptにて制御される。
* スクリプトを実行すると、複数のコマンドが、AppleEventの形式で、制御対象アプリケーションに、プロセス間通知を使用して送られる。
* Cocoa Scriptingは、AppleEventの受診, 解析,  ユーザ定義のコールバックの呼び出しを行う。

## Overview of Cocoa Support for Scriptable Applications
* スクリプト制御可能なアプリーケーションのために用語の辞書を定義する。辞書は、スクリプト作成者が使用可能なクラスやメソッドを定義する。
* Cocoa Scriptingは、AppleScriptをサポートするためのクラス・カテゴリ・スクリプト化するための情報を持つ。
* Cocoa Scriptingでは、　KVO, MVCを利用する。アプリケーションに送られたコマンドは、アプリケーションモデルオブジェクトに直接作用する。KVOを使用する事で、スクリプト制御に必要なコードの追加を少なくする。

## AppleScript and Scriptable Applications
* AppleScriptは、スクリプト制御可能なアプリーケーションやmacOSの一部(Finderなど)を制御するための言語。
* AppleScriptは特定のアプリケーションにむけた機能をサポートしない。その変わり、`get`, `set`, `make`, `delete`などの共通コマンドを定義する。
* 一方でスクリプト制御可能なアプリーケーションは、その処理の内容に応じた特定の処理を定義する事ができる。
* AppleEventはプロセス間通信にて送られるメッセージで、複雑な処理とデータを持てる。
* AppleEventによって複雑なタスクを1つにまとめ 、異なるプロセスにこれを送り出し、結果をイベントとして受け取る事ができる。
* スクリプト制御可能なアプリーケーションは、AppleEventを、Apple Event Managerに登録
* AppleScriptとAppleEventは、[Open Scripting Architecture(OSX)](https://applescriptlibrary.files.wordpress.com/2013/11/applescript-overview.pdf)で定義される。

## The AppleScript Object Model
* スクリプト制御可能なアプリーケーションは、いずれもAppleScript object model を定義する。
* AppleScript object model には、スクリプトでアクセス可能なオブジェクト, クラスの継承関係やプロパティが定義される。
* 通常、オブジェクトモデルのオブジェクトと、スクリプトサポートを実装するオブジェクトクラスは一致していますが、必ずしもそうする必要はない。

## Scriptability Information
* スクリプト制御可能なアプリーケーションは、scriptability informationを提供する必要がある。Scriptability informationは、AppleScript objectとアプリケーションオブジェクトモデルの対応状態を表す。
* Scriptability informationは、次の2種類の定義を含む:
  1. スクリプト中で、アプリケーションを制御するために可能な用語、その目的と使い方を説明する
  2. 上記用語とアプリケーションの実装を関連付ける。
  3. KVOで使用するキーの値を定義します。このキーを通じて、スクリプトからのプロパティの読み書きを行う。
* Scriptability informationの中の用語をユニークにするために、four-character-code(もしくはAppleEventCode)を使用する。コードは4文字の文字列で定義する。詳細は “Code Constants Used in Scriptability Information”
* Scriptability informationでは、関連する機能（たとえば、テキスト、グラフィック、データベースなどの操作）用語も、スイートとしてまとめられる。 定義済みの用語については、"Built-in Support for Standard and Text Suites"を参照。

### Scriptability Information Formats
* Scriptability informationを記述するフォーマットは以下の2通り:
  1.  `sdef`フォーマット: XMLを用いて、用語, コマンド, クラス, 定数を定義する、1つのファイル(scripting definition, `.sdef`)に全ての情報が格納される。
  2. 上より古いフォーマット。
* XMLのフォーマットについては、manページ、または“Preparing a Scripting Definition File”を参照。

### Viewing Scripting Terminology

### Built-in Support for Standard and Text Suites
* ほとんどのアプリーケーションは、標準的なAppleScript制御、すなわちcount, makeコマンドをサポートする。これらは標準スイート(Standard Suite)と呼ばれる。
* 標準コマンドを実装するためのNSCountCommandやNSCreateCommandなどのコマンドクラスが提供されている
* さらに、NSApplicationやNSDocumentなどのクラスは、標準のスクリプトサポートの特定の側面を実装している
* `set`及び`get`コマンドも、 NSGetCommandとNSSetCommandクラスにてサポートされる。これらは標準スイートではないが、アプリケーションオブジェクトのプロパティと要素の値を取得および設定できるようにすることは、スクリプト対応するための重要な部分である。
* 標準スイートでは、次のコマンドを定義する: コピー、カウント、作成、削除、存在、および移動。
* リッチテキスト, ワード, パラグラフをサポートする時は、テキストスイートが必要。
* テキストスイートについては、“Use the Document Architecture”, “Access the Text Suite”を参照
* テキストスイートをサポートする方法については、“Turn On Scripting Support in Your Application”を参照

### Built-in Support for Basic AppleScript Types
