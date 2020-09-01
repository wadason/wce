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