Go 语言学习笔记
－－－－－－－－－－－－－－－－－－－
**2013-03-30**


# Go环境的安装
1. 下载完成GO的代码包后unpack至/home/rogee/SDK/go目录下
2. Terminal进入到GO目录下的src目录下， 运行`./all.bash`
3. 等待执行过程直到看到“*** You need to add /home/rogee/SDK/go/bin to your PATH.”这句话表示安装完成， 需要你手动设置你的PATH环境来使GO可以在命令行下使用
4. 设置完PATH后测试下`go version`查看是不是跟你下载的GO的版本是一致的


# 第一个GO程序｀hello.go｀

1. 写入下面的代码
```go
package main
import "fmt"

func main(){
    fmt.Println("Hello world, 你好世界");
}
```
2. 运行程序 `go run hello.go`

# 变量的声明
```go
var vi int
var v2 string
var v3 [10]int //数组
var v4 []int //数组切片
var v5 struct {
    f int
}
var v6 *int //指针
var v7 map[string]int
var v8 func(a int) int
```

# 变量的初始化
* 三种变量赋值方式
```go
var v1 int = 10
var v2 = 10 //编辑器可以自动推导出来变量的类型
v3 := 10 //注意与上面的“冒号”赋值的差别
```
* GO语言中同时也提供了一种的多重赋值功能,不支持这种功能的语言需要引入一个第三方的tmp变量来实现
```go
    i, j = j, i
```
#  匿名变量
* Go语言支持多个返回值， 只想获取其中一个返回值的话可以通过设置`匿名变量`来进行操作
```go
func GetName() (firstName, secondName, thirdName){
    return "yang", "hao", "ren"
}

_, _, nickName := GetName()
```

# 常量
* 通过`const`关键字来定义一个常量
```go
const PI float64 = 3.1415926
const zero = 0.0
const {
    size int 64 = 1023
    eof = -1
}
const u, v float32 = 0, 3
const a,b,c = 3,4,"foo"
```
* 预定义常量 `true`, `false`, `iota`, iota可变， 在每一次出现时，它都会进行自增操作（这种自增只可在const代码块内实现，单个const声明不会生效）; 如果两个赋值语句是一样的， 可以只写一个而省略后一个赋值语句，只写上变量即可。
```go
const {
    c0 = iota // ------> 0
    c1 = iota // ------> 1
    c2 = iota // ------> 2
}
```

# 枚举类型
* 枚举类型关键字同样是`const`， 但是会把const代码块的**{**替换为**(**； 还一个要注意的是，小写字母开头的不会被导出变量， 因为他在枚举定义中是私有性质存在的。
```go
const (
    Sunday = iota
    Monday
    Tuesday
    Wednesday
    Thursday
    Friday
    Saturday
    totalWeekDayNum // 注意这个变量不会被导出，小写字母开头的为私有
)
```

# 布尔类型
* Boolean类型只有两个关键字`true`和`false`， 在变量赋值中不可以为1,0等 形式存在， 但是可以 (1==1)的比较返回值存在。
```go
var v bool
v = true
v1 := (1==1)
```

# 整型
* 一个注意的是**不同类型的整型不可以直接赋值**
```go
var v int32 = 1000
var v2 int

v2 = v //这里会报编译错误
v2 = int(v) //编译通过
```

# 数组
* 数组的定义
```go
[32]byte
[2*N] struct (x, y int32) //升序为32的数组， 每个元素为一个字节
[1000]*float64 //复杂类型的数组
[3][5]int //二维数组----->以此类推多维数组
```
* 数组元素的访问
```go
var array =  [5]int{5,4,3,2,1}
for i:0; i< len(array); i++{
    fmt.Println("Element", i, "of array is", array[i]);
}
```

* GO语言中还提供一个关键字`range`用于遍历容器中的元素， 上面的遍历可以简写为
```go
var array =  [5]int{5,4,3,2,1}
for i,v := range array{
    fmt.Println("Array element[", i, "]", v);
}
```
Result:
```
rogee@Rogee:~/go$ go run hello.go
Array element[ 0 ] 5
Array element[ 1 ] 4
Array element[ 2 ] 3
Array element[ 3 ] 2
Array element[ 4 ] 1
```