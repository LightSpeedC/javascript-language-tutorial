Ruby vs JavaScript (Node.js)
============================

node -v

ruby 2.0.0 x64

終了コマンド
------------

会話型実行ツールから脱出する方法

irb
> exit
> quit
> abort
> exit!
> ^D


node
> .exit
> ^D
> ^C^C
> process.exit()


nil と null と undefined
------------------------

ruby: nil (変数の初期値はnil)

js: null と undefined (変数の初期値はundefined)

true と false
-------------

ruby: false, nil が偽、他は真

js: false, 0, NaN, "", null, undefined が偽、後は真。


文字列化
--------

ruby: 123.to_s 123.4.to_s

js: 123..toString() 123.4.toString()

ruby: puts, p, to_s, inspect を勉強した。
ruby: puts x → x.to_s

js: console.log(x); x.toString();

ruby: p obj → obj.inspect

self と this
------------

js: util = require('util');
　util.inspect(obj); // obj.inspect
　console.log(x); // puts x
　console.dir(obj); // p obj
　JSON.stringify(obj); // も inspect に近い?

 js: this
ruby: self
ここは考えどころだ。
同じかも。違うかも。


private メソッド
----------------

ruby: privateメソッドっていう名称に違和感がある。
なんか javaや c# の protected なイメージ。

self.puts "hello" ってできない。
puts が private メソッドだから。

Rubyのprivateメソッドは、レシーバを指定できない/させないメソッド。

self.puts とやってコケるputsと、
$stdout.puts "hello" で表示されるputsは別のputs。

Kernelモジュールでprivateで定義されているputsは、
フツーのプログラミング言語みたいに使えるようにするのに使われる。
レシーバを指定してputsしたければ、$stdio.puts と、IO#putsする。


基本オブジェクト BasicObject
----------------------------

Object を継承するインスタンスについて。

js:
o1 = {};
o2 = new Object();
o3 = Object.create(Object.prototype);
上記3つは Object を継承した普通のオブジェクト。

o4 = Object.create(null);
これは Object も継承しないオブジェクト。
なので o4.toString() もエラーで
o4 + "" でも文字列化できない。

rubyではそういうものが作成できるかどうかも不明。


Objectすら継承しないのであれば、BasicObjectですね。
BasicObject.new.to_s するとコケます。


なるほど。Object と BasicObject の関係は? 継承関係とは無いの?


ObjectのsuperclassがBasicObject


なるほど。Object が一番上と思っていたけど、BasicObjectがあったのか。
それより上はあるの?


BasicObjectが一番上です。
マニュアルには「必要ないなら使うな。Objectを継承しろ」と書いてあります。


なるほど。わかる様な気がします。システムプログラマ以外はBasicObjectから継承させる意味はほとんど無いと思う。

BasicObject.methods とか Object.methods を見てみたのですが多過ぎて理解不能です。


BasicObjectは1.9で入りました。それまではObjectが一番上で、メソッドがほとんどないObjectがほしい場合には、Objectをコピーしてメソッドを抜いて使ってたようです。BlankSlateというライブラリがありました。


== なんかも BasicObject に定義されているんですよね。
演算子が全部メソッドだから、そういう構造なんですね。

演算子も継承したクラスでは上書きできるんでしょうか。
+ とか - も定義できるんでしょうか。あまりにも基本的な演算子まで上書き(override)できると困る様な気がします。

1+2とか1+--2とか。


できますよ。継承したものどころかそのものを変更することすらできます。

p 100 + 1 # => 101

class Fixnum
　def +(other)
　　self - other
　end
end

p 100 + 1 # => 99


rubyコマンドでは実行できたけど irb ではエラーした。
irb 内で + を使っているから?


Windows上の2.0のirbでやったらコケるんですけど、
Linux上の2,1のirbでやったらちゃんと動きました。何が違うんだろう。


整数(Fixnum)の + を上書きしているんだから、全ての ruby で書かれたライブラリに影響すると思う。というか native メソッド以外、動く方が不思議。






 ruby 勉強中。

ruby の継承されているクラスのリストを見ただけでも、言語仕様がすごく膨れ上がっている様に感じた。
Fixnum < Integer < Numeric < Comparable < Object < Kernel < BasicObject とか多過ぎでしょう。
こりゃ性能をあげるのに苦労すると思う。

JavaScript では整数も浮動小数点も number だし、auto-boxing した Number クラスも Number < Object で終わり。
基本的な数値 number, 文字列 string, 真偽 boolean, あたりは JavaScript の型で十分に抽象化されていて丁度良いと思う。 C/C++/Java では int/double/long など様々にわかれているけどそこまで気にしないといけないのかどうか...

js では演算子も少ない言語仕様で規定されている。演算子はメソッドでは無い。

ここは宗教論争になる部分だろうな。

私は js も実際のところ number は int/long/double とわかれていて、後は string, boolean くらいが良かったと思うけどね...
JSONも発見されたけど、結構抽象化されていていいと思う。


メソッド呼び出しのコストが高いと言う話を聞いたのですが、クラスとメソッドの探索に時間がかかってるのかもしれませんね。





Ruby の勉強を始めて、最初に感じた違和感は Ruby の Object::ARGV の様に、Object とか Kernel の上位クラスになんでも詰め込み過ぎなんですね。
一番上位のクラスというか、そういう基本のクラスは、だんだんやれる事が減っていき一番基本のメモリとかCPUだけで処理できる事になるべきで、I/Oとか何か特殊な事ができるというのは、大きすぎると思います。頭でっかちに見えます。

私には最初の設計が間違ったのではないかと思われます。
デフォルトの考え方を間違ったのではないかと思います。
どう考えてもARGVは外界とのI/Fなので process とか、environment とかもっと他のクラスなりオブジェクトの持つ変数などであるべきでしょう。
もちろん puts とか p も console オブジェクトとかのメソッドでも良かったかな。

JavaScript ではデフォルトの this が指すのは外界から与えられる環境オブジェクトというか言語仕様ではグローバルオブジェクトになっています。node では global だし、browser では window オブジェクトです。node の global では process, module, require, console 等にアクセスできます。browser では document, location, history, navigator 等にアクセスできますよね。window.document は document です。

global.console.log('xxx'); // node
window.console.log('xxx'); // browser
グローバルオブジェクトは省略可能です。
global, root, this は node では同じ。
window, self, top 等は browser (top window) では同じ。

前にも言ったかもしれませんが、 JavaScript には I/O がが無く、ファイルもネットワークも無く、全く手足が無い状態です。文字列処理、配列、ハッシュマップ、正規表現、エラー処理(throw/catch)、そういう基本的なものしか定義されていません。C言語と一緒です。

HTML5では localStorage とか browser から与えられるオブジェクトのインターフェースを使って、初めて外界とアクセスでき、オブジェクトを保存できる様になったり、カメラやビデオにアクセスできる様になります。

この基本的な思想に違和感を感じました。
いかがでしょう。

ちなみに class というキーワードで ES6 から使用できるようになりますので、prototype ベースオブジェクト指向の表記が変になっている問題は徐々に気にならなくなってくると思っています。


Rubyがこのようになっているのは、ある部分ではPerlの代替になることを目指したからでしょうし、ある部分では言語のコアの部分の小さくして機能をクラス側へ追い出したからでしょう。Kernelにputsがなければ、$stdio.putsと書くことになったでしょう。それは、C++のようなプログラミング言語としてはごく普通のことなんだけど、シェルスクリプトやPerlの代替を目指すのであればやはり使いにくい。そういうことなんだと思います。

おそらく、Kernelが持っているputsの実体は、$stdio.putsだと思います。

あと、Kernel (これはクラスではなくてモジュールですが)が持っているメソッドのほとんどは、他のしかるべき場所に同じものあるものが、たくさんあります。openとかputsとかがそうです。これらのほとんどは、スクリプト言語として簡単にコードを書くために置かれているのだと思います。もしこれがなければ僕は、Railsようなアプリケーションは書いていたかもしれませんが、身の回りのちょっとした問題を片づける言語としては使ってないかもしれません。(Perlを使い続けていたかも)


私が言いたかったのは上位クラスにメソッドを集めるのではなく、
グローバルオブジェクトにショートカット的な変数をおけば良かったのではないかという事です。

javascript では var p = console.log.bind(console); としておけば、
p は関数なので、p('xxx'); の様に、いつでも書ける様になるわけで、
console オブジェクトの log メソッドへのショートカットとして、
外のオブジェクトに p を定義するという事は、可能だと思います。

スクリプトとして生まれた Ruby で、メソッドの引数の ( ) を省略できる事は、
非常に良い構文ですね。end による構文も { } を省略できますね。

ただ上位クラスに詰め込み過ぎたのは良くなかったと思う。
予約語が多すぎる感じです。
普通のクラスに p とか puts とか定義してもいいの? 使い分けは、どうなるのかな。

私が認識を間違っていたのは、Ruby をプログラミング言語として見ようとしていたことですね。
JavaScript もプログラミング言語として見てました。

シェルスクリプトの様なスクリプト言語として見れば、構文は Ruby の方が絶対いいですね。

私は C/Java系の文法が好きなでプログラミング言語として JavaScript を見ているから、いいのかも。Ruby はスクリプトなのでプログラミング言語の厳密さを求めてはいけないのかも。

私がプログラミング言語に求めている事は、性能と、その後に言語の文法や設計の美しさでしょうか。
それで Node.js は JavaScript の文法と仕様で高速なので好きなんですよね。
言語スペックが大きくなり過ぎる事もイヤかな。

引き続き Ruby も勉強しますが、Ruby との比較で、やはり JavaScript について。

ちなみに JavaScript の仕様つまり ECMA-262 は割と小さいと感じています。

現状の仕様 ECMAScript 5/5.1 版の前に、簡単な ECMAScript 3/3.1 版を勉強するといいと思います。その次の ES6 も勉強すべきとは思いますが、処理系が出回っているわけではないので徐々にで良いと考えています。

お勧めのリンク。
http://www2u.biglobe.ne.jp/~oz-07ams/prog/ecma262r3/


自分のクラスのメソッドの名前について、Kernelにあるメソッドの名前を気にする必要はまずないです。
自分が混乱しなければの話ですが。

Kernelにあるのはprivateメソッドなので、レシーバを指定した形式で呼ぶことはできないですから、同じ名前でも(自分が定義したクラスの外側で使う分には)あまり混乱しないです。クラスの中で使うと混乱します。


手足をもぎ取って仕様を小さくしたというのであれば、mrubyがJavaScriptに近いんじゃないですかね。外側でオブジェクトを用意してmrubyを呼び出してやるような使い方をするようですし。

なるほど。それで機械に組み込むんですよね。


