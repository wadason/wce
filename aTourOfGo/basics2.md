# Flow control statemeents : for, if, else, switch and defer

- 2020/08/25

## 1.For

```go
func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```
## 2.For continued

```go
func main() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}
```

## 3.For is Go's "while"

whileはforでかける。

```go
func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}
```

## 4.Forever

無限ループ。

```go
	for {
	}
```

## 5.If

基本的にgoの文法は () は不要、 {} は必要。

```go
if x < 0 {
  return sqrt(-x) + "i"
}
```

## 6.If with a short statement

評価する前のステートメントを書くことができる。
ifのスコープ内でのみ利用可能。

```go
if v := math.Pow(x, n); v < lim {
  return v
}
return lim
```

## 7.If and else

ifで宣言した変数は、else内でも利用可能。

```go
if v := math.Pow(x, n); v < lim {
  return v
} else {
  fmt.Printf("%g >= %g\n", v, lim) // v使える
}
// can't use v here, though
return lim
```

## 8.Exercise: Loops and Functions

## 9.Switch

> Go では選択された case だけを実行してそれに続く全ての case は実行されません。
breakかく必要なないとのこと。
switchケースは定数である必要がないとのこと。

```go
switch os := runtime.GOOS; os {
case "darwin":
  fmt.Println("OS X.")
case "linux":
  fmt.Println("Linux.")
default:
  // freebsd, openbsd,
  // plan9, windows...
  fmt.Printf("%s.\n", os)
}
```

## Switch evaluation order

goのswitch caseは上から下へ評価されるため、case条件が一致した時点でbreakされる。

## Switch with no condition

"if-then-else" 

```go
switch {
  case t.Hour() < 12:
    fmt.Println("Good morning!")
  case t.Hour() < 17:
    fmt.Println("Good afternoon.")
  default:
    fmt.Println("Good evening.")
}
```

## Defer

returnするまで処理をしない。
評価自体は行われるが、実行自体はreturn直後になる。

```go
func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

## Stacking defers

`defer`を複数定義した時に、その実行の順序はLIFOとなる。

[Defer, Panic, and Recover](https://blog.golang.org/defer-panic-and-recover)
