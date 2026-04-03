[C#言語2026 第05回]

# 配列とfor文の真の姿

## キーポイント

* 配列は「同じ型のデータを複数まとめて扱う」機能
* 配列を宣言するには次のように書く
  ```c#
  型[] 配列名 = { データ0, データ1, ... };
  型[] 配列名 = new 型[データ数];
  ```
* 配列の`a`番目の変数を読み書きするには次のように書く
  ```c#
  配列名[a];
  ```
  このとき、`[`と`]`の中に書く数値のことを「添字(そえじ)」という
* 添字は`0`から数える<br>
  例えば長さ`5`の配列の場合、有効な添字は`0`,`1`,`2`,`3`,`4`で、`5`は使えない
* for文には「真の姿」があり、次のように書く
  ```c#
  for (添字変数の宣言; 繰り返し条件; 添字変数の更新)
  {
    繰り返したいプログラム
  }
  ```

## 1 配列(Array)型

### 1.1 配列の宣言

「配列(はいれつ)」は、同じ型のデータを複数まとめて扱うための機能です。<br>
配列は、次のような用途で役立ちます。

* 多数の敵
* 多数のアイテム
* 多数の弾丸
* マップデータ

配列の宣言には、「初期値を指定するバージョン」と「要素数を指定するバージョン」があります。「初期値を指定するバージョン」は次のように書きます。

```c#
型[] 配列名 = { 初期値1, 初期値2, 初期値3, ... };
```

この書きかたでは、`{`と`}`のあいだに必要なだけ初期値を指定します。指定した数が、配列の要素数になります。以下の例は、初期値に`2`、`3`、`5`を指定した`int`型の配列の宣言です。

```c#
int[] a = { 2, 3, 5 };
```

もうひとつの、「要素数を指定するバージョン」は次のように書きます。配列は「複雑な型」に分類されるので、`new`(ニュー)命令を使って作成します。

```c#
型[] 配列名 = new 型[個数];
```

この書きかたでは、それぞれの要素の初期値は`0`(数値型の場合)または`""`(文字列の場合)になります。以下の例は、`a`という名前で、`int`型の`3`個の配列を宣言します。

```c#
int[] b = new int[3];
```

例として、5体の敵の座標を変数にすることを考えます。普通の変数を使う場合は、次のような宣言になるでしょう。

```c#
float enemyX1 = 0.0f;
float enemyY1 = 0.0f;

float enemyX2 = 0.0f;
float enemyY2 = 0.0f;

float enemyX3 = 0.0f;
float enemyY3 = 0.0f;

float enemyX4 = 0.0f;
float enemyY4 = 0.0f;

float enemyX5 = 0.0f;
float enemyY5 = 0.0f;
```

もっと敵を増やしたい場合は、このような宣言を必要なだけ増やします。この方法では、敵を増やすたびに新しい変数宣言も増やす必要があります。

同じことを配列を使って宣言すると、次のようになります。

```c#
float[] enemyX = new float[5];
float[] enemyY = new float[5];
```

このように、同じ目的(例えば「敵の座標」とか)のデータが複数ある場合、配列を使うほうがずっと宣言が短くなります。また、数字を変えるだけで、簡単に要素数を増やしたり減らしたりできます。

### 1.2 配列の読み書き

配列の要素を読み書きするには、`[`と`]`のあいだに番号を指定します。番号は、先頭が`0`で、次が`1`、その次が`2`、のようになります。この番号のことを「添字(そえじ)」といいます。

```c#
a[1]
```

例えば、配列`a`の「先頭から3個目」の要素をコンソールに表示するには、次のように添字に`2`を指定します。

```c#
int[] a = { 2, 3, 5 };
Console.WriteLine(a[2]);
```

**実行結果**<br>
&emsp;5

要素に代入するには次のように書きます。

```c#
int[] a = { 2, 3, 5 };
Console.WriteLine(a[0]);
a[0] = 10;
Console.WriteLine(a[0]);
```

**実行結果**<br>
&emsp;2
&emsp;10

### 1.3 ジャンプゲームのサボテンを増やす

配列の練習として、ジャンプゲームのサボテンの数を増やしましょう。Visual Studioを起動して、`JumpGame`プロジェクトを開いてください。

そして、サボテンのX座標をあらわす変数を、次のように配列に変更してください。

```diff
 // プレイヤーの変数
 float playerX = 4.0f; // プレイヤーのX座標
 float playerY = 5.0f; // プレイヤーのY座標

 // サボテンの変数
-float sabotenX = 22.0f; // サボテンのX座標
+float[] sabotenX = { 22.0f, 20.0f, 12.0f }; // サボテンのX座標
 float sabotenY = 5.0f;  // サボテンのY座標

 // 地面を表示
 Console.WriteLine(""); // 0行目
 Console.WriteLine(""); // 1行目
```

どのサボテンも地面のすぐ上に表示されるので、Y座標は同じはずです。ですから、Y座標の変数はひとつで十分です。

### 1.4 サボテンの表示を配列に対応させる

`sabotenX`変数を配列に変えたため、`sabotenX`変数を使っているプログラムが動かなくなっています。そこで、それらのプログラムを配列に対応させて、意図したとおりにに動くようにしましょう。現在、`sabotenX`変数は以下のプログラムで使われています。

1. サボテンを表示する
2. サボテンを動かす
3. 衝突判定
4. リスタート

それでは、「サボテンを表示する」プログラムから変更しましょう。サボテンを表示するプログラムを、次のように変更してください。

```diff
 Console.WriteLine(""); // 5行目
 Console.WriteLine("🧱🧱🧱🧱🧱🧱🧱🧱🧱🧱🧱"); // レンガ12個

 for (; ; )
 {
   // サボテンを表示
-  Console.SetCursorPosition((int)sabotenX, (int)sabotenY);
+  Console.SetCursorPosition((int)sabotenX[0], (int)sabotenY);
   Console.WriteLine("🌵");
+  Console.SetCursorPosition((int)sabotenX[1], (int)sabotenY);
+  Console.WriteLine("🌵");
+  Console.SetCursorPosition((int)sabotenX[2], (int)sabotenY);
+  Console.WriteLine("🌵");

   // サボテンを動かす
   sabotenX -= 0.002f;
   if (sabotenX < 0.0f)
```

基本的に、配列に対応させるというのは「変数に添字を付け」て、「配列の数だけプログラムをコピペ」すれば実現できます。

### 1.5 サボテンの移動を配列に対応させる

次に、「サボテンを動かすプログラム」を配列に対応させます。サボテンを動かすプログラムを、添字`0`のサボテンを動かすように変更してください。

```diff
   Console.SetCursorPosition((int)sabotenX[1], (int)sabotenY);
   Console.WriteLine("🌵");
   Console.SetCursorPosition((int)sabotenX[2], (int)sabotenY);
   Console.WriteLine("🌵");

   // サボテンを動かす
-  sabotenX -= 0.002f;
-  if (sabotenX < 0.0f)
+  sabotenX[0] -= 0.002f;
+  if (sabotenX[0] < 0.0f)
   {
     // サボテンを消す
     Console.SetCursorPosition(0, (int)sabotenY);
     Console.WriteLine("  "); // 絵文字の幅は空白2個

-    sabotenX = 22.0f; // 右端に戻す
+    sabotenX[0] = 22.0f; // 右端に戻す
   }

   // スペースキーの状態を調べる
   if (isJumping == false && GetAsyncKeyState(VkSpace) < 0)
```

もちろん、これは`sabotenX[0]`変数を動かしているだけです。`sabotenX[1]`と`sabotenX[2]`を動かすプログラムは、新しく追加しなくてはなりません。`sabotenX[0]`を動かすプログラムの下に、次のプログラムを追加してください。

```diff
   // サボテンを動かす
   sabotenX[0] -= 0.002f;
   if (sabotenX[0] < 0.0f)
   {
     // サボテンを消す
     Console.SetCursorPosition(0, (int)sabotenY);
     Console.WriteLine("  "); // 絵文字の幅は空白2個

     sabotenX[0] = 22.0f; // 右端に戻す
   }
+
+  sabotenX[1] -= 0.002f;
+  if (sabotenX[1] < 0.0f)
+  {
+    // サボテンを消す
+    Console.SetCursorPosition(0, (int)sabotenY);
+    Console.WriteLine("  "); // 絵文字の幅は空白2個
+
+    sabotenX[1] = 22.0f; // 右端に戻す
+  }
+
+  sabotenX[2] -= 0.002f;
+  if (sabotenX[2] < 0.0f)
+  {
+    // サボテンを消す
+    Console.SetCursorPosition(0, (int)sabotenY);
+    Console.WriteLine("  "); // 絵文字の幅は空白2個
+
+    sabotenX[2] = 22.0f; // 右端に戻す
+  }

   // スペースキーの状態を調べる
   if (isJumping == false && GetAsyncKeyState(VkSpace) < 0)
```

ここで追加したプログラムは、`sabotenX[0]`のプログラムをコピペして、添字の番号を変えただけです。

### 1.6 衝突判定を配列に対応させる

続いて、「衝突判定」を配列に対応させます。この部分のプログラムは少し長いので、コピペすると読みづらくなりそうです。そこで、衝突に関連するプログラムを以下の2つの部分に分けます。

1. 衝突を判定するプログラム
2. 衝突した場合に実行するプログラム

衝突判定プログラムを、次のように変更してください。

```diff
   // 操作キャラクターを表示
   Console.SetCursorPosition((int)playerX, (int)playerY);
   Console.WriteLine("🧍");

   // 操作キャラクターとサボテンの座標が同じだったら衝突
+  bool isHit = false; // 衝突したらtrue
-  if ((int)playerX == (int)sabotenX && (int)playerY == (int)sabotenY)
+  if ((int)playerX == (int)sabotenX[0] && (int)playerY == (int)sabotenY)
   {
+    isHit = true;
+  }
+
+  // 衝突していたらゲームオーバー
+  if (isHit)
+  {
     Console.SetCursorPosition(0, 0);
     Console.WriteLine("GAME OVER");
     Console.WriteLine("[Enterキーでリスタート]");
     Console.ReadLine();
```

`bool`型の変数`isHit`(イズ・ヒット、「命中してる？」のような意味)には、衝突判定プログラムの結果が代入されます。`isHit`が`true`の場合のみ「衝突した場合に実行するプログラム」が実行されます。

プログラムの分離ができたので、配列に対応させるためにコピペしましょう。衝突を判定するプログラムの下に、次のプログラムを追加してください。

```diff
   // 操作キャラクターとサボテンの座標が同じだったら衝突
   bool isHit = false; // 衝突したらtrue
   if ((int)playerX == (int)sabotenX[0] && (int)playerY == (int)sabotenY)
   {
     isHit = true;
   }
+  if ((int)playerX == (int)sabotenX[1] && (int)playerY == (int)sabotenY)
+  {
+    isHit = true;
+  }
+  if ((int)playerX == (int)sabotenX[2] && (int)playerY == (int)sabotenY)
+  {
+    isHit = true;
+  }

   // 衝突していたらゲームオーバー
   if (isHit)
   {
```

### 1.7 リスタートを配列に対応させる

最後に、リスタートのプログラムを配列に対応させます。といっても、`sabotenX`を使っているのは「サボテンを右端に移動」するプログラムだけです。ということで、サボテンを右端に移動するプログラムを、次のように変更してください。

```diff
     Console.SetCursorPosition(0, 0);
     Console.WriteLine("GAME OVER");
     Console.WriteLine("[Enterキーでリスタート]");
     Console.ReadLine();

     // サボテンを右端に移動
-    sabotenX = 22.0f;
+    sabotenX[0] = 22.0f;
+    sabotenX[1] = 20.0f;
+    sabotenX[2] = 12.0f;

     // GAME OVERの文章を消す
     Console.SetCursorPosition(0, 0);
     Console.WriteLine("         "); // 空白9個
     Console.WriteLine("               "); // 空白15個
```

ここまでのプログラムが全て書けたら、上部の`>JumpGame`をクリックしてアプリを実行してください。3つのサボテンが流れてきたら成功です。

<div style="page-break-after: always"></div>

## 2. for文の真の力を解放する

### 2.1 配列とfor文

配列を使うと、複数の変数をひとつにまとめられます。しかし、配列を使う場面では、普通の変数と同じようにひとつずつプログラムを書かなくてはなりません。

例えば、3要素の配列を使ったサボテンの表示プログラムは、次のようになっていました。

```c#
// サボテンを表示
Console.SetCursorPosition((int)sabotenX[0], (int)sabotenY);
Console.WriteLine("🌵");
Console.SetCursorPosition((int)sabotenX[1], (int)sabotenY);
Console.WriteLine("🌵");
Console.SetCursorPosition((int)sabotenX[2], (int)sabotenY);
Console.WriteLine("🌵");
```

このプログラムでは、添字以外は同じプログラムを、配列の要素数だけ **繰り返し** 書いています。

・・・今、「繰り返し」と言いましたよね？

繰り返しといえばfor文です。for文を使ったらもっと簡単に書けるのではないでしょうか。例えば、次のように **添字を変数にする** のはどうでしょう。

```c#
// サボテンを表示
int a = 0; // 添字
for (; ; )
{
  Console.SetCursorPosition((int)sabotenX[a], (int)sabotenY);
  Console.WriteLine("🌵");
  a += 1; // 添字をひとつ増やす
}
```

これは悪くなさそうです。繰り返しの最後で変数`a`を`1`増やしているので、繰り返しのたびに`sabotenX[0]`、`sabotenX[1]`、`sabotenX[2]`、と添字が変わります。

しかし、`for(;;)`は無限ループなので、添字は無限に増え続けます。ところが、`sabotenX`の要素は`3`個しかありません。つまり、添字は`2`までしか増やせないのです。

そこで、添字が`3`未満のときだけプログラムを実行し、`3`以上になったら`break`文で繰り返しを終わらせます。これは、次のようなプログラムになります。

```c#
// サボテンを表示
int a = 0; // 添字
for (; ; )
{
  // 条件がtrueならプログラムを実行
  if (a < 3)
  {
    Console.SetCursorPosition((int)sabotenX[a], (int)sabotenY);
    Console.WriteLine("🌵");
    a += 1; // 添字をひとつ増やす
  }
  else
  {
    break; // 条件がfalseになったらブレイクする
  }
}
```

このように、for文と変数をうまく組み合わせると、配列のすべての要素を表示することができます。

### 2.2 for文の真の姿

for文を使うと、何度も`Console.SetCursorPosition`や`Console.WriteLine`を書かなくて済みます。ですが、プログラムの行数は増えているし、あまり読みやすくもありません。コピペで済むぶん、for文を使わないほうが簡単に書けてよい気もします。

しかし実は、for文には「真の姿」があるのです。for文を「真の姿」にするには、`(; ; )`の部分に繰り返しを制御するプログラムを書きます。具体的には、以下の無限for文を、

```c#
添字変数の宣言
for (; ; )
{
  if (繰り返し条件)
  {
    繰り返したいプログラム
    添字を増やす
  }
  else
  {
    break;
  }
}
```

次のように書き直せるのです。

```c#
for (添字変数の宣言; 繰り返し条件; 添字を増やす)
{
  繰り返したいプログラム
}
```

サボテンを表示するプログラムを使って、for文を少しずつ真の姿に変えてみましょう。真の姿に変えるには、以下の3段階の手順を踏みます。

1. 添字変数の宣言の移動
2. 繰り返し条件の移動
3. 添字を増やすプログラムの移動

まずは、「添字変数の宣言」を`(; ; )`の先頭に移動します。それには、プログラムを次のように変更します。

```diff
 // サボテンを表示
-int a = 0; // 添字
+for (int a = 0; ; )
 {
   // 条件がtrueならプログラムを実行
   if (a < 3)
   {
     Console.SetCursorPosition((int)sabotenX[a], (int)sabotenY);
     Console.WriteLine("🌵");
     a += 1; // 添字をひとつ増やす
   }
   else
   {
     break; // 条件がfalseになったらブレイクする
   }
 }
```

次に、「繰り返し条件」を`(; ; )`の`;`と`;`の間、つまり中央部分に移動します。それには、プログラムを次のように変更します。

```diff
 // サボテンを表示
-for (int a = 0; ; )
+for (int a = 0; a < 3; )
 {
-  // 条件がtrueならプログラムを実行
-  if (a < 3)
-  {
     Console.SetCursorPosition((int)sabotenX[a], (int)sabotenY);
     Console.WriteLine("🌵");
     a += 1; // 添字をひとつ増やす
-  }
-  else
-  {
-    break; // 条件がfalseになったらブレイクする
-  }
 }
```

`else`句と`break`文が無くなっている点に注目してください。「真の姿」の能力によって、条件が`false`になると、for文が自動的に`break`してくれるからです。

最後に、「添字を増やすプログラム」を`(; ; )`の後半部分に移動します。それには、プログラムを次のように変更します。

```diff
 // サボテンを表示
-for (int a = 0; a < 3; )
+for (int a = 0; a < 3; a += 1)
 {
   Console.SetCursorPosition((int)sabotenX[a], (int)sabotenY);
   Console.WriteLine("🌵");
-  a += 1; // 添字をひとつ増やす
 }
```

全てのプログラムを移動して、真の姿になったfor文は次のようになります。

```c#
// サボテンを表示
for (int a = 0; a < 3; a += 1)
{
  Console.SetCursorPosition((int)sabotenX[a], (int)sabotenY);
  Console.WriteLine("🌵");
}
```

元の無限ループのfor文と比べてみてください。動作は同じでも、「真の姿」を使ったプログラムのほうが、ずっと短い行数で書くことができます。

ただし、for文の「真の姿」を使いこなすには、`(; ; )`のどの部分にどんなプログラムを書くかを、しっかりと理解しなくてはなりません。for文の「真の姿」を使いこなすには、プログラム経験を積む必要があるのです。

### 2.3 配列の要素数

配列の要素数は、配列から直接調べることができます。配列の要素数を調べるには、`Length`(レングス)プロパティを使います。

>「プロパティ」は「性質」や「資産」という意味です。C#のプロパティは、必要に応じて読み取りまたは代入を禁止できる「ちょっと特殊な変数」となっています。例えば、`Length`は「読み取り専用」で、代入はできません。`a.Length = 1;`のように書くとエラーになります。「複雑な型」は固有のプロパティを持ちます。

```c#
int[] a = { 2, 3, 5 };
Console.WriteLine(a.Length);
```

**実行結果**<br>
&emsp;3

`Lenght`プロパティを使うと、サボテンを表示するプログラムは次のように変更できます。

```diff
 // サボテンを表示
-for (int a = 0; a < 3; a += 1)
+for (int a = 0; a < sabotenX.Length; a += 1)
 {
   Console.SetCursorPosition((int)sabotenX[a], (int)sabotenY);
   Console.WriteLine("🌵");
 }
```

上のプログラムでは、要素数の`3`という数字が、`sabotenX.Length`(サボテン・エックス・レングス)プロパティに置き換わっています。

現在、`sabotenX`配列の要素数は`3`なので、`sabotenX.Lenght`には`3`が代入されています。`sabotenX`の要素数が変わると、`Length`プロパティの数値も自動的に更新されます。

つまり、`Lenght`プロパティを使うと、配列の要素数が変化したときにfor文の「繰り返し条件」を書き換える必要がなくなります。

### 2.4 for文の真の姿を使う

for文の「真の姿」の練習として、ジャンプゲームのプログラムを真の姿に書き換えましょう。`JumpGame`プロジェクトを開き、サボテンを表示するプログラムを次のように変更してください。

```diff
 for (; ; )
 {
   // サボテンを表示
+  for (int a = 0; a < sabotenX.Length; a += 1)
+  {
-    Console.SetCursorPosition((int)sabotenX[0], (int)sabotenY);
+    Console.SetCursorPosition((int)sabotenX[a], (int)sabotenY);
     Console.WriteLine("🌵");
+  }
-  Console.SetCursorPosition((int)sabotenX[1], (int)sabotenY);
-  Console.WriteLine("🌵");
-  Console.SetCursorPosition((int)sabotenX[2], (int)sabotenY);
-  Console.WriteLine("🌵");

   // サボテンを動かす
   sabotenX[0] -= 0.002f;
   if (sabotenX[0] < 0)
```

`sabotenX`配列を使っている、他のプログラムも「真の姿」に書き換えましょう。サボテンを動かすプログラムを、次のように変更してください。

```diff
   // サボテンを表示
   for (int a = 0; a < sabotenX.Length; a += 1)
   {
     Console.SetCursorPosition((int)sabotenX[a], (int)sabotenY);
     Console.WriteLine("🌵");
   }

   // サボテンを動かす
+  for (int a = 0; a < sabotenX.Length; a += 1)
+  {
-    sabotenX[0] -= 0.002f;
-    if (sabotenX[0] < 0.0f)
+    sabotenX[a] -= 0.002f;
+    if (sabotenX[a] < 0.0f)
     {
     // サボテンを消す
     Console.SetCursorPosition(0, (int)sabotenY);
     Console.WriteLine("  "); // 絵文字の幅は空白2個

-    sabotenX[0] = 22.0f; // 右端に戻す
+    sabotenX[a] = 22.0f; // 右端に戻す
     }
+  }
-
-  sabotenX[1] -= 0.002f;
-  if (sabotenX[1] < 0.0f)
-  {
-    // サボテンを消す
-    Console.SetCursorPosition(0, (int)sabotenY);
-    Console.WriteLine("  "); // 絵文字の幅は空白2個
-
-    sabotenX[1] = 22.0f; // 右端に戻す
-  }
-
-  sabotenX[2] -= 0.002f;
-  if (sabotenX[2] < 0.0f)
-  {
-    // サボテンを消す
-    Console.SetCursorPosition(0, (int)sabotenY);
-    Console.WriteLine("  "); // 絵文字の幅は空白2個
-
-    sabotenX[2] = 22.0f; // 右端に戻す
-  }

   // スペースキーの状態を調べる
   if (isJumping == false && GetAsyncKeyState(VkSpace) < 0)
```

for文の「真の姿」は、元のプログラムが長いほど、プログラムを短くする効果が高くなります。

続いて、衝突判定プログラムを「真の姿」で書き直しましょう。衝突判定プログラムを、次のように変更してください。

```diff
   // 操作キャラクターとサボテンの座標が同じだったら衝突
   bool isHit = false; // 衝突したらtrue
+  for (int a = 0; a < sabotenX.Length; a += 1)
+  {
-    if ((int)playerX == (int)sabotenX[0] && (int)playerY == (int)sabotenY)
+    if ((int)playerX == (int)sabotenX[a] && (int)playerY == (int)sabotenY)
     {
       isHit = true;
     }
+  }
-  if ((int)playerX == (int)sabotenX[1] && (int)playerY == (int)sabotenY)
-  {
-    isHit = true;
-  }
-  if ((int)playerX == (int)sabotenX[2] && (int)playerY == (int)sabotenY)
-  {
-    isHit = true;
-  }

   // 衝突していたらゲームオーバー
   if (isHit)
   {
```

### 2.5 真の姿を使えない場合

`sabotenX`配列を使っているプログラムは、あとひとつ残っています。それは、リスタート時に「サボテンを右端に移動」するプログラムです。

```c#
    // サボテンを右端に移動
    sabotenX[0] = 22.0f;
    sabotenX[1] = 20.0f;
    sabotenX[2] = 12.0f;
```

ですが、ここではfor文の「真の姿」を使えません。なぜなら、代入する数値が違うからです。「真の姿」は、「添字以外はすべて同じプログラム」に対してしか使えないのです。

とりあえず、2.4までのプログラムが書けたら、`>JumpGame`をクリックしてアプリを実行してください。for文を真の姿に書き換える前と全く同じ動作をしていたら成功です。

### 2.6 真の姿を使えるようにする

for文の真の姿は、「添字以外はすべて同じプログラム」でなくては使えません。裏を返せば、「添字以外はすべて同じプログラム」に変えてしまえば、for文の真の姿を解放できるわけです。

「サボテンを右端に移動」するプログラムの場合、例えば、「代入する数値を乱数で置き換える」という方法が使えます。乱数は毎回違う数値になりますが、プログラムは乱数を使うという点で同じにできます。

乱数を使うには、`Random`(ランダム)型の変数を作成します。`Program.cs`ファイルの先頭に、次のプログラムを追加してください。

```diff
 // 絵文字を使用可能にする
 Console.OutputEncoding = System.Text.Encoding.UTF8;
+
+Random rand = new();  // ランダム作成用の変数

 // タイトルを表示して、Enterキーの入力を待つ
 Console.WriteLine("ジャンプゲーム");
 Console.WriteLine("[Enterでゲーム開始]");
 Console.ReadLine();
```

「サボテンを右端に移動」するプログラムは、実際には全てのサボテンを右端に移動しているわけではありません。

自然な雰囲気を出すために、最初のサボテン以外は、右端から少し離れた座標に移動させています。また、絵文字のサイズを考慮して、設定する座標は2の倍数になっています。

これらのことを考慮すると、サボテンを移動できる座標は`12`, `14`, `16`, `18`, `20`, `22`の6種類になります。そこで、`0～5`の乱数を作成して`2`倍し、`12`を足します。これで、サボテンの座標を作り出すことができます。

それでは、サボテンを右端に移動するプログラムを、次のように変更してください。

```diff
     Console.SetCursorPosition(0, 0);
     Console.WriteLine("GAME OVER");
     Console.WriteLine("[Enterキーでリスタート]");
     Console.ReadLine();

     // サボテンを右端に移動
-    sabotenX[0] = 22.0f;
-    sabotenX[1] = 20.0f;
-    sabotenX[2] = 12.0f;
+    sabotenX[0] = rand.Next(6) * 2 + 12;
+    sabotenX[1] = rand.Next(6) * 2 + 12;
+    sabotenX[2] = rand.Next(6) * 2 + 12;

     // GAME OVERの文章を消す
     Console.SetCursorPosition(0, 0);
     Console.WriteLine("         "); // 空白9個
     Console.WriteLine("               "); // 空白15個
```

この変更により、「サボテンごとに違う座標に移動」させながら、「添字以外はすべて同じプログラム」を作り出せました。

あとは、for文の真の姿で書き換えるだけです。サボテンを右端に移動するプログラムを、次のように変更してください。

```diff
     Console.SetCursorPosition(0, 0);
     Console.WriteLine("GAME OVER");
     Console.WriteLine("[Enterキーでリスタート]");
     Console.ReadLine();

     // サボテンを右端に移動
-    sabotenX[0] = rand.Next(6) * 2 + 12;
-    sabotenX[1] = rand.Next(6) * 2 + 12;
-    sabotenX[2] = rand.Next(6) * 2 + 12;
+    for (int a = 0; a < sabotenX.Length; a += 1)
+    {
+      sabotenX[a] = rand.Next(6) * 2 + 12;
+    }

     // GAME OVERの文章を消す
     Console.SetCursorPosition(0, 0);
     Console.WriteLine("         "); // 空白9個
     Console.WriteLine("               "); // 空白15個
```

プログラムが書けたら、`>JumpGame`をクリックしてアプリを実行してください。ゲームをリスタートするたびに、サボテンの位置が変わっていたら成功です。

### 2.7 サボテンを戻す座標をランダムにする

ここまでの流れのついでに、左端に凸龍したサボテンを右端に戻すときも、座標をランダムにしましょう。サボテンを右端に戻るプログラムを次のように変更してください。

```diff
   // サボテンを動かす
   for (int a = 0; a < sabotenX.Length; a += 1)
   {
     sabotenX[a] -= 0.002f;
     if (sabotenX[a] < 0.0f)
     {
     // サボテンを消す
     Console.SetCursorPosition(0, (int)sabotenY);
     Console.WriteLine("  "); // 絵文字の幅は空白2個

-    sabotenX[a] = 22.0f; // 右端に戻す
+    sabotenX[a] = rand.Next(10) * 2 + 22; // ランダムな位置に戻す
     }
   }
```

乱数の範囲は適当です。`22`～`40`の範囲の偶数の座標を選ぶようにしてみました。範囲を広くするほどサボテンの出現間隔のバリエーションが増えて、予測しにくくなります。

しかし、地面の右端は`22`なので、それより右にサボテンがある場合は表示しないようにしたいです。サボテンを表示するプログラムを、次のように変更してください。

```diff
   // サボテンを表示
   for (int a = 0; a < sabotenX.Length; a += 1)
   {
+    if (sabotenX <= 22)
+    {
       Console.SetCursorPosition((int)sabotenX[a], (int)sabotenY);
       Console.WriteLine("🌵");
+    }
   }

   // サボテンを動かす
   for (int a = 0; a < sabotenX.Length; a += 1)
```

プログラムが書けたら、`>JumpGame`をクリックしてアプリを実行してください。サボテンの並び順がランダムに変わっていたら成功です。

<div style="page-break-after: always"></div>

## 3. ゲームの動作を改善する

### 3.1 カーソルを非表示にする

絵文字が移動するとき、画面に「細い縦棒(たてぼう)」がちらつきます。この「細い縦棒」は、文字の表示位置を示す「カーソル」です。高速で何度も表示位置を変更しているために、ちらついて見えるのです。

カーソルのちらつきをなくすには、カーソルを消してしまうのが簡単です。カーソルを消すには`Console.CursorVisible`(コンソール・カーソル・ビジブル、「コンソールにカーソルを表示するか？」という意味)プロパティに`false`を代入します。

絵文字を使用可能にするプログラムの下に、次のプログラムを追加してください。

```diff
 // 絵文字を使用可能にする
 Console.OutputEncoding = System.Text.Encoding.UTF8;
+
+// カーソルを消す
+Console.CursorVisible = false;

 Random rand = new();  // ランダム作成用の変数

 // タイトルを表示して、Enterキーの入力を待つ
 Console.WriteLine("ジャンプゲーム");
 Console.WriteLine("[Enterでゲーム開始]");
```

プログラムが書けたら、`>JumpGame`をクリックしてアプリを実行してください。カーソルのちらつきがなくなっていたら成功です。

### 3.2 サボテンの速度を一定にする

ところで、画面に表示されるサボテンの数が変わると、サボテンの移動速度も変わることに気がついたでしょうか？

サボテンの表示プログラムをたくさん実行するほど、コンピューターが実行する命令数が多くなって繰り返しに時間がかかるようになります。

時間がかかると、1秒間に実行できる繰り返しの回数が減ります。すると、サボテンを移動させるプログラムの実行回数が減って遅くなるのです。

しかし、ゲームの場合は最速で繰り返す必要はありません。1/30から1/60秒の間隔で繰り返しができれば、人間にとって十分な操作感が得られます。

繰り返しの間隔を一定にするには、以下のプログラムを追加します。

1. ストップウォッチ機能でプログラムの実行時間を測る
2. 1で測った時間が1/60秒未満の場合、`Thread.Sleep`命令を使って1/30秒経過するまでプログラムを停止する

ストップウォッチ機能を使うには、`Stopwatch`(ストップウォッチ)型の変数を宣言します。`Stopwatch`型の変数は、`new()`(ニュー)命令を使って作成します。

変数名は頭文字を小文字にした`stopwatch`(ストップウォッチ)とします。それでは、サボテンの変数を宣言するプログラムの下に、次のプログラムを追加してください。

```diff
 // カーソルを消す
 Console.CursorVisible = false;

 Random rand = new(); // ランダム作成用の変数
+Stopwatch stopwatch = new(); // ストップウォッチ変数を宣言

 // タイトルを表示して、Enterキーの入力を待つ
 Console.WriteLine("ジャンプゲーム");
 Console.WriteLine("[Enterでゲーム開始]");
```

ストップウォッチは、`Restart`(リスタート)命令で計測を開始し、`Stop`(ストップ)命令で計測を停止します。経過時間は`ElapsedMilliseconds`(エラプスド・ミリセコンズ、「経過ミリ秒」という意味)変数で調べられます。

1回の繰り返し時間を計測したいので、「繰り返しの最初」に`Restart`命令を実行します。for文の下に、次のプログラムを追加してください。

```diff
 Console.WriteLine(""); // 3行目
 Console.WriteLine(""); // 4行目
 Console.WriteLine(""); // 5行目
 Console.WriteLine("🧱🧱🧱🧱🧱🧱🧱🧱🧱🧱🧱"); // レンガ12個

 for (; ; )
 {
+  stopwatch.Restart(); // 時間計測を開始
+
   // サボテンを表示
   for (int a = 0; a < sabotenX.Length; a += 1)
   {
     if (sabotenX <= 22)
```

計測を停止するタイミングは「繰り返しの終わり」です。衝突判定の下、for文の終わりの`}`の上に、次のプログラムを追加してください。

```diff
     // GAME OVERの文章を消す
     Console.SetCursorPosition(0, 0);
     Console.WriteLine("         "); // 空白9個
     Console.WriteLine("               "); // 空白15個
   }
+
+  stopwatch.Stop(); // 時間計測を終了
 } // forの終わり
```

これで経過時間を計測できたので、繰り返し時間が1//60秒になるように調節します。それには、経過時間が1/60秒より短かったら、`Thread.Sleep`命令でプログラムを停止すればよいです。時間計測の終了プログラムの下に、次のプログラムを追加してください。

```diff
     // GAME OVERの文章を消す
     Console.SetCursorPosition(0, 0);
     Console.WriteLine("         "); // 空白9個
     Console.WriteLine("               "); // 空白15個
   }

   stopwatch.Stop(); // 時間計測を終了

+  // 経過時間が1/60秒未満の場合、1/60秒が経過するまで停止
+  if (stopwatch.ElapsedMilliseconds < 1000 / 60)
+  {
+    Thread.Sleep(1000 / 60 - stopwatch.ElapsedMilliseconds);
+  }
 } // forの終わり
```

プログラムが書けたら、`>JumpGame`をクリックしてアプリを実行してください。サボテンの移動速度がものすごく遅く(約20秒に1回動く)なっていたら成功です。

### 3.3 移動速度を調整する

サボテンの移動速度が遅くなった原因は、「元の繰り返しが速すぎた」からです。速すぎるので、1回に移動させる量を`0.001f`のようにかなり小さくしていたのです。

しかし、`Stopwatch`と`Thread.Sleep`によって、繰り返し時間は1/60秒に変わりました。新しい繰り返し時間に合わせて、1回に移動させる量も調整する必要があります。

なお、繰り返しの速度を1/60秒と決めたので、移動量はかなり決めやすくなりました。例えば、サボテンを2秒で横切らせたい場合の「1回の移動量」を考えます。

1. 地面の長さは全角12文字なので、半角にすると`24`文字分
2. 2秒間の繰り返し回数は`2 * 60 = 120`より`120`回
3. 24文字を120回に分けて移動するので、1回の移動量は`24 / 120 = 0.2`より`0.2`文字

つまり、サボテンの移動量を`0.2`にすれば、約2秒で画面を横切る速度にできるわけです。それでは、サボテンを動かすプログラムを、次のように変更してください。

```diff
   // サボテンを表示
   Console.SetCursorPosition((int)sabotenX, (int)sabotenY);
   Console.WriteLine("🌵");

   // サボテンを動かす
-  sabotenX -= 0.001f;
+  sabotenX -= 0.2f;
   if (sabotenX < 0.0f)
   {
     // サボテンを消す
     Console.SetCursorPosition(0, (int)sabotenY);
     Console.WriteLine("  "); // 絵文字の幅は空白2個
```

プログラムが書けたら、`>JumpGame`をクリックしてアプリを実行してください。サボテンの移動速度が普通になっていたら成功です。

### 3.4 Sleepの精度を改善する

サボテンが横切る速度を測ってみると、2.5～3.0秒になると思います。「正確に測ったのに2秒にならない」原因は、`Thread.Sleep`命令の精度が悪くて、停止時間が不正確だからです。

`Thread.Sleep`の精度を改善するには、OSの機能を使う必要があります。`Program.cs`ファイルの先頭に、次のプログラムを追加してください。

```diff
 using System.Runtime.InteropServices;

 // OSの「キーを調べる機能」を使えるようにする
 [DllImport("user32.dll")]
 static extern short GetAsyncKeyState(int key);

 // GetAsyncKeyStateで使う仮想キー番号
 const int VkSpace = 32;     // スペースキー
+
+// OSの「スリープ時間の精度を変える機能」を使えるようにする
+[DllImport("winmm.dll")]
+static extern uint timeBeginPeriod(uint uMilliseconds);
+
+timeBeginPeriod(1); // スリープの精度を1ミリ秒に設定

 // プレイヤーの変数
 float playerX = 4.0f; // プレイヤーのX座標
 float playerY = 5.0f; // プレイヤーのY座標
```

`timeBeginPeriod`(タイム・ビギン・ピリオド、「時間の制御期間を開始」という意味)命令は、スリープの最小間隔を変更します。指定単位は「ミリ秒」です。

プログラムが書けたら、`>JumpGame`をクリックしてアプリを実行してください。サボテンの移動速度が少し速くなっていたら成功です。

>「時間の制御期間」はアプリ終了と同時に終わります。OSの`timeEndPeriod`(タイム・エンド・ピリオド)命令を使って、明示的に終わらせる方法もあります。

### 3.5 ジャンプを改善する

for文の繰り返し速度を遅くしたので、ジャンプの速度もかなり遅くなっています。いい機会なので、ジャンプの高さと時間を決めましょう。ジャンプの高さは3文字、時間は1秒(上昇が0.5秒、下降が0.5秒)とします。

実際のジャンプ計算に必要な「ジャンプ速度(文字数/秒)」と「重力(文字数/秒²)」は、等加速度直線運動の公式を使って求めます。

* 0.5秒で3文字分ジャンプする → 0.5秒で3文字分落下する
* 等加速度直線運動の変位の公式 $ x(t) = v_0 t + \frac{1}{2} a t ^ 2 $ を使い、重力加速度 $ a $ 求める
  * 3文字ジャンプしたいので変位 $ x(t) $ は`3`、時間 $ t $ は`0.5`、ジャンプ頂点では速度`0`になるので、初速 $ v_0 $ は`0`と仮定
  * $ 3 = \frac{1}{2} a (0.5^2) $
  * $ 3 = 0.125a $
  * $ a = 24 $ より、重力加速度は`24`
  * 改めて初速 $ v_0 $ を求めると、
  * $ 3 = 0.5 v_0 - \frac{24}{2} 0.5^2 $
  * $ 3 = 0.5 v_0 - 3 $
  * $ 0.5 v_0 = 6 $
  * $ v_0 = 12 $ より、初速は`12`

つまり、「初速`12`、重力加速度`24`でジャンプすれば、高さが`3`文字で時間が1秒のジャンプになる」わけです。

それでは、ジャンプの計算プログラムを次のように変更してください。

```diff
   // ジャンプ
   if (isJumping == false && GetAsyncKeyState(VkSpace) < 0)
   {
     isJumping = true;
-    jumpSpeed = -3.0f;
+    jumpSpeed = -12.0f; // 初速
     Console.SetCursorPosition(0, 0);
     Console.WriteLine("jump");
   }
   else
   {
     Console.SetCursorPosition(0, 0);
     Console.WriteLine("    "); // 半角空白を4個
   }

   // ジャンプの制御
   if (isJumping)
   {
-    playerY += jumpSpeed * 0.001f; // Y座標にジャンプ速度を足す
-    jumpSpeed += 0.002f; // ジャンプ速度に重力を足す
+    playerY += jumpSpeed / 60.0f; // Y座標にジャンプ速度を足す
+    jumpSpeed += 24.0f / 60.0f; // ジャンプ速度に重力を足す

     // Y座標が地面の高さ以上になったらジャンプ終了
     if (playerY >= 5.0f)
     {
       playerY = 5.0f;    // 地面ぴったりの高さを代入
       jumpSpeed = 0.0f;  // ジャンプ速度をゼロにする
       isJumping = false; // ジャンプしてない状態にする
```

プログラムが書けたら、`>JumpGame`をクリックしてアプリを実行してください。スペースキーを押したとき、高さ3文字、時間1秒でジャンプしたら成功です。

>数学や物理の知識、ゲームを作るときに結構使います。少なくとも中学レベル、できれば高校レベルの知識があるとよいです。中学、高校の教科書で復習しておきましょう。
