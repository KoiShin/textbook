
Object Oriented Programming
===========================

オブジェクト指向プログラミング教育用の資料

目次
-------

[作成中]

- [はじめに](#preface)
- [オブジェクト指向とは](#introduce)
- [オブジェクトとは](#object)
- [クラスとは](#class)
- [自作クラス](#build-class)
- [コンストラクタとは](#constructor)
- [thisキーワード](#this)
- [オーバーロード](#overload)
- [アクセス権](#access)
- [クラスメソッドとクラス変数](#static)
- [継承とは](#extends)
- [オーバーライドとは](#override)
- [インターフェースとは](#interface)
- [抽象クラスとは](#abstruct)
- [インターフェースと抽象クラスの違い](#interface-and-abstruct)
- [UML図の簡単な説明](#uml)

<a name="preface"></a>

はじめに
-------

このドキュメントは、オブジェクト指向を教えるために用意した資料です。
このドキュメントが新米プログラマの教育担当の方の役に立てば、との思いで作成しております。
fork や pull request は自由にしてもらっても構いません。
新出単語は__太字__で表しています。

<a name="introduce"></a>

オブジェクト指向とは
-----------------

#### 背景

オブジェクト指向が登場する以前、ソフトウェアに対する需要はまだ少なく、
大規模なものでも十分なお金と時間をかけることができました。

しかし、その後私達の生活のあらゆる部分にコンピュータが使われるようになり、
ソフトウェアに対する需要は高まりました。
そのため、品質の良いソフトウェアを短期間でより多く開発しなくてはならない状況になりました。
そして「面倒なプログラミングの作業を楽にしたい」という要望が増えました。

結果として、__オブジェクト指向言語__と、
それを用いた開発方法、つまり__オブジェクト指向設計手法__が生まれたのです。

#### 現在

今では、有名なプログラミング言語の大半はオブジェクト指向が使えるようになりました。
つまり、この世にあるほとんどのソフトウェアはオブジェクト指向で書かれているとも言えます。

<a name="object"></a>

オブジェクトとは
--------------

__フィールド__や__メソッド__を持つものを__オブジェクト__と呼びます。

フィールドとはオブジェクトが持つ「情報を保存する場所」のことを指します。フィールドは「変数」に似ています。  
メソッドとは、オブジェクトが持つ「情報を処理する場所」のことを指します。メソッドは、「関数」に似ています。

また、オブジェクトのフィールドとメソッドをまとめて__メンバ__（member）と呼ぶことにします。

#### 具体例

次のスニペットは、フィールドとメソッドの使い方を示したものです。

Java

~~~ java
// x は座標（オブジェクト）のフィールド
point.x

// size() は配列（オブジェクト）のメソッド
array.size()
~~~

<a name="class"></a>

クラスとは
---------

オブジェクトのフィールドとメソッドを定義したものを__クラス__と呼びます。
Javaには多くのクラスがすでに定義されています。
String、ArrayやArrayListなどの大文字から始まる型の名前は全てクラスです。

また、クラスを元にしてできた新たなオブジェクトを__インスタンス__と呼び、
`new`してインスタンスを作ることを__インスタンスの生成__と呼びます。

#### 具体例

Java

~~~ java
// 変数ary は Arrayクラス のインスタンス
Array[] ary = new Array[5];

// 変数str は Stringクラス のインスタンス
String str = new String("apple");

// Stringクラスにおける、別のインスタンスの生成方法（推奨）
String str = "apple";
~~~

<a name="build-class"></a>

自作クラス
---------

クラスは自分で作成することもできます。
説明のために、ここでは座標を表すためのPointクラスを作成していきます。

#### フィールドの定義例

はじめに、フィールドのみで構成される簡単なPointクラスを定義します。  

Main.java

~~~ java
package your_package_name;

public class Main {
    public static void main(String[] args) {
        Point point = new Point();
        point.x = 3;
        point.y = 4;
        System.out.println(point.x); //=> 3
        System.out.println(point.y); //=> 4
    }
}

// クラスの定義
class Point {
    // フィールドの定義
    public int x;
    public int y;
}
~~~

フィールドへの代入は `point.x = 3` 、フィールドへのアクセスは `point.x` のようにします。


#### メソッドの定義例

次に、フィールドである座標と原点との距離を返す関数 distance を作成します。
原点との距離は、公式 √(x^2 + y^2) を使います。

Main.java

~~~ java
package your_package_name;

public class Main {
    public static void main(String[] args) {
        Point point = new Point();
        point.x = 3;
        point.y = 4;
        System.out.println(point.distance()); //=> 5.0
    }
}

// クラスの定義
class Point {
    // フィールドの定義
    public int x;
    public int y;

    // メソッドの定義
    double distance() {
        return Math.sqrt(x*x + y*y);
    }
}
~~~


<a name="constructor"></a>

コンストラクタとは
---------------

演算子`new`でインスタンス化するための処理は、
__コンストラクタ__と呼ばれるメソッドを定義することによって行われます。

コンストラクタを定義することで、
インスタンス化する際に `new Point(3, 4)` のように引数を渡して初期化を行うことができます。

Main.java

~~~ java
package your_package_name;

public class Main {
    public static void main(String[] args) {
        Point point = new Point(3, 4);
        System.out.println(point.distance()); //=> 5.0
    }
}

class Point {
    public int x;
    public int y;

    // コンストラクタの定義
    Point(int _x, int _y) {
        x = _x;
        y = _y;
    }

    double distance() {
        return Math.sqrt(x*x + y*y);
    }
}
~~~

コンストラクタを用いたことにより、インスタンスの初期化が楽になりました。


<a name="this"></a>

thisキーワード
-------------

__this__キーワードを使うことで、フィールドの変数と関数の引数の変数を、区別することができます。
セクション「[コンストラクタ](#constructor)」のコンストラクタ定義部分のコードと比較しながら見てください。

Java

~~~ java
class Point {
    public int x;
    public int y;

    // コンストラクタの定義
    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    // ...
}
~~~


<a name="overload"></a>

オーバーロード
------------

関数名は同じだが、引数の数または種類が異なる関数を定義することを、
多重定義 もしくは __オーバーロード__ と呼びます。

#### 具体例

原点からの距離を求める関数 `distance()` があります。
それに加えて、引数に Point型 の変数を与えると、
自分の座標と引数の座標との距離を返す関数 `distance(Point other)` を定義します。
関数 distance におけるオーバーロードの例です。

~~~ java
public class Main {
    public static void main(String[] args) {
        Point point = new Point(3, 4);
        Point otherPoint = new Point(15, 20);

        System.out.println(point.distance()); //=> 5.0
        System.out.println(point.distance(otherPoint)); //=> 20.0
    }
}

class Point {
    public int x;
    public int y;

    // ...

    double distance() {
        return Math.sqrt(x*x + y*y);
    }

    double distance(Point other) {
        double xDiff = this.x - other.x;
        double yDiff = this.y - other.y;
        double dist = Math.sqrt(
            Math.pow(xDiff, 2) +
            Math.pow(yDiff, 2)
        );
        return dist;
    }
}
~~~

2点間の距離は、公式 √((x1 - x2)^2 + (y1- y2)^2) を使います。



<a name="access"></a>

アクセス権
---------

__アクセス修飾子__を使うことでフィールドやメソッドのアクセス権を制御することができます。

アクセス修飾子は __public__, 記述なし(__default__), __protected__, __private__ の4種類あります。
下に行くほどアクセス権が厳しくなります。

- `public variable;`
- `public function() { ... }`
    - 同一パッケージ、外部パッケージからアクセスできる（どこからでもアクセスできる）
- `variable;`
- `function() { ... }`
    - 同一パッケージからのみアクセスできる
- `protected variable;`
- `protected function() { ... }`
    - 同一パッケージ、自クラス、自クラスのサブクラスのみからアクセスできる
- `private variable;`
- `private function() { ... }`
    - 自クラスからのみアクセスできる

#### 具体例

フィールドやメソッドにおけるアクセス権の検証

Main.java

~~~ java
package your_package_name;

public class Main {
    public static void main(String[] args) {
        AccessTest accessTest = new AccessTest();

        System.out.println(accessTest.public_var);
        System.out.println(accessTest.default_var);
        System.out.println(accessTest.protected_var);
        System.out.println(accessTest.private_var); // エラー発生!

        accessTest.public_func();
        accessTest.default_func();
        accessTest.protected_func();
        accessTest.private_func(); // エラー発生!
    }
}

class AccessTest {
    // 順に public, (default), protected, private

    public int public_var = 1;
    int default_var = 1;
    protected int protected_var = 1;
    private int private_var = 1;

    public void public_func() {}
    void default_func() {}
    protected void protected_func() {}
    private void private_func() {}
}
~~~

public, (default), protected 修飾子をつけると、
クラスの外からアクセスされるべきでないフィールドの中身が書き換えられてしまったり、
クラス内でしか使わないメソッドがクラスの外から勝手に起動させられたりするので、
基本的に全て private にして、公開する機能だけ public にしましょう。

クラス内のメンバは全て private または public にするのが一般的なので、
ここでは public, (default), protected 修飾子の使い分けを説明しません。（説明が面倒くさい）

また、クラスにも public修飾子 または (default)修飾子 をつけることができます。

- `public class ClassName { ... }`
    - このパッケージが、外部のパッケージに使われる際に、このクラスにアクセスできる
    - 同一パッケージ内からもアクセスできる
- `class ClassName { ... }`
    - このパッケージが、外部のパッケージに使われる際に、このクラスにアクセスでき__ない__
    - 同一パッケージ内からはアクセスできる



<a name="static"></a>

クラスメソッドとクラス変数
-----------------------

メソッドに__static__修飾子を付けると、そのメソッドはクラスにとって唯一のメソッドとなり、
インスタンスの生成しなくてもそのメソッドを使うことができます。
クラスにとって唯一のメソッドのことを、__クラスメソッド__と呼ぶことにします。
なお、クラスメソッドは特性上、staticでないフィールドやメソッドにはアクセスできません。

フィールドに__static__修飾子を付けると、そのフィールドはクラスにとって唯一のフィールドとなり、
インスタンスを生成しなくてもそのフィールドを使うことができます。
クラスにとって唯一のフィールドのことを、__クラス変数__と呼ぶことにします。

#### 具体例

現在のユーザ数を記録する クラス変数userNum と現在のユーザ数を返す クラスメソッドgetUserNum を持つクラス User を作成する

Main.java

~~~ java
package your_package_name;

public class Main {
    public static void main(String[] args) {
        System.out.println(User.getUserNum()); //=> 0
        User user1 = new User();
        User user2 = new User();
        User user3 = new User();
        System.out.println(User.getUserNum()); //=> 3
    }
}

class User {
    static private int userNum = 0;
    static int getUserNum() {
        return userNum;
    }

    // Userのインスタンスが生成されるごとに ユーザ数 を +1 する
    User() {
        userNum++;
    }
}
~~~

デストラクタがある言語は、
「Userのインスタンスが（メモリから）解放されるごとに ユーザ数 を -1 する」
という処理があってもいいでしょう。




<a name="extends"></a>

継承とは
-------

あるクラスの性質を引き継いでクラスを作ることを__継承__と呼びます。
継承元になった親クラスを__スーパークラス__、継承した子クラスを__サブクラス__と呼びます。

スーパークラスを継承することによって、
そのクラスの public または protected なフィールドとメソッドが
サブクラスで使えるようになります。

#### 具体例

次に、Pointクラスは2次元の座標しか扱えないので、
このクラスを継承して3次元座標を扱えるPoint3Dクラスを作成します。

さらに、3次元空間における原点からの距離を返す関数 distance3d を新たにPoint3Dクラスで定義します。

Main.java

~~~ java
package your_package_name;

public class Main {
    public static void main(String[] args) {
        Point3D point3d = new Point3D(3, 4, 5);
        System.out.println(point3d.distance()); //=> 5
        System.out.println(point3d.distance3d()); //=> 7.07...
    }
}

class Point {
    public int x;
    public int y;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    double distance() {
        return Math.sqrt(x*x + y*y);
    }
}

// Pointクラス を継承した Point3Dクラス の定義
class Point3D extends Point {
    public int z;

    // コンストラクタの定義
    Point3D(int x, int y, int z) {
        super(x, y);  //< スーパークラスのコンストラクタを呼んでいる
        this.z = z;
    }

    // Point3dクラスで、新しいメソッドの定義
    double distance3d() {
        return Math.sqrt(x*x + y*y + z*z);
    }
}
~~~




<a name="override"></a>

オーバーライドとは
----------------

スーパークラスから継承したメソッドを再定義することを__オーバーライド__と呼びます。

#### 具体例

スーパークラスの関数 distance をサブクラスでオーバーライドする例です。

Java

~~~ java
class Point {
    public int x;
    public int y;

    // ...

    double distance() {
        return Math.sqrt(x*x + y*y);
    }
}

class Point3D extends Point {
    public int z;

    // ...

    // スーパークラスから継承したメソッドを再定義（上書き）する
    @Override
    double distance() {
        return Math.sqrt(x*x + y*y + z*z);
    }
}
~~~

オーバーライドをする関数の前に `@Override` を追加します。
この`@`から始まる注釈を__アノテーション__と呼びます。
アノテーションは付けなくてもオーバーライドはできますが、
これを付けるとコンパイラに「このメソッドはオーバーライドするんだよ」と伝えることができます。

アノテーションを加えることで、次のような利点があります。

- コンパイル時に、正しくオーバーライドできていないときには、警告してくれる
- どのメソッドがオーバーライドしているのか、視覚的に分かりやすい

補足）英語で上書きは「overwrite」ですが、オーバーライドの綴りは「override」です。



<a name="interface"></a>

インターフェースとは
-----------------

オブジェクトのメソッド名を「規定」するためのものを__インターフェース__と呼びます。
また、そのインターフェースの規定に従ってクラスを作ることを、
（クラスの）__インターフェースを実装する__といいます。

インターフェースには、__抽象メソッド__を定義します。
抽象メソッドとは中身のない名前だけのメソッドのことです。
__abstruct__修飾子を付けることで抽象メソッドを作ることができます。

以下のリストは、インターフェースにおける修飾子のルールです。

- インターフェースのメソッドは必ず抽象メソッドとなるため、
abstruct修飾子 が自動で付けられます。
abstruct修飾子は明示的に記述してもいいです（筆者は推奨）

- また、インターフェースにフィールドを与えることもできますが、
そのフィールドは必ず唯一の定数となるため、static修飾子 と final修飾子 が自動で付けられます。
staticとfinal修飾子は明示的に記述してもいいです（筆者は推奨）

- さらに、全てのメンバは必ず public となるため、public修飾子 が自動で付けられます。
public修飾子は明示的に記述してもいいです（筆者は推奨）

#### インターフェースの定義例

USBに接続するためのインターフェースの定義例です。

Java

~~~ java
// USBInterface という interface の定義
interface USBInterface {
    // 変数の定義例
    public static final float USB_VERSION = 3.0;

    // メソッドの規定
    public abstract boolean connectUSB();
    public abstract boolean disconnectUSB();
}
~~~

#### インターフェースの実装例

インターフェースの抽象メソッドを実装するのは、インターフェースを実装するクラスです。
「インターフェースを実装する」というのは、抽象メソッド

ここでは Printer は USB接続を行うものとします。

Java

~~~ java
class Printer implements USBInterface {
    public boolean connectUSB() {
        // USB接続を行う具体的な処理
        // ...
    }

    public boolean disconnectUSB() {
        // USB接続を切断する具体的な処理
        // ...
    }

    public boolean print(PDF pdf) {
        // pdfファイルを印刷する具体的な処理
        // ...
    }

    // ...
}
~~~

ここまで読まれた方は、
「なんでインターフェースを定義する必要があるの？
インターフェース定義している暇があったら、さっさと実装しろよ（怒）」
と思うかもしれません。
この例では Printer しか使っていないので、そう感じるかもしれません。

では、次の例を考えてみてください。USB接続を行うものはたくさんあります。
外部記憶装置、入力装置、出力装置、etc.
それらは全てクラスで実装されているのですが、
それぞれが独自にUSBに接続するメソッドを定義していたらどうでしょう。
あるクラスでは `connectUSB()`、別のクラスでは `connect()`、
また別のクラスでは `startConnection()` etc.
これらを使うプログラマとしては、全てのクラスの全てのメソッドを把握する必要があり、とても大変です。

そこで、interface を使うとどうなるでしょうか。ユーザは、使いたいクラスが USBInterface を実装しているか確認するだけで、どのメソッドを使えばいいのかすぐに理解できるようになります。
また、これから USB接続の処理を実装しようとしているプログラマに、
必要となるメソッドの名前を示唆してくれます。

インターフェースの利点を生かした設計技法は、デザインパターン（別紙）で詳しく扱いたいと思います。



<a name="abstruct"></a>

抽象クラスとは
------------





<a name="interface-and-abstruct"></a>

インターフェースと抽象クラスの違い
------------------------------


<a name="uml"></a>

UML図の簡単な説明
---------------













~~~
<>∧∨
~~~


























































