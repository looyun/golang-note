# 函数

## 1.闭包

* 个人理解：使一个变量超出自己本该有的生命期。\(alert,return, **defer?**\)~~外部函数通过return内部函数，得到内部函数中的变量。~~

* 使用闭包会造成变量在内存中长久使用，不利于性能，要恰当地释放内存。\* 调用外部函数值取地址


## 2.defer

* 函数return时，defer语句反序执行_ 函数严重错误仍执行_ 支持匿名函数调用\* GO没有异常处理机制， 但有panic\/recover模式处理错误 panic可在任意地方引发，recover只在defer调用的函数中有效

## 3.通过代码理解

```go
func main() {
var fs = [4]func(){}
for i := 0; i < 4; i++ { defer fmt.Println("defer i = ", i) defer func() { fmt.Println("defer_closure i = ", i) }() fs[i] = func() { fmt.Println("closure i = ", i) } } for _, f := range fs { f() }}
```

