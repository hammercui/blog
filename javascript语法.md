>JavaScript常用易忘记语法小结



## Array拼接

1. 不去重的拼接，使用`concat`

```
a:Array =
arrayObject.concat(arrayX,arrayX,......,arrayX)
```

2. 去重的操作，[lodash]()大法好

`_.union([arrays])`：合并并且去重

```
_.union([1, 2], [4, 2], [2, 1]);
// => [1, 2, 4]
```

## 判断obj类型

```
let a = "aa";

typeof a // "string"
```