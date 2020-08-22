# Packages, variables and functions...

- 2020/08/22

## 1.Packages

プログラムの構成要素と考える。
インポートパスの最後の要素が、実際のパッケージの名前になる。
```
import (
	"fmt"
	"math/rand" // package rand
)
```

## 2.Imports

factoredインポートステートメント( factored import statement )という。
factoredは「要素化、グループ化、整理済み」という意味。
```
// factoredインポートステートメント
import (
	"fmt"
	"math"
)

// これでも良いけど。。
import "fmt"
import "math"
```

## 3.Exported names

大文字でないと、外部のパッケージから参照できない。
大文字の場合は、エクスポートされている状態であり、外部から参照できる(pkg.Method)
現状はpublic/privateの違いでしかないという認識。

```
// ex) unexportedな関数を実行して失敗している
./prog.go:9:14: cannot refer to unexported name math.pi
./prog.go:9:14: undefined: math.pi
```

## 4.Functions

```
func ${method_name} (${arg} ${型}, ....){}
```

```
func add(x int, y int) int {
	return x + y
}
```

goの関数の考え方

[Go's Declaration Syntax](https://blog.golang.org/declaration-syntax)

## 5.Functions continued

ただ単に省略できるよって話。
```
func add(x, y int) int {
	return x + y
}
```

## 6.Multiple results
関数には複数の戻り値を返すことができる。

```
func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```

## 7.Named return values

"naked return"という小技。
これを使うときは、短い関数で利用すべき。

```
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}
```

## 8.Variables

varで宣言。複数可能。

## 9.Variables with initializers

```
var i, j int
i, j = 1, 2
// ↓
// 単純にこう
var i, j int = 1, 2
```

## 10.Short variable declarations

暗黙的な型宣言の話。

```
// 同じ
var i = 3
k := 3
```

## 11.Basic types

Go言語の基本型(組み込み型)

```
bool

string

// 基本intだと考えて良い
int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 の別名

rune // int32 の別名
     // Unicode のコードポイントを表す

float32 float64

complex64 complex128
```

## 12.Zero values

変数に初期値を与えないと、ゼロ値になる。

```
数値型
(int,floatなど): 0

bool型: 
false

string型: 
"" (空文字列( empty string ))
```

## 13.Type conversions

型変換の話。Goでは明示的な変換が必要。

## 14.Type inference

型推論の話。明示的に型を指定しない場合( := や var = のいずれか)は型推論される。

```
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

## 15.Constants

定数。`const ${XXX}`で宣言する。`:=`は使えない。

## 16.Numeric Constants

> 数値の定数は、高精度な 値 ( values )です。
> 型のない定数は、その状況によって必要な型を取ることになります。

以下が参考になった。

[Go の定数の話](https://qiita.com/hkurokawa/items/a4d402d3182dff387674)

定数に型がない場合に、独自定義型の変数にも代入が可能。
