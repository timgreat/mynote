## R统计模型

### R相关与回归

#### 相关分析

考虑连续型随机变量之间的关系，相关系数（pearson相关系数）的定义为

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803132424008.png" alt="image-20230803132424008" style="zoom:67%;" />

rho接近+1表示X与Y正相关，接近-1表示负相关；相关系数表示的是**线性**相关性，对于<img src="/home/timi/.config/Typora/typora-user-images/image-20230803133529268.png" alt="image-20230803133529268" style="zoom: 67%;" />

样本相关系数为：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803133745132.png" alt="image-20230803133745132" style="zoom: 67%;" />

```R
set.seed(1)
nsamp <- 30
x <- runif(nsamp, -10, 10)
y <- 20 + 0.5*x + rnorm(nsamp,0,0.5)
plot(x, y)
```



<img src="/home/timi/.config/Typora/typora-user-images/image-20230803134455282.png" alt="image-20230803134455282" style="zoom:67%;" />

```R
set.seed(1)
y2 <- 0.5*x^2 + rnorm(nsamp,0,2)
plot(x, y2)
```

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803134554734.png" alt="image-20230803134554734" style="zoom:67%;" />

#### 相关系数的性质

- 取值范围$$-1\leq r \leq1$$

- 变量ax+b与cy+d的相关系数等于x与y的相关系数；
- 如果和之间是非线性相关， 相关系数不一定能准确反映其关系， 只能作为一定程度的近似。

使用`cor(x,y)`计算样本相关系数：

```R
set.seed(1)
x <- runif(30, 0, 10)
xx <- seq(0, 10, length.out = 100)
y <- 40 - (x-7)^2 + rnorm(30)
yy <- 40 - (xx-7)^2
plot(x, y, pch=16)
lines(xx, yy)

cor(x,y)
## [1] 0.8244374
```

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803141019021.png" alt="image-20230803141019021" style="zoom:67%;" />

#### 相关与因果

相关系数是从数据中得到的和两个变量关系的一种描述，**不能直接引申为因果关系。**

#### 相关系数大小与检验

- 相关系数绝对值在0.8以上认为高度相关。
- 在0.5到0.8之间认为中度相关。
- 在0.3到0.5之间认为低度相关。
- 在0.3以下认为不相关或相关性很弱以至于没有实际价值。
- 在特别重要的问题中， 只要经过检验显著不等于零的相关都认为是有意义的。

相关系数$$r$$是总体相关系数$$\rho$$的估计。<img src="/home/timi/.config/Typora/typora-user-images/image-20230803141455966.png" alt="image-20230803141455966" style="zoom:67%;" />

检验统计量<img src="/home/timi/.config/Typora/typora-user-images/image-20230803141545876.png" alt="image-20230803141545876" style="zoom:67%;" />

当总体（X，Y）服从二元正态分布且$$H_0 : \rho = 0$$成立时，$$t$$服从$$t(n-2)$$分布。设$$t$$统计量为$$t_0$$，p值为<img src="/home/timi/.config/Typora/typora-user-images/image-20230803141856767.png" alt="image-20230803141856767" style="zoom:67%;" />。

使用`cor.test(x,y)`计算检验。

```R
cor.test(d.class$height, d.class$weight)
## 
##  Pearson's product-moment correlation
## 
## data:  d.class$height and d.class$weight
## t = 7.5549, df = 17, p-value = 7.887e-07
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.7044314 0.9523101
## sample estimates:
##       cor 
## 0.8777852
```

可以用ggstatsplot包将相关系数检验进行图示：

```R
library(ggstatsplot)
ggscatterstats(
  data  = d.class,
  x     = height,
  y     = weight,
  xlab  = "身高",
  ylab  = "体重",
  title = "体重与身高的相关性"
)
```

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803144127672.png" alt="image-20230803144127672" style="zoom:67%;" />

#### 相关系数检验的功效与样本量

pwr包的`pwr.r.test()`可以计算相关系数检验的功效和样本量。

检验“小”的效应， 功效为， 需要的样本量：

```R
cohen.ES(test = "r", size = "small")
## 
##      Conventional effect size from Cohen (1982) 
## 
##            test = r
##            size = small
##     effect.size = 0.1
pwr.r.test(
  r = 0.1,
  power = 0.80,
  alternative = "two.sided"
)
## 
##      approximate correlation power calculation (arctangh transformation) 
## 
##               n = 781.7516
##               r = 0.1
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
```

#### 相关阵

如果`x`是一个仅包含数值型列的数据框， 则`cor(x)`计算样本相关系数矩阵； `var(x)`计算样本协方差阵。

`corrgram::corrgram()`可以绘制相关系数矩阵的图形， 用颜色和阴影浓度代表相关系数正负和绝对值大小， 用饼图中阴影部分大小代表相关系数绝对值大小。 

```R
library(corrgram)
corrgram(
  baseball[,c("Assists","Atbat","Errors","Hits","Homer","logSal",
           "Putouts","RBI","Runs","Walks","Years")], 
  order=TRUE, main="Baseball data PC2/PC1 order",
  lower.panel=panel.shade, upper.panel=panel.pie)
```

其中选项`order=TRUE`重排各列的次序使得较接近的列排列在相邻位置。

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803151434677.png" alt="image-20230803151434677" style="zoom:67%;" />

#### Spearman秩相关系数

Spearman秩相关系数也成为Spearman rho系数， 是两个变量的秩统计量的相关系数。

```R
cor.test(d.class$height, d.class$weight,
  method="spearman")
## 
##  Spearman's rank correlation rho
## 
## data:  d.class$height and d.class$weight
## S = 164.43, p-value = 2.983e-06
## alternative hypothesis: true rho is not equal to 0
## sample estimates:
##       rho 
## 0.8557611
```

#### Kendall tau系数

当变量（X，Y）正相关性很强时，任意两个观测的X值的大小顺序相同；如果独立，一对观测的X值比较和Y值比较顺序相同与顺序相反的数目应该基本相同。Kandall tau系数也是取值于区间， 用这样的思想表示两个变量的相关性和正负。

```R
cor.test(d.class$height, d.class$weight,
  method="kendall")
##  Kendall's rank correlation tau
## 
## data:  d.class$height and d.class$weight
## z = 4.2136, p-value = 2.513e-05
## alternative hypothesis: true tau is not equal to 0
## sample estimates:
##       tau 
## 0.7142984
```

### 一元回归分析

#### 模型

考虑自变量X与因变量Y的关系，希望使用X值的变化去解释Y值的变化。

假设模型：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803153045616.png" alt="image-20230803153045616" style="zoom:67%;" />

在回归分析中，通常假定X是非随机的，而Y的随机性来自误差项$$\epsilon$$，实际应用中X也常常是随机的。

#### 最小二乘法

直观上看，要找最优的直线y=a+bx使得直线与观测的点最为接近。例如：

```R
set.seed(1)
nsamp <- 30
x <- runif(nsamp, -10, 10)
y <- 20 + 0.5*x + rnorm(nsamp,0,0.5)
plot(x, y)
abline(lm(y ~ x), col="red", lwd=2)
```

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803154130694.png" alt="image-20230803154130694" style="zoom:67%;" />

a,b的解用最小二乘得到：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803155121804.png" alt="image-20230803155121804" style="zoom:67%;" />

最小二乘解的表达式为

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803155215663.png" alt="image-20230803155215663" style="zoom:67%;" />

其中$$r_{xy}$$为X与Y的样本相关系数，$$S_x$$与$$S_y$$分别为X与Y的样本标准差。

随机误差方差$$\sigma^2$$的估计取为：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803160038723.png" alt="image-20230803160038723" style="zoom:67%;" />

标准误差：为衡量$$\hat{a}$$和$$\hat{b}$$的估计精度，计算SE($$\hat{a}$$)和SE($$\hat{b}$$)。

估计结果：

- 系数估计即相应的标准误差估计
- 拟合值（预测值）
- 残差
- $$SSE = \sum_i{{e_i}^2}$$称为残差平方和，越小，拟合效果越好。

#### 回归有效性

##### 拟合优度指标

按照最小二乘估计公式，只要$$x_i$$不全相等， 不论之间有没有相关关系， 都能计算出参数估计值。 得到的回归直线与观测数据的散点之间的接近程度代表了回归结果的优劣。

残差平方和为：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803165420761.png" alt="image-20230803165420761" style="zoom:67%;" />

残差平方和越小， 说明回归直线与观测数据点吻合得越好。 均方误差为：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803165502741.png" alt="image-20230803165502741" style="zoom:67%;" />

$$y$$是因变量， 因变量的数据的离差平方和：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803165748304.png" alt="image-20230803165748304" style="zoom:67%;" />

代表了因变量的未知变动大小， 需要对这样的变动用自变量进行解释， 称SST为总平方和。

SSR是回归平方和：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803165927314.png" alt="image-20230803165927314" style="zoom:67%;" />

在总平方和中， 回归平方和是能够用回归斜率与自变量的变动解释的部分， 残差平方和是自变量不能解释的变动。 分解中，回归平方和越大，误差平方和越小， 拟合越好。

有如下平方和分解公式：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803170025023.png" alt="image-20230803170025023" style="zoom:67%;" />

令

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803170052135.png" alt="image-20230803170052135" style="zoom:67%;" />

是回归的复相关系数，或判定系数。判定系数越大，回归拟合越好。在一元回归中$$R^2$$是x和y的样本相关系数的平方。在多元回归时只能用复相关系数。

估计误差项方差$$\sigma^2$$为：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803171141311.png" alt="image-20230803171141311" style="zoom:67%;" />

$$\hat{\sigma}$$是$$\sigma$$的估计，R的回归结果显示为"residual standard error"（残差标准误差）。$$\hat{\sigma}$$是残差$$e_i = y_i-\hat{y_i}$$的标准差的一个粗略估计。

##### 线性关系显著性检验

当b=0时，模型退化为$$Y=a+\epsilon$$，X不出现在模型中，说明Y与X不相关。检验

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803174258217.png" alt="image-20230803174258217" style="zoom:67%;" />

取统计量：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803174315626.png" alt="image-20230803174315626" style="zoom:67%;" />

检验P值为$$P(F(1,n-2)>c)$$（设c为F统计量的值）。取检验水平$\alpha$，p值小于等于$\alpha$时拒绝$H_0$，认为y与x有显著的线性相关关系，否则认为y与x没有显著的线性相关关系。

也可以使用t统计量<img src="/home/timi/.config/Typora/typora-user-images/image-20230803185706359.png" alt="image-20230803185706359" style="zoom:67%;" />，其中<img src="/home/timi/.config/Typora/typora-user-images/image-20230803185728047.png" alt="image-20230803185728047" style="zoom:67%;" />，设t统计量为c，检验P值为

$P(|t(n-2)| >|c|)$。

#### R程序

设数据保存在数据框d中，变量名为y和x，用R的`lm()`函数计算回归，如:

```R
set.seed(1)
nsamp <- 30
x <- runif(nsamp, -10, 10)
y <- 20 + 0.5*x + rnorm(nsamp,0,0.5)
d <- data.frame(x=x, y=y)
lm1 <- lm(y ~ x, data=d); lm1
## 
## Call:
## lm(formula = y ~ x, data = d)
## 
## Coefficients:
## (Intercept)            x  
##     20.0388       0.4988
summary(lm1)
## 
## Call:
## lm(formula = y ~ x, data = d)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -1.03030 -0.16436 -0.05741  0.29511  0.64046 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 20.03883    0.07226   277.3   <2e-16 ***
## x            0.49876    0.01244    40.1   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.3956 on 28 degrees of freedom
## Multiple R-squared:  0.9829, Adjusted R-squared:  0.9823 
## F-statistic:  1608 on 1 and 28 DF,  p-value: < 2.2e-16
```

#### 回归诊断

除了系数的显著性检验以外， 对误差项是否符合回归模型假定中的独立性、方差齐性、正态性等， 可以利用回归残差进行一系列的回归诊断。 还可以计算一些异常值、强影响点的诊断。

对n组观测值，回归模型为：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803193519996.png" alt="image-20230803193519996" style="zoom:67%;" />

拟合值为：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803193543827.png" alt="image-20230803193543827" style="zoom:67%;" />

残差为：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803193609098.png" alt="image-20230803193609098" style="zoom:67%;" />

残差相当于模型中的随机误差， 但仅在模型假定成立时才是随机误差的合理估计。 所以残差能够反映违反模型假定的情况。

$e_i$有量纲，因此定义标准化残差：

<img src="/home/timi/.config/Typora/typora-user-images/image-20230803193833036.png" alt="image-20230803193833036" style="zoom:67%;" />

样本量较大时标准化残差近似服从标准正态分布。R中用`residuals()`从回归结果计算残差， 用`rstandard()`从回归结果计算标准化残差。

### R多元回归

