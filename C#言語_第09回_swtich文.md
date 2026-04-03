[C#言語2026 第09回]

# switch文

## キーポイント

* 数値の一致判定だけを行うif～elseは、switch文で置き換えられる。
* caseとdefaultの末尾は`:`(コロン)になる。

## 1 switch(スイッチ)文

### 1.1 if～elseをswitch文で書き直す

次のようなif～elseを使ったプログラムがあるとします。

```c#
Console.WriteLine("どうする？");
Console.WriteLine("1=攻撃 2=魔法 3=薬 4=防御");

int a = int.Parse(Console.ReadLine());

if (a == 1)
{
  Console.WriteLine("剣で攻撃！");
}
else if (a == 2)
{
  Console.WriteLine("魔法で攻撃！");
}
else if (a == 3)
{
  Console.WriteLine("回復薬を使った！");
}
else
{
  Console.WriteLine("守りを固めた！");
}
```

**実行結果**

```txt
どうする？
1=攻撃 2=魔法 3=薬 4=防御
2
魔法で攻撃！
```

この例のように、「変数の値に対応する処理に分岐させる」というプログラムでは、数値以外は全く同じif文を、何度も繰り返し書くことになります。これは面倒ですし、例えば「変数名を変えたい」という場合は、全部のif文を書き直す必要があります。

この手間を減らすために、C#言語には`switch`(スイッチ)文という機能が用意されています。

次のプログラムは、switch文を使って、前の節のプログラムを書き直したものです。

```c#
Console.WriteLine("どうする？");
Console.WriteLine("1=攻撃 2=魔法 3=薬 4=防御");

int a = int.Parse(Console.ReadLine());

switch (a)
{
case 1:
  Console.WriteLine("剣で攻撃！");
  break;
case 2:
  Console.WriteLine("魔法で攻撃！");
  break;
case 3:
  Console.WriteLine("回復薬を使った！");
  break;
default:
  Console.WriteLine("守りを固めた！");
  break;
}
```

**実行結果**

```txt
どうする？
1=攻撃 2=魔法 3=薬 4=防御
2
魔法で攻撃！
```

このプログラムでは、変数`a`を、switch文の最初の一回しか書いていない点に注目してください。ひとつだけなので、変数名を変えるのも簡単です。

### 1.2 switch文の書きかた

switch文は「カッコ内の式と一致するラベルに処理を分岐させる」構文です。switch文は次のように書きます。

```c#
switch (式)
{
case 値１:
  プログラム;
  break;
case 値２:
  プログラム;
  break;
// 以下、caseラベルとプログラムを必要なだけ書く
    ・
    ・
    ・
default:
  プログラム;
  break;
}
```

ラベルには、`case`(ケース)ラベルと`default`(デフォルト)ラベルの２種類があります。

#### caseラベル

caseラベルはif文、またはelse if文に相当し、次のように書きます。

```c#
case パターン:
  プログラム
  break;
```

「パターン」はif文の`(条件式)`に対応します。例えば`case 1:`と書いた場合、「式の結果が`1`の場合」という意味になります。

「パターン」の後ろにあるのは`:`(コロン)です。C#言語では、「末尾にひとつの`:`記号を付けるとラベル(付箋)になる」と決められています。

#### defaultラベル

defaultラベルはelse句に相当し、次のように書きます。

```c#
default:
  プログラム
  break;
```

`default`(デフォルト)ラベルは、「いずれの`case`ラベルとも一致しない場合」の分岐先となります。`default`ラベルが不要な場合は省略できます。

>**【caseとdefaultにはコロンを書く】**<br>
>慣れないうちは、ラベルの末尾の`:`(コロン)と、文の末尾の`;`(セミコロン)を間違えがちです。<br>
>「`case`と`default`にはコロン」と覚えてください。

### 1.3 ラベルには必ずbreakを書く

「`case`または`default`を終了してswitch文を終わらせる」には`break`(ブレイク)文を書きます。`break`を書き忘れてしまうとエラーになります。

なお、`break`の終わりには`;`(セミコロン)が必要です。「`break`は文であり、ラベルではない」からです。

以下のプログラムでは、`break`を書き忘れています。

```c#
Console.WriteLine("どうする？");
Console.WriteLine("1=攻撃 2=魔法 3=薬 4=防御");
string a = Console.ReadLine();

switch (a)
{
case "1": // エラー. breakがない
  Console.WriteLine("剣で攻撃！");
case "2": // エラー. breakがない
  Console.WriteLine("魔法で攻撃！");
case "3": // エラー. breakがない
  Console.WriteLine("回復薬を使った！");
default: // エラー. breakがない
  Console.WriteLine("守りを固めた！");
}
```

### 1.4 breakを書かない使い方(フォールスルー)

場合によっては、「あえてbreakを書かない」ほうが便利なことがあります。<br>
以下は、威力の異なるパンチとキックを選択するプログラムです。

```c#
Console.WriteLine("どうする？");
Console.WriteLine("1=弱パンチ 2=中パンチ 3=強パンチ");
Console.WriteLine("4=弱キック 5=中キック 6=強キック");
int a = int.Parse(Console.ReadLine());

switch (a)
{
case 1:
case 2: // OK. 上のcaseにプログラムがない
case 3: // OK. 上のcaseにプログラムがない
  Console.WriteLine("パンチ攻撃！");
  Console.WirteLine(a + "のダメージを与えた！");
  Console.WriteLine("あと" + a + "ターンは動けません");
  break;
case 4:
case 5: // OK. 上のcaseにプログラムがない
case 6: // OK. 上のcaseにプログラムがない
  Console.WriteLine("キック攻撃！");
  Console.WriteLine((a - 2) + "のダメージを与えた！");
  Console.WriteLine("あと" + (a - 2) + "ターンは動けません");
  break;
}
```

**実行結果**

```txt
どうする？
1=弱パンチ 2=中パンチ 3=強パンチ
4=弱キック 5=中キック 6=強キック
5
キック攻撃！
3のダメージを与えた！
あと3ターンは動けません;
```

このように、ひとつの処理に対して複数のラベルを割り当てると、よく似た処理をまとめられます。<br>
この使い方をするときは、`case`と`case`の間にプログラムを書いてはいけません。

### 1.5 caseラベルの高度な使い方

`case`の「パターン」には整数だけでなく、実数や文字列も指定できます。次のプログラムは、文字列を比較する例です。

```c#
string s = Console.ReadLine();
switch (s)
{
case "run":
  Console.WriteLine("走る");
  break;
case "walk":
  Console.WriteLine("歩く");
  break;
case "stop":
  Console.WriteLine("立ち止まる");
  break;
default:
  Console.WriteLine(s + "をする");
  break;
}
```

さらにパターンには、「条件」を書くことができます。次のプログラムを見てください。

```c#
int a = int.Parse(Console.ReadLine());

switch (a)
{
case <= 100:
  Console.WriteLine(a + "は100以下");
  break;
case <= 200:
  Console.WriteLine(a + "は200以下");
  break;
default:
  Console.WriteLine(a + "は200より大きい");
  break;
}
```

上のプログラムの`<= 100:`というパターンは、if文で書くと`if (a <= 100)`という意味になります。

switch文でも、if文と同様に「より厳しい条件を先に書く」ようにします。例えば上のプログラムの場合、`<= 100`と`<= 200`の順序を逆にすることはできません。

>if文とswitch文のどちらが書きやすいかは状況によります。例えばif文だけ、またはif文とelse句だけ、という場合は、switch文にするメリットは少ないです。逆に「else ifがたくさん続く」ような場合は、switch文のほうが書きやすいでしょう。

### 1.6 パターンに変数は使えない

「パターン」では変数を使えません。変数と比較したい場合はif文を使ってください。例えば、以下のような変数同士の比較は「パターン」ではできません。

```c#
string a = Console.ReadLine();
string b = Console.ReadLine();
string c = Console.ReadLine();

//switch (a)
//{
//case b: // エラー. パターンに変数は使えない
//  Console.WriteLine("aとbは等しい");
//  break;
//
//case c: // エラー. パターンに変数は使えない
//  Console.WriteLine("aとcは等しい");
//  break;
//}

// switchの代わりにif文とelse if文を使う
if (a == b)
{
  Console.WriteLine("aとbは等しい");
}
else if (a == c)
{
  Console.WriteLine("aとcは等しい");
}
```

### 1.7 switchブロック

switch文は全体でひとつのブロックになります。また、ラベルはブロックを作りません。そのため、ラベルが違っても、同じ名前の変数は定義できません。

```c#
int a = int.Parse(Console.ReadLine());
switch (a)
{
case 1:
  int b = 100;
  Console.WriteLine(b); 
  break;

case 2:
  //int b = 200; // エラー. 変数bは上のcaseで定義されている。
  int c = 200;
  Console.WriteLine(c);
  break;

default:
  //int b = 300; // エラー. 変数bは上のcaseで定義されている。
  //int c = 300; // エラー. 変数cは上のcaseで定義されている。
  int d = 200;
  Console.WriteLine(d);
  break;

} // ←ここでswitchブロックが終わり、変数b, c, dが削除される
```

同じ名前の変数を定義したい場合は、次のように「無名ブロック」を使います。

```c#
int a = int.Parse(Console.ReadLine());
switch (a)
{
case 1:
  { // ←無名ブロックの開始
    int b = 100;
    Console.WriteLine(b); 
  } // ←無名ブロックの終わり(変数bが削除される)
  break;

case 2:
  { // ←無名ブロックの開始
    int b = 200; // OK. ブロックが異なるので同じ名前の変数を定義できる
    Console.WriteLine(b);
  } // ←無名ブロックの終わり(変数bが削除される)
  break;

default:
  { // ←無名ブロックの開始
    int b = 200; // OK. ブロックが異なるので同じ名前の変数を定義できる
    Console.WriteLine(b);
  } // ←無名ブロックの終わり(変数bが削除される)
  break;

} // ←ここでswitchブロックが終わる(変数は削除済みなので何も起こらない)
```

このような、`{`と`}`だけで作られたブロックを「無名ブロック」、または「匿名ブロック」といいます。

<div style="page-break-after: always"></div>

