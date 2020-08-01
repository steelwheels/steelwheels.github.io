# Cocoa Scripting

## Cocoa Scriptingとは
* Cocoa Scriptingとは、スクリプト制御可能なmacOSアプリケーションを実装するための、Cocoaフレームワークの機能を指します。これをサポートしたアプリケーションは、AppleScriptで制御可能になります。
* 当初は、[Apple Event Manager](https://developer.apple.com/documentation/applicationservices/apple_event_manager)が使用されていましたが、Tiger以降はCocoa Scriptingがこれに取って変わりました。

## Cocoa Scriptのサポート準備
macOSアプリにてCocoa Scriptingをサポートする際には、実装以前に複数項目の準備が必要です。
### ターゲットプロパティ (.plistファイル)
plistファイルに以下の要素を追加し、CocoaScriptingサポートを有効化します。設定中の`Scripting.sdef`は後述するスクリプト定義ファイルの名前です。
下記の例では、.sdefファイルは、アプリケーションのResourceフォルダの直下におかれる事を想定しています。
````
<key>NSAppleScriptEnabled</key>
<string>YES</string>
<key>OSAScriptingDefinition</key>
<string>Scripting.sdef</string>
````

## スクリプト定義 (.sdefファイル)
アプリケーションがサポートするスクリプトの種類を定義します。　xmlフォーマットに基づいて記述します。
詳細については、[Preparing a Scripting Definition File](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ScriptableCocoaApplications/SApps_creating_sdef/SAppsCreateSdef.html#//apple_ref/doc/uid/TP40001979-BBCBCIJE)を参照して下さい。

以下はsdefファイルの一例です。最上位は、｀dictionary`ノードです。
````
<?xml version="1.0" encoding="UTF-8"?> 
<!DOCTYPE dictionary SYSTEM "file://localhost/System/Library/DTDs/sdef.dtd">
<dictionary title="CoconutScript">
  ... スクリプト定義 ...
</dictionary> 
````

dictionaryノードは、1つ以上の`suite`ノードを含みます。関連のある定義が1つの`suite`ノードに格納されます。
````
<dictionary title="CoconutScript">
  <suite name="Preference Suite" code="CcPS" description=" Suites by CocoaScript">
   <class name="application" code="capp" description="The application's top-level scripting object.">
     ... Application suite 定義 ...
   </class>
  </suite>
</dictionary> 
````

`suite`の1つです。アプリケーションクラスを定義します。アプリケーションクラスは、[NSApplicationDelegate](https://developer.apple.com/documentation/appkit/nsapplicationdelegate)にマップされます。のインターフェースを定義します。
````
<class name="application" code="capp" description="The application's top-level scripting object.">
     <cocoa class="NSApplication"/>
     ... Application プロパティ定義 ...
</class>
````

以下の例では、アプリケーションクラスに、`foreground color`と`background color`プロパティを定義します。
````
<property name="foreground color" code="fgcl" description="Foreground color of the terminal" type="Color" access="rw"/>
<property name="background color" code="bgcl" description="Background color of the terminal" type="Color" access="rw"/>
````

## 機能実装
AppleScriptからアプリケーションへのアクセスは、KVOのプロトコルで通知されます。`.sdef`ファイルのプロパティの`name`要素で指定した名前が、キーとして使用されます。

````
open class CNScriptableAppicationDelegate: CNApplicationDelegate
{
  public override func value(forKey key: String) -> Any? {
  }
  open override func setValue(_ value: Any?, forKey key: String) {
  }
}
````


## 参考文献
1. [Cocoa Scripting Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ScriptableCocoaApplications/SApps_intro/SAppsIntro.html#//apple_ref/doc/uid/TP40001982-BCICHGIE): Cocoa Scriptingについて、おそらく最も詳細なドキュメント。ただ、かなり昔(2008年)に書かれている。PDF版は[こちら](https://applescriptlibrary.files.wordpress.com/2013/11/cocoa-scripting-guide.pdf)。
1. [Getting Started With Cocoa Scripting](http://www.apeth.net/matt/scriptability/scriptabilityTutorial.html): Cocoa Scriptingについて、簡単な例を持って説明。[1]よりかなりシンプル。