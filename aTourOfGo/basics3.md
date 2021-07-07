# More types: structs, slices, and maps

- 2020/09/01

## 1.Pointer

ポインタの話。ポインタ演算はできない。
型は*${valiable}型となる(ex. `t`は`*t`型)と言ったように。ゼロ値はnilとなる。

"dereferencing"、 "indirecting" の話がよくわからん。

`&`はポインタの引き渡しに使う。

```go
	i, j := 42, 10

	p := &i         // point to i
	fmt.Println(*p) // 42 iも42
	*p = 21         // set i through the pointer
	fmt.Println(i)  // 21 *pも21

	p = &j         // point to j
	*p = *p / 2    // divide j through the pointer
	fmt.Println(j) // 5 *pも5
```

この記事がすごくわかりやすかった。
[Goで学ぶポインタとアドレス](https://qiita.com/Sekky0905/items/447efa04a95e3fec217f)

## 2.Structs

おなじみ。

```go
type Vertex struct {
	X int
	Y int
}

func main() {
	fmt.Println(Vertex{1, 2})
}
```

## 3.Struct Fields

これもおなじみ。

```go
type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)
}
```

## 4.Pointers to structs

structフィールドはポインタを介して参照することが可能。

```go
type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v // structのポインタp
	p.X = 1e9
	fmt.Println(v) //{1000000000 2}
}
```

## 5.Struct Literals

`Name:`構文を利用すると、フィールドの一部に対して適用できる。順序は関係なし。

```go
type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)
```

## 6.Arrays

配列。`var a [10]int`のように定義する。
配列のサイズは変更不可。

```go
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
```

## 7.Slices

配列は固定長だが、スライスは可変長。
`a[low : high]`のように定義する。

```go
	primes := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = primes[1:4]
	fmt.Println(s) // [3 5 7]
```

## 8.Slices are like references to arrays

考え方としては、配列への参照をするためにスライスを用いる。スライス自体はデータを持っていない。スライスの要素を変更することで、配列の対応する要素が変更できる。

```go
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names) // [John Paul George Ringo]

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b) // [John Paul] [Paul George]

  b[0] = "XXX"
  // b[0]は元々Paulで、参照先も変更されている
	fmt.Println(a, b) // [John XXX] [XXX George]
	fmt.Println(names) // [John XXX George Ringo]
```

## 9.Slice literals

通常の配列リテラルと、スライスによる配列の生成。

```go
[3]bool{true, true, false}

[]bool{true, true, false}
```

## 10.Slice defaults

スライスは上限・下限を省略することが可能。

```go

var a [10]int

// 上限・下限
a[0:10] 

// 省略できる
a[:10]
a[0:]
a[:]
```

## 11.Slice length and capacity

スライスは、長さ(length)と容量(capacity)を持っている。
スライスの長さは、それに含まれる要素の数。容量はスライスの最初の要素から計算した配列要素数。

```go
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s)

	// Extend its length.
	s = s[:4]
	printSlice(s)

	// Drop its first two values.
	s = s[2:]
  printSlice(s)

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

## 12.Nil slices

スライスのゼロ値はnil。nilスライスは0の長さと容量を持っている。

## 13.Creating a slice with make

`make`による動的サイズの配列作成。

```go
a := make([]int, 5)  // len(a)=5

b := make([]int, 0, 5) // len(b)=0, cap(b)=5
```

## 14.Slices of slices

連想配列。

```go
// Create a tic-tac-toe board.
board := [][]string{
  []string{"_", "_", "_"},
  []string{"_", "_", "_"},
  []string{"_", "_", "_"},
}

// The players take turns.
board[0][0] = "X"
board[2][2] = "O"
board[1][2] = "X"
board[1][0] = "O"
board[0][2] = "X"
```

## 15.Appending to a slice

> append の戻り値は、追加元のスライスと追加する変数群を合わせたスライスとなります。

```go
func append(s []T, vs ...T) []T

func main() {
	var s []int
	printSlice(s) // len=0 cap=0 []

	// append works on nil slices.
	s = append(s, 0)
	printSlice(s) // len=1 cap=1 [0]

	// The slice grows as needed.
	s = append(s, 1)
	printSlice(s) // len=2 cap=2 [0 1]

	// We can add more than one element at a time.
	s = append(s, 2, 3, 4)
	printSlice(s) // len=5 cap=6 [0 1 2 3 4]
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

## 16.Range

`for`のループで利用する`range`はスライスやマップ(map)を反復処理できる。

```go
  var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
```

## 17.Range continued

`_`で必要ない値は捨てる。

```go
for i, _ := range pow
for _, value := range pow
```

## 19.Maps

キーと値を関連ずける。

```go
type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex // 配列と、上の構造体を紐付けている

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}
```

## 20.Map literals

> mapリテラルは、structリテラルに似ていますが、 キー ( key )が必要です。

## 21.Map literals continued

> もし、mapに渡すトップレベルの型が単純な型名である場合は、リテラルの要素から推定できますので、その型名を省略することができます。


```go
var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967}, // "Bell Labs": Vertex{ 40.68433,-74.39967 }
	"Google":    {37.42202, -122.08408},
}

```

## 22.Mutating Maps

- m へ要素(elem)の挿入や更新
  - `m[key] = elem`
- 要素の取得:
  - `elem = m[key]`
- 要素の削除
  - `delete(m, key)`
- キーに対する要素が存在するかどうかは、2つの目の値で確認
  - `elem, ok = m[key]` or `elem, ok := m[key]`
  - もし、 m に key があれば、変数 ok は true となり、存在しなければ、 ok は false となります。
  - 存在しない場合は、mapの要素型のゼロ値となる

## 24.Function values

> 関数値( function value )は、関数の引数に取ることもできますし、戻り値としても利用できます。

```go
func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot))
	fmt.Println(compute(math.Pow))
}
```

## 25.Function closures

Goの関数はクロージャ。