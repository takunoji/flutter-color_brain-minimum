# color_brain

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.


Day5(別版): 実際にアプリ(ゲーム)を作ってみる
fastriver
fastriver
2022.12.12に更新
このチャプターの目次
流れと目標
今日の流れ
アプリの画面を設計する
どのような画面が必要かを考える
ゲーム画面の構成
ゲーム部分を作る
ゲーム画面のStatefulWidgetを用意する
中央縦並びレイアウトにする
要素を配置する
ボタンを配置
問題を自動で作る
問題に答える
発展課題
Webに公開する
ビルド
Netlifyを使って公開する
おしまい
流れと目標
目標: 脳トレゲームを作ってWebに公開する



今日の流れ
アプリの画面を設計する
ゲーム部分を作る
レイアウトとゲームのロジックを組む
Webに公開する
完成時のプログラムは↓にあるので適宜参照のこと。


アプリの画面を設計する
Webサイトやアプリを作る場合、プログラムを組む前に必要なのが「設計」という工程である。特に画面については、必要な機能を画面のどこに置くのかというのを予めデザインツールなので検討することが多い。

どのような画面が必要かを考える
今回作るのは文字の色を当てるゲームである。まずどのような画面が必要かを考えると、以下が思い浮かぶ。

ゲーム画面
ゲームで遊ぶ
結果画面
結果を表示する
それ以外にもスタート画面を用意したり、設定画面を追加したりなど工夫は可能だが、まずはシンプルに1画面のみで作ってみよう。

ゲーム画面の構成
文字の色を当てるゲームで画面に必要なものを以下に挙げる。

遊び方を表示するテキスト
問題を表示するテキスト
ユーザが解答するためのボタン
これらを適当に並べると



要素が少ないので中央で縦に並べるのがまとまりがよい。

画面はできたので次はどのWidgetを組み合わせればこの画面が作れるのかを考えよう。



まず中央縦並びは、CenterとColumnを使って実現できる。
メッセージ部分はTextを置けばよい。
ボタンはElevatedButtonを使い、Rowで横並びにする。

ゲーム部分を作る
まずはtic_tac_toeというプロジェクトを新しく作る



code [フォルダ]と打ち込むとそのフォルダをVSCodeで開いてくれたりする(PCによってはできないかもしれない)

ゲーム画面のStatefulWidgetを用意する
lib/main.dartを以下のように置き換える

// ignore_for_file: library_private_types_in_public_api, use_key_in_widget_constructors, prefer_const_constructors, avoid_print, sized_box_for_whitespace, prefer_const_literals_to_create_immutables
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Color Brain',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});
  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

最後のContainerの部分を置き換えて画面を作っていく。

flutter run -d chromeで画面を出しても真っ白である

中央縦並びレイアウトにする
Centerの中にColumnを置き、mainAxisSize属性をminにするとColumnの子らが中央縦並びになる。

  Widget build(BuildContext context) {
    return Scaffold(
        body: Center(
      child: Column(mainAxisSize: MainAxisSize.min, children: [
        Container(
          width: 100,
          height: 100,
          color: Colors.green,
        ),
        Container(
          width: 100,
          height: 100,
          color: Colors.blue,
        ),
        Container(
          width: 100,
          height: 100,
          color: Colors.red,
        ),
      ]),
    ));
  }



要素を配置する
ゲーム画面に必要な要素は、テキスト2つと解答用ボタンの3つである。

それらを先程のContainerの代わりに配置する。

  Widget build(BuildContext context) {
    return Scaffold(
        body: Center(
      child: Column(mainAxisSize: MainAxisSize.min, children: [
        Text(
          "文字の色のボタンを押そう",
          style: TextStyle(fontSize: 30),
        ),
        Padding(
          padding: const EdgeInsets.all(32.0),
          child: Text(
            "あお",
            style: TextStyle(color: Colors.green, fontSize: 100),
          ),
        ),
        Container(
          width: 200,
          height: 50,
          color: Colors.red,
        ),
      ]),
    ));
  }




赤い四角にボタンを配置する。

ボタンを配置
ボタンはとりあえず赤・緑・青の3つを置くことにする。3つであれば横並びでよいだろう。

横並びを実現するためにはRow Widgetを使う。

 Widget build(BuildContext context) {
   return Scaffold(
       body: Center(
     child: Column(mainAxisSize: MainAxisSize.min, children: [
       Text(
         "文字の色のボタンを押そう",
         style: TextStyle(fontSize: 30),
       ),
       Padding(
         padding: const EdgeInsets.all(32.0),
         child: Text(
           "あお",
           style: TextStyle(color: Colors.green, fontSize: 100),
         ),
       ),
         Container(
           width: 200,
           height: 50,
           color: Colors.red,
         ),
         Row(
           mainAxisSize: MainAxisSize.min,
           children: [],
         ),
     ]),
   ));
 }

このchildrenの配列に、ボタンを入れればよい。

ボタンを作る
それぞれのボタンはElevatedButtonにパディングを加えたものとする。

Padding(
  padding: const EdgeInsets.all(8.0),
  child: ElevatedButton(
    onPressed: null,
    child: Text("Blue"),
  ),
),

Padding: ボタンの周りの余白を指定
ElevatedButton: ボタンの挙動を指定
Text: ボタンの上に表示する文字を指定
ボタンを配置する
先程用意したRowのchildrenの配列に入れれば、順番に並べられる。

Row(
  mainAxisSize: MainAxisSize.min,
  children: [
    Padding(
      padding: const EdgeInsets.all(8.0),
      child: ElevatedButton(
        onPressed: null,
        child: Text("Red"),
      ),
    ),
    Padding(
      padding: const EdgeInsets.all(8.0),
      child: ElevatedButton(
        onPressed: null,
        child: Text("Green"),
      ),
    ),
    Padding(
      padding: const EdgeInsets.all(8.0),
      child: ElevatedButton(
        onPressed: null,
        child: Text("Blue"),
      ),
    ),
  ],
)




onPressedにnullを指定しているので、ボタンが無効化されていて押せない。

問題を自動で作る
画面はとりあえずできたので、ロジックを作る。

ゲームで出題される問題は、単純に作れるため毎回ランダムに生成するようにしよう。候補となる文字とその色をそれぞれ配列で保持しておいて、それぞれを乱数で選択することにする。

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});
  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

List<String> texts = [
  "あか", "みどり", "あお"
];

List<Color> colors = [
  Colors.red, Colors.green, Colors.blue
];

現状は上のような3つとした。次に、StatefulWidget内で現在の問題が何なのかを保持しておく変数を用意する。

class _MyHomePageState extends State<MyHomePage> {
   late Color currentColor;
   late String currentText;

//...

}

場所ができたので、先程の配列から色とテキストを選び、変数に格納するメソッドupdateProblem()を作ろう。このメソッドをページの最初の読み込み時に実行されるinitState()内で呼び出すことで、ページ表示ごとにランダムな値が変数に入ることになる。

class _MyHomePageState extends State<MyHomePage> {
 late Color currentColor;
 late String currentText;

   @override
   void initState() {
     super.initState();
     updateProblem();
   }

   void updateProblem() {
     currentColor = colors[Random().nextInt(3)];
     currentText = texts[Random().nextInt(3)];
   }

//...

}

次に画面で変数を使うように変更する。

Text("文字の色のボタンを押そう", style: TextStyle(fontSize: 30),),
Padding(
 padding: const EdgeInsets.all(32.0),
 child: Text(
     "あお",
     currentText,
     style: TextStyle(color: Colors.green, fontSize: 100),
     style: TextStyle(color: currentColor, fontSize: 100),
 ),
),

ここまで実装すると、画面を更新するごとに別の問題が出てくるようになっている。


問題に答える
問題が自動で作れるようになったので、ユーザが問題に答え、正解不正解の判断ができるようにしよう。checkAnswer()というメソッドを作り、呼び出したときに答えの判定と次の問題への移動を行う。

  void checkAnswer(Color guess) {
    if (currentColor == guess) {
      print("正解!");
    }
    else {
      print("不正解...");
    }
    setState(() {
      updateProblem();
    });
  }

引数としてユーザが選んだ色を受け取り、現在の色と一致していたら正解、不一致なら不正解と標準出力に表示するようにしている。また、setState()の中でupdateProblem()を呼び出すことで、次の問題を変数に入れ、画面が更新される。

ボタンが押されるたびにこのメソッドが呼ばれるよう変更する。それぞれのElevatedButtonのonPressedに値を入れ、その中でcheckAnswer()を呼ぶ。引数にはそれぞれのボタンの表示に合った色を渡す。

 Row(
   mainAxisSize: MainAxisSize.min,
   children: [
     Padding(
       padding: const EdgeInsets.all(8.0),
       child: ElevatedButton(
           onPressed: null,
           onPressed: () {
             checkAnswer(Colors.red);
           },
         child: Text("Red"),
       ),
     ),
     Padding(
       padding: const EdgeInsets.all(8.0),
       child: ElevatedButton(
           onPressed: null,
           onPressed: () {
             checkAnswer(Colors.green);
           },
         child: Text("Green"),
       ),
     ),
     Padding(
       padding: const EdgeInsets.all(8.0),
       child: ElevatedButton(
           onPressed: null,
           onPressed: () {
             checkAnswer(Colors.blue);
           },
         child: Text("Blue"),
       ),
     ),
   ],
 )




ボタンを押すことで、標準出力に正解不正解が表示され、次の問題になることが確認できる。

発展課題
問題のレパートリーを増やす
正答率/正答数を表示する
Timer/Stopwatchを使い、経過時間を表示する
25問解いたら終了し、結果画面を表示する
Twitterシェアボタンを作る
Webに公開する
ビルド
さて、自分で作ったWebアプリがflutter runでばっちり動くことを確認できたら次は他の人も遊べるようにしてみたいと思うかもしれない。
それをするためには、Flutter Webはビルドという作業が必要になる。

flutter runで動かしたときは開発者が動かしやすいように、バーチャルマシン上で動かしていたりデバッグ用の重いプログラムを一緒に動かしていたりする。一方アプリとして配布する場合は開発者を意識する必要はないため、いらないものを削除するなど様々な最適化をしてからパッケージングする。特にFlutter Webではどのブラウザでも動くようにDartで書かれた物をJavaScriptに変換(トランスパイル)する。この変換工程をビルドと呼ぶ。

ビルドしたい場合はコマンドでflutter build webを打ち込む。



ビルドが成功すると、プロジェクトのbuild/web/というフォルダにindex.htmlなどのファイルが生成される。このフォルダごと配信することで、Webアプリとして配布することが可能になっている。



Netlifyを使って公開する
実際にWebサイトを公開したい場合、サイトを置いておくためのサーバが必要である。しかしFlutter Webのような単純なWebアプリであればホスティングサービスというものを利用することで簡単に安く(多くの場合無料で)公開することが可能になっている。

今回は代表的なホスティングサービスの一つである、Netlifyを使ってみよう。

https://www.netlify.com/

上記のサイトでSign upすると、下のような画面が出てくる。



この下部、Drag and drop your site output folder here と書かれた部分に先程のbuild/web/フォルダをフォルダごとドラッグアンドドロップする。



しばらく待つと、URLが表示される。これで全世界に公開完了！

URLにPCなりスマホなりでアクセスすれば、三目並べで遊べることがわかる。



おしまい

https://zenn.dev/fastriver/books/caff13da8b852c/viewer/new5new