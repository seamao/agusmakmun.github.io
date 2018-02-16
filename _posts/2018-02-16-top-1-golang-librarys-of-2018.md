# 1月15日总结

### 问题：求１０００以内的质数

解密质数特性：质数定义为在大于1的自然数中，除了1和它本身以外不再有其他因数。
```
package main

import "fmt"

func main() {

for i := 2; i < 999; i++ {
isZhiShu := true
//筛选　２到９９９
for k := 2; k < i; k++ {
if i%k == 0 {
isZhiShu = false
break
}

}

if isZhiShu {
fmt.Println(i)
}
}

}
```

### `swich` 语句　：
`swich`如同声明一把锁，而开锁的需要`case`命令与`swich`内容相符，当`case`与`swich`命令相符时，执行该`case`的命令，并无法执行其余`case`内容
而当`case`命令下使用`fallthought` 时，可以继续执行紧挨着的下一个`case`的内容！！！

### 输入输出对话框
```
package main

import "fmt"

func main() {
	fmt.Print("输入一个数字：")
	score := 0
	fmt.Scanf("%d", &score)
	fmt.Println(score)
}
```
命令执行后可在终端输入相关数字回复

### 数组
--　如何判断一个数组的类型
```
// 如何去判断一个变量的类型

// var num int
// var v type
```
-- 　遍历、快速枚举
```
for i, v := range nums {
  fmt.Println(i) //索引
  fmt.Println(v) //值
```
备注:　数组中的索引　０　表示的是数组中的第一个数的值

--　切片
｀mySlice := make([]int, 3, 3)｀

其意为声明一个长度为３、容量为３的切片数组

-- append
`append`　可以为固定长度的数组添加值
例如：
```
package main

import "fmt"

func main() {

	mySlice := []string{"Monday", "Tuesday"}
	myOtherSlice := []string{"Wednesday", "Thursday", "Friday"}

	mySlice = append(mySlice, myOtherSlice...)

	fmt.Println(mySlice)
}
```
append将两个数组的值赋值给了mySlice 数组


感谢观看 ! ! !

