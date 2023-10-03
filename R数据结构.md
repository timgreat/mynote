# R数据结构

原链接：https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/index.html

R语言数据结构包括向量，矩阵和数据框，多维数组， 列表，对象等。数据中元素、行、列还可以用名字访问。 最基本的是向量类型。 向量类型数据的访问方式也是其他数据类型访问方式的基础。

### 1.1、数值向量

**向量**是将若干个基础类型相同的值存储在一起， 各个元素可以按序号访问。 如果将若干个数值存储在一起可以用序号访问， 就叫做一个数值型向量。

例：

```R
x<-c(1,2,3)
#get the length of vector x
length(x)
## [1] 3
#generate vector = 0 * 10 
numeric(5)
## [1] 0 0 0 0 0
```

单个数值称为**标量**， R没有单独的标量类型， 标量实际是长度为1的向量。`%/%`表示整除，`%%`表示取余数。

### 1.2、运算

​	向量与标量的运算为每个元素与标量的运算, 一个向量乘以一个标量， 就是线性代数中的数乘运算。

四则运算时如果有缺失值，缺失元素参加的运算相应结果元素仍缺失。

```R
x<-c(1,10)
x+2
##[1] 3 12
y<-c(1,NA,3)
y+10
##[1] 11 NA 13
```

​	等长向量的运算为对应元素两两运算。两个等长向量的加、减运算就是线性代数中两个向量的加、减运算。

```R
x1<-c(1,2,3)
x2<-c(4,5,6)
x1+x2
##[1] 5,7,9
```

​	两个不等长向量的四则运算， 如果其长度为**倍数关系**，规则是每次从头重复利用短的一个。不仅是四则运算，R中有两个或多个向量按照元素一一对应参与某种运算或函数调用时， 如果向量长度不同，一般都采用这样的规则。如果两个向量的长度不是倍数关系，会给出警告信息。

```R
x1<-c(10,20)
x2<-c(1,2,3,4)
x1+x2
##[1] 11 22 13 24
c(1,2)+c(1,2,3)
## Warning in c(1, 2) + c(1, 2, 3): longer object length is not a multiple of
## shorter object length
## [1] 2 4 4
```



### 1.3 向量函数

​	R的函数一般都是向量化的，如果普通的一元函数一向量为自变量，则会对每个元素进行计算。

```R
sqrt(c(1, 4, 6.25))
## [1] 1.0 2.0 2.5
```

如果自己编写的函数没有考虑向量化问题， 可以用`Vectorize()`函数将其转换成向量化版本。

#### 排序函数

```R
x<-c(1,5,3)
#return the sorted result of vector
sort(x)
## [1] 1 3 5
# get the reversed result of vector
rev(x)
##[1] 3 5 1
#get the scripts used to sorted
order(x)
## [1] 1 3 2
x[order(x)]
## [1] 1 2 3
```

#### 统计函数

`sum`(求和), `mean`(求平均值), `var`(求样本方差，用n-1做分母), `sd`(求样本标准差), `min`(求最小值), `max`(求最大值), `range`(求最小值和最大值)等函数称为统计函数， 把输入向量看作样本，计算样本统计量。 `prod`求所有元素的乘积。

```R
cumsum(1:5) #the accumulated sum from left to right
## [1]  1  3  6 10 15
cumprod(1:5)
## [1]   1   2   6  24 120
x<-c(1,2,3)
y<-c(4,5,6)
pmin(x,y) #the min of corresponding position
## [1] 1 2 3
pmax(x,y)
## [1] 4 5 6
cumin(x) #the accumulated min from left to right 
## [1] 1 1 1
cummax(x) 
##[1] 1 2 3 4
```

#### 生成规则序列的函数

seq函数是冒号运算符的推广。 比如，`seq(5)`等同于`1:5`。 `seq(2,5)`等同于`2:5`。 `seq(from=11, to=15, by=2)`产生11,13,15。 `seq(0, 2*pi, length.out=100)`产生从0到的等间隔序列， 序列长度指定为100。

`rep()`函数用来产生重复数值。 为了产生一个初值为零的长度为n的向量， 用`x <- rep(0, n)`。 `rep(c(1,3), 2)`把第一个自变量重复两次， 结果相当于`c(1,3,1,3)`。

`rep(c(1,3), c(2,4))` 把第一自变量的第一个元素按照第二自变量中第一个元素的次数重复， 把第一自变量中第二个元素按照第二自变量中第二个元素的次数重复， 结果相当于`c(1,1,3,3,3,3)`。

如果希望重复完一个元素后再重复另一元素，用`each=`选项， 比如`rep(c(1,3), each=2)`结果相当于`c(1,1,3,3)`。**`1:5`和`seq(5)`的结果是整型（integer）的， `c(1,3,5)`和`seq(1, 5, by=2)`的结果是浮点型（double）的。**

#### 复数向量

用函数`complex()`生成复数向量， 指定实部和虚部。 如`complex(real = c(1,0,-1,0), imaginary = c(0,1,0,-1))`相当于`c(1+0i, 1i, -1+0i, -1i)`。

在`complex()`中可以用`mod`和`arg`指定模和辐角，如 `complex(mod=1, arg=(0:3)/2*pi)`结果同上。

用`Re(z)`求`z`的实部， 用`Im(z)`求z的虚部， 用`Mod(z)`或`abs(z)`求z的模， 用`Arg(z)`求z的辐角， 用`Conj(z)`求z的共轭。

`sqrt`, `log`, `exp`, `sin`等函数对复数也有定义，需要指定数据为复数。

```R
sqrt(-1)
## [1] NaN
## Warning message:
## In sqrt(-1) : NaNs produced
sqrt(-1 + 0i)
## [1] 0+1i
```

### 2.1逻辑向量

逻辑型是R的基本数据类型之一，只有两个值TRUE和FALSE, 缺失时为NA。逻辑值一般产生自比较。向量与标量，向量与向量之间的比较都遵从向量间运算的一般规则。

```R
c(1, 3, 5) > 2
## [1] FALSE  TRUE  TRUE
(1:4) >= (4:1)
## [1] FALSE FALSE  TRUE  TRUE
c(1, NA, 3) > 2
## [1] FALSE    NA  TRUE
NA == NA
## [1] NA
x<-c(1,2,3,4)
x[c(TRUE,FALSE,FALSE,TRUE)]
## [1] 1 4
```

为了判断向量每个元素是否NA， 用`is.na()`函数：

```R
is.na(c(1, NA, 3) > 2)
## [1] FALSE  TRUE FALSE
```

用`is.finite()`判断向量每个元素是否是有限值，`is.infinite()`判断是否是无限值：

```R
x<-c(1,2,Inf)
is.finite(x)
## [1]  TRUE  TRUE FALSE
is.infinite(x)
## [1] FALSE FALSE  TRUE
```

`%in%`判断前者元素是否属于后者：

```R
c(1,3) %in% c(2,3,4)
## [1] FALSE  TRUE
```

`match(x,y)`函数与`%in%`类似，但是返回前者在后者中的位置：

```R
match(c(1, 3), c(2,3,4,3))
## [1] NA  2
```

### 2.2逻辑运算

用`xor(x, y)`表示`x`与`y`的异或运算， 即值不相等时为真值，相等时为假值， 有缺失值参加运算时为缺失值。逻辑向量与逻辑标量之间的逻辑运算， 两个逻辑向量之间的逻辑运算规则遵从一般R向量间运算规则。

在右运算符是缺失值时， 如果左运算符能够确定结果真假， 可以得到非缺失的结果。 例如，`TRUE | NA`为`TRUE`, `FALSE & NA`为`FALSE`。 不能确定结果时返回`NA`， 比如， `TRUE & NA`为`NA`, `FALSE | NA`为`NA`。

`&&`和`||`分别为短路的标量逻辑与和短路的标量逻辑或， 仅对两个标量进行运算，如果有向量也仅使用第一个元素。

```R
TRUE || sqrt(-1)>0
## [1] TRUE
```

### 2.3逻辑运算函数

因为R中比较与逻辑运算都支持向量之间、向量与标量之间的运算， 所以在需要一个标量结果时要特别注意， 后面讲到的`if`结构、`while`结构都需要逻辑标量而且不能是缺失值。 这时，应该对缺失值结果单独考虑。

```R
#determine all elements of x is TRUE
x<-(c(1,NA,3)>2)
all(x)
## [1] FALSE

#determine exist elements of x is TRUE
any(x)
## [1] TRUE

#return the scripts of the elements is TRUE in x
which(c(FALSE, TRUE, TRUE, FALSE, NA))
## [1] 2 3

#determine  whether x,y are same
identical(c(1,2,3), c(1,2,NA))
## [1] FALSE

#return whether each elements is duplicated
duplicated(c(1,2,1,3,NA,4,NA))
## [1] FALSE FALSE  TRUE FALSE FALSE FALSE  TRUE
unique(c(1,2,1,3,NA,4,NA))
## [1]  1  2  3 NA  4
```

### 3.1字符向量

字符型向量是元素为字符串的向量。 字符串在程序中写成用两个双撇号包围或者用两个单撇号包围的内容`c('abc', '', 'a cat', NA, '李明')`。**空字符串并不能自动认为是缺失值， 字符型的缺失值仍用`NA`表示。**

### 3.2字符函数

#### 转义字符

使用`\`转义，当需要转义的内容较多时，可以采用原始字符串`r"(...)"`。`cat()`函数类似于`print()`输出字符串内容，但前者无返回值。

```R
cat(r"(\"1\\2\")")
## \"1\\2\"
```

#### paste函数

针对字符型数据最常用的R函数是`paste()`函数。 `paste()`用来连接两个字符型向量， 元素一一对应连接， 默认用空格连接。

`paste()`在连接两个字符型向量时采用R的一般向量间运算规则， 而且可以自动把数值型向量转换为字符型向量。 可以作一对多连接。

```R
paste("x",1:4,sep="")
## [1] "x1" "x2" "x3" "x4"
paste(c("x","y","z"),collapse=" ")
## [1] "x y z"
```

#### 大小写转换

`toupper()`函数把字符型向量内容转为大写， `tolower()`函数转为小写。

```R
x<-"aBcD"
toupper(x)
## [1] "ABCD"
tolower(x)
## [1] "abcd"
```

#### 字符串长度

用`nchar(x, type='bytes')`计算字符型向量`x`中每个字符串的以字节为单位的长度，用`nchar(x, type='chars')`计算字符型向量`x`中每个字符串的以字符个数为单位的长度。

在画图时可以用`strwidth()`函数计算某个字符串或表达式占用的空间大小。

```R
x<-"莫"
nchar(x,type = "chars")
##[1] 1
nchar(x,type = "bytes") #chinese character coded in utf-8 need 3 bytes,GBK 2 bytes
##[1] 3
```

#### 取子串

`substr(x, start, stop)`从字符串x中取出从第start个到第stop个的子串。

```R
substr('JAN07', 1, 3)
## [1] "JAN"
```

如果x是一个字符型向量，`substr`将对每个元素取子串。如

```R
substr(c('JAN07', 'MAR66'), 1, 3)
## [1] "JAN" "MAR"
```

用`substring(x, start)`可以从字符串x中取出从第start个到末尾的子串。如

```R
substring(c('JAN07', 'MAR66'), 4)
## [1] "07" "66"
```

#### 类型转换

用`as.numeric()`把内容是数字的字符型值转换为数值，并且是可向量化的。

```R
as.numeric(substr(c('JAN07', 'MAR66'), 4, 5))
## [1]  7 66
```

用`as.character()`函数把数值型转换为字符型，如果自变量本来已经是字符型则结果不变。

```R
as.character((1:5)*5)
## [1] "5"  "10" "15" "20" "25"
```

为了用指定的格式数值型转换成字符型， 可以使用`sprintf()`函数， 其用法与C语言的`sprintf()`函数相似， 只不过是向量化的。

```R
sprintf("file%03d.txt",c(1,2,3))
## [1] "file001.txt" "file002.txt" "file003.txt"
```

readr包的`parse_number()`输入一个字符串向量， 对每个字符串， 找到第一个能识别为数值的内容， 舍弃其它内容， 返回转换浮点型结果。 没有数值时返回缺失值， 并增加一个表格用来记录所有的不成功转换。

```R
readr::parse_number(c(
  "123", "output-123.txt", "a123.456bc04", 
  "30.2%", "abc"
))
##Warning: 1 parsing failure.
##row col expected actual
##  5  -- a number    abc
##[1]  123.000 -123.000  123.456   30.200       NA
##attr(,"problems")
### A tibble: 1 × 4
##    row   col expected actual
##  <int> <int> <chr>    <chr> 
##1     5    NA a number abc  
```

#### 字符串替换功能

用`gsub()`可以替换字符串中的子串， 这样的功能经常用在数据清理中。 比如，把数据中的中文标点改为英文标点， 去掉空格，等等。

```R
x<-'1,3;5'
gsub(';', ',', x, fixed=TRUE)
## [1] "1, 3, 5"
```

#### 正则表达式

R中支持perl语言格式的正则表达式， `grep()`和`grepl()`函数从字符串中查询某个模式， `sub()`和`gsub()`替换某模式。

```R
#change more than one spaces to one space
gsub('[[:space:]]+', ' ','a   cat  in a box', perl=TRUE)
## [1] "a cat in a box"
```

### 4.1R向量下标和子集

对于向量可以使用正整数下标获取元素，给元素赋值，也可以使用负整数下标删除元素获得向量的子集，这些下标都支持向量化。**负整数下标不能与正整数下标同时用来从某一向量中取子集。**

`x[0]`是一种少见的做法， 结果返回类型相同、长度为零的向量， 如`numeric(0)`。 相当于空集。

```R
x<-c(1,2,3)
#x[] get all the elements of x
x[]<-999
x
## [1] 999 999 999
x<-999
x
## [1] 999
x<-c(1,2,3)
#x[0] same as numeric(0)
```

#### 下标超界

如何使用下标超出向量的长度，返回`NA`。如果给超出向量长度的下标赋值，则向量变长，中间无赋值元素变为`NA`。

#### 逻辑下标

下标可以是与向量等长的逻辑表达式， 一般是关于本向量或者与本向量等长的其它向量的比较结果.

```R
x<-c(1,4,5)
#get the elements > 3
x[x>3]
## [1] 4 5
x[c(TRUE,FALSE,TRUE)]
## [1] 1 5
```

逻辑下标除了用来对向量取子集， 还经常用来对数据框取取子集， 也用在向量化的运算中。**要注意的是，如果逻辑下标中有缺失值， 对应结果也是缺失值。 所以，在用逻辑下标作子集选择时， 要加上`!is.na`前提。**

```R
x <- c(1, 4, 6.25, NA)
x[x > 2]
## [1] 4.00 6.25   NA
x[!is.na(x) & x > 2]
## [1] 4.00 6.25
```

函数`which()`可以用来找到满足条件的下标，`seq(along=x)`会生成由`x`的下标组成的向量。用`which.min()`、`which.max`求最小值的下标和最大值的下标， 不唯一时只取第一个。

```R
x<-c(3,4,3,5,7,5,9)
which(x > 5)
## [1] 5 7
seq(along=x)[x>5]
## [1] 5 7
which.min(x)
## [1] 1
which.max(x)
## [1] 7
```

#### 元素名

向量可以为每个元素命名，使用元素名获取向量值，本质上是一个值域到另一个值域间的映射。

```R
ages<-c("a"=1,"b"=2,"c"=3)
#or
ages<-c(1,2,3)
names(ages)=c("a","b","c")
#or
ages<-setNames(c(1,2,3),c("a","b","c"))
```

用`unname(x)`返回去掉了元素名的`x`的副本， 用`names(x) <- NULL`可以去掉`x`的元素名。

#### 集合运算

可以把向量x看成一个集合，但是其中的元素允许有重复。 用`unique(x)`可以获得x的所有不同值。

```R
unique(c(1,5,2,5))
## [1] 1 5 2
```

用`a %in% x`判断`a`的每个元素是否属于向量x，函数`match(x,y)`返回`x`中的每个元素在`y`中的位置

```R
5 %in% c(1,5,2)
## [1] TRUE
match(5,c(1,5,2))
## [1] 2
match(c(2,5,0), c(1,5,2,5))
## [1]  3  2 NA
```

用`intersect(x,y)`求交集，结果中不含重复元素.

```R
```

用intersect(x,y)求交集，结果中不含重复元素。

```R
intersect(c(5, 7), c(1, 5, 2, 5))
## [1] 5
```

用`union(x,y)`求并集，结果中不含重复元素.

```R
union(c(5, 7), c(1, 5, 2, 5))
## [1] 5 7 1 2
```

用`setdiff(x,y)`求差集，即`x`的元素中不属于`y`的元素组成的集合， 结果中不含重复元素。

```R
setdiff(c(5, 7), c(1, 5, 2, 5))
## [1] 7
```

用`setequal(x,y)`判断两个集合是否相等， **不受次序与重复元素的影响**。

```R
setequal(c(1,5,2), c(2,5,1))
## [1] TRUE
setequal(c(1,5,2), c(2,5,1,5))
## [1] TRUE
```

### 5.1R数据类型

R的变量可以存储多种不同的数据类型， 可以用`typeof()`函数来返回一个变量或表达式的类型。

```R
typeof(1:3)
## [1] "integer"
typeof(c(1,2,3))
## [1] "double"
typeof(c(1, 2.1, 3))
## [1] "double"
typeof(c(TRUE, NA, FALSE))
## [1] "logical"
typeof('Abc')
## [1] "character"
typeof(factor(c('F', 'M', 'M', 'F')))
## [1] "integer"
```

R中数据的最基本的类型包括logical, integer, double, character, complex, raw。

为了判断某个向量`x`保存的基本类型， 可以用`is.integer(x)`, `is.double(x)`, `is.numeric(x)`, `is.logical(x)`, `is.character(x)`, `is.complex(x)`, `is.raw(x)`。 其中`is.numeric(x)`对integer和double内容都返回真值。

在R语言中数值一般看作double, 如果需要明确表明某些数值是整数， 可以在数值后面附加字母L。

```R
is.integer(c(1, -3))
## [1] FALSE
is.integer(c(1L, -3L))
## [1] TRUE
```

`NULL`值可以用来表示类型未知的零长度向量，`NA`与`NULL`不同，前者是有类型的，表示存在但未知，后者代表不存在。

#### 类型转换与类型升档

可以用`as.xxx()`函数在不同类型间强制转换。

```R
as.numeric(c(FALSE, TRUE))
## [1] 0 1
as.character(sqrt(1:4))
## [1] "1" "1.4142135623731" "1.73205080756888" "2"
```

在用`c()`函数合并若干元素时， 如果元素基本类型不同， 将统一转换成最复杂的一个。不同类型参与要求类型相同的运算时， 也会统一转换为最复杂的类型。复杂程度从简单到复杂依次为： `logical<integer<double<character`。 这种做法称为类型升档。

```R
c(FALSE, 1L, 2.5, "3.6")
## [1] "FALSE" "1"     "2.5"   "3.6"
TRUE+10
## [1] 11
paste("abc",1)
## [1] "abc 1"
```

#### 属性

R的变量都可看作是对象，都具有属性，如维度，类属等。R对象一般都有`length`和`mode`两个属性。常用属性有`names`, `dim`，`class`等。

对象`x`的所有属性可以用`attributes()`读取。

```R
x <- table(c(1,2,1,3,2,1)); print(x)
## 
## 1 2 3 
## 3 2 1
attributes(x)
## $dim
## [1] 3
## 
## $dimnames
## $dimnames[[1]]
## [1] "1" "2" "3"
## 
## 
## $class
## [1] "table"
```

`table()`函数的结果有三个属性，前两个是dim和dimnames, 这是数组(array)具有的属性； 另一个是class属性，值为`"table"`。

对于数组的访问：

```R
x[1]
## 1 
## 3
```

也可以用`attributes()`函数修改属性，修改后`x`即非数组也非table。

```R
attributes(x) <- NULL
x
## [1] 3 2 1
```

可以用`attr(x, "属性名")`的格式读取或定义`x`的属性，这样的属性常常称为“元数据”。

```R
x <- c(1,3,5)
attr(x, "theta") <- c(0, 1)
print(x)
## [1] 1 3 5
## attr(,"theta")
## [1] 0 1
```

有元素名的向量、列表、数据框等都有`names`属性， 许多R函数的输出本质上也是列表， 所以也有`names`属性。 `names`用`names(x)`的格式读取或设定。对于没有元素名的向量`x`，`names(x)`的返回值是`NULL`。

#### 类属

R具有一定的面向对象语言特征， 其数据类型有一个`class`属性， 函数`class()`可以返回变量类型的类属。

```R
typeof(factor(c('F', 'M', 'M', 'F')))
## [1] "integer"
mode(factor(c('F', 'M', 'M', 'F')))
## [1] "numeric"
storage.mode(factor(c('F', 'M', 'M', 'F')))
## [1] "integer"
class(factor(c('F', 'M', 'M', 'F')))
## [1] "factor"
```

用`str()`函数可以显示对象的类型和主要结构及典型内容。

```R
s <- 101:200
attr(s,'author') <- '李小明'
attr(s,'date') <- '2016-09-12'
str(s)
##  int [1:100] 101 102 103 104 105 106 107 108 109 110 ...
##  - attr(*, "author")= chr "李小明"
##  - attr(*, "date")= chr "2016-09-12"
```

### 5.2因子类型

R中用因子代表数据中的分类变量。使用`factor()`函数把字符型向量转换成因子。

```R
x <- c("男", "女", "男", "男",  "女")
sex <- factor(x)
sex
## [1] 男 女 男 男 女
## Levels: 男 女
level(sex)
## [1] "男" "女"
```

连续取值的变量，可以用`cut()`函数将其分段， 转换成因子。 使用`breaks()`参数指定分点， 最小分点要小于数据的最小值， 最大分点要大于等于数据的最大值， 默认使用左开右闭区间分组。

```R
f<-cut(1:10, breaks=c(0, 5, 10))
##  [1] (0,5]  (0,5]  (0,5]  (0,5]  (0,5]  (5,10] (5,10] (5,10] (5,10] (5,10]
## Levels: (0,5] (5,10]
#modify the level
levels(f) <- c("a", "b")
f
## [1] a a a a a b b b b b
```

`table()`函数统计因子出现的次数，也可以对一般的向量统计每个不同元素出现的次数。

```R
table(sex)
## sex
## 男 女 
##  3  2
```

`tapply()`函数可以按照因子分组然后每组单独计算统计量。

```R
height <- c(165, 170, 168, 172, 159)
tapply(height, sex, mean)
##       男       女 
## 168.3333 164.5000
```
