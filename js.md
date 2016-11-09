js
====


型、値、オブジェクト、プロパティ、関数、プログラム構文

オブジェクト

プロパティ

プロパティの属性  
　ReadOnlyなど DontEnum DontDelete Internal

プロパティは、他のオブジェクトや プリミティブ値 、メソッド を保持するコンテナである。

プリミティブ値  
Undefined, Null, Boolean, Number, String

オブジェクト  
Objectなど

メソッド  
関数

組み込みオブジェクト (built-in object) の集合を定義し、これは ECMAScript 実体の定義を完成する。これらの組み込みオブジェクトは、Global オブジェクト、 Object オブジェクト、 Function オブジェクト、 Array オブジェクト、 String オブジェクト Boolean オブジェクト、 Number オブジェクト、 Math オブジェクト、 Date オブジェクト、 RegExp オブジェクト、そして Error オブジェクト、Error, EvalError, RangeError, ReferenceError, SyntaxError, TypeError, URIError を含む。

組み込みの演算子のセットも定義し、厳密には、それらは関数やメソッドではないかもしれない。ECMAScript 演算子は、さまざまな単項演算(unary operations), 乗除演算子(multiplicative operators), 加減演算子(additive operators), ビットシフト演算子(bitwise shift operators), 関係演算子(relational operators), 等価演算子(equality operators), ビット演算子(binary bitwise operators), 論理演算子(binary logical operators), 代入演算子(assignment operators), カンマ演算子(comma operator) を含む。

プリミティブ値 (primitive value)  
プリミティブ値 (primitive value) は Undefined, Null, Boolean, Number, String 
undefined  
null  
true, false  
'string', "string"
0, -0, NaN, +-Infinity


オブジェクト (Object)  
オブジェクトは Object 型の構成要素である。序列のないプロパティの集合体で、それぞれのプロパティがプリミティブ値やオブジェクト、関数を含む。オブジェクトのプロパティに格納された関数はメソッドと呼ばれる。

コンストラクタ (Constructor)  
コンストラクタとは、オブジェクトを生成し初期化する Function オブジェクトのことである。各コンストラクタは関連する prototype オブジェクトを持ち、継承と共有プロパティの実装に用いられる。

プロトタイプ (Prototype)  
プロトタイプはオブジェクトであり、ECMAScript 中では、構造(structure), 状態(state), 振る舞い(behaviour) の継承に用いられる。コンストラクタがオブジェクトを生成するとき、プロパティの参照を解決する目的で、そのオブジェクトは暗黙にコンストラクタに関連するプロトタイプを参照する。コンストラクタに関連するプロトタイプはプログラム式 constructor.prototype で参照され、オブジェクトのプロトタイプに追加されたプロパティは、継承を通して、そのプロトタイプを共有する全オブジェクトによって共有される。

return の後ろに改行があると終了してしまう

コメント  
//, /* */

ES3予約語  
break
case
catch
continue
default
delete
do
else
finally
for
function
if
in
instanceof
new
return
switch
this
throw
try
typeof
var
void
while
with

将来の予約語
abstract
boolean
byte
char
class ES6
const ES6
debugger
double
enum
export ES6
extends ES6
final
float
goto
implements
import ES6
int
interface
long
native
package
private
protected
public
short
static ES6
super ES6
synchronized
throws
transient
volatile

実行コンテキスト

スコープ連鎖と識別子の解決 (Scope Chain and Identifier Resolution)

Global オブジェクト (Global Object)

Activation オブジェクト (Activation Object)

this

arguments

arguments.callee　ES5 strict modeで使用不可  
arguments.length

withは使用しない


グローバルコード  
Evalコード  
関数コード

returnや例外(throw)により抜ける時に実行コンテキストはなくなる  
(ES6 yieldでは実行コンテキストは維持?)

this, 識別子, リテラル, [](Array), {}(Object)

ref++, ref--

delete ref  
void ex  
typeof ex  
++ref, --ref  
+ex, -ex  
~ex, !ex

*, /, %  
+, -  
<<, >>, >>>  
<, >, <=, >=, instanceof, in (forでは使えない)  
==, !=, ===, !==  
&, ^, |  
&&, ||  
? :  
= *= /= %= += -= <<= >>= >>>= &= ^= |=  
,

{}, var, ex;  
if, else,  
do, while, for,  
with [ES5 strict modeで無効]  
swicth () {case : default: break; }  
try catch finally throw

function



