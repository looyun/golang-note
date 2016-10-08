#array/slice/map 定义
## 1.array
```
	var a []int
	a := new([10]int)        //指针
	a := []int{}
	a = []int{1, 3, 3}
```

## 2.slice
（可变长,不是数组，指向数组）     函数：len()  cap() append()
**copy()：可以包含数组和切片**
```
	var s []int
	s := make([]int, len, cap)
	s := []int{}
	s = []int{1, 2, 3}
	s[a] = b           //a为索引值，固有
	s = a[2:5]

```

## 3.map
函数：delete() len()...
```
	var m map[int]string
	m := map[int]string{}
	m := make(map[int]string, cap)
	m = map[int]string{1: "a", 2: "b", 3: "c"}
	m[a] = b            //a为键值，需输入

```
## 4.PS
```
	for i := range a {
	}
	for i, v := range s {
	}
	for k, v := range m {
	}
```
i,k,v可省略为_