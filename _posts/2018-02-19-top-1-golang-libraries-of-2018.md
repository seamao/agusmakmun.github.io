---
layout: post
title:  "1月17日总结_map字典"
date:   2018-02-19
categories: [golang, map]
---
## map字典

### Demo
```
package main

import "fmt"

func main() {

	Card := map[string]string{
		"company": "孔壹学院",
		"place":   "北京",
		"name":    "海博",
		"tel":     "18368852171",
	}
	fmt.Println(Card)
	fmt.Println(Card["name"])
	delete(Card, "place")
	fmt.Println(Card)

	for key, val := range Card {
		fmt.Println(key, " - ", val)
	}
}

```
输出为:
```
map[name:海博 tel:18368852171 company:孔壹学院 place:北京]
海博
map[company:孔壹学院 name:海博 tel:18368852171]
company  -  孔壹学院
name  -  海博
tel  -  18368852171


```

- 声明一个名为`Card`的map字典，字典的钥匙为`string`类型，字典的里面的值为`string`类型。

  `Card := map[string]string{}`


- 索引字典的为`name`所对应的值。

  `Card[name]`

- 删除字典的钥匙为２所对应的值。

  `delete(Card, "place")`

- 遍历map字典
```
for key, val := range Card {
  fmt.Println(key, " - ", val)
}
```

> 每天只要一点点,感谢观看！！！

来一波公众号
![](http://p3qaz7oyz.bkt.clouddn.com/0.jpeg)
