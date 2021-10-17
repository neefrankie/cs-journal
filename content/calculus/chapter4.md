---
title: "简易微积分：第4章 最简单的例子"
date: 2021-10-17T15:58:53+08:00
draft: true
---

## 第4章 最简单的例子

现在我们来看看，如何对一些简单的代数表达式进行微分。

### 例1

我们先从简单表达式 y = x² 开始。现在要记得，微积分最根本的观点是*增长*。数学家称之为变动（varying）。由于y和x²相等，很明显，如果x增长，x²也会增长。如果x²增长，y也随之增长。我们需要找出的是y的增长和x的增长之间的比率。花奴讲话说，我们的任务是找出dy和dx的比例，即找出 dy/dx。

假设x增大了一点点，变为x+dx；同样，y就会增大一点点，变为y+dy。显然，变大的y依然等于变大的x的平方。写出来就是：

y + dy = (x + dx)²

展开得到：

y + dy = x² + 2x * dx + (dx)²


(dx)²表示什么？别忘了dx表示一点点 -- x的一点点。那么(dx)²就表示x的一点点的一点点；如前所述，就是一点点的二阶微量。和其他项相比， (dx)²微不足道，因为可以丢弃。去掉(dx)²后得到：

y + dy = x² + 2x * dx

由于 y = x²，从上述等式中减去这两项得到：

dy = 2x * dx

两边除以dx得到：

dy / dx = 2x

这就是我们要找的结果。在本例中，y的增量相对于x的增量比率是 2x。

#### 数字举例

假设 x = 100，则y = 10000。让x增长到101（即dx = 1）。增长后的y就是 101 x 101 = 10201。不过，如果我们认同二阶微量可以忽略不计，那么相对于10000就可以忽略1，于是取整得到10200。y从10000增长到10200，因此增量dy是200。

dy / dx = 200 / 1 = 200

根据前面的代数计算，我们找出来 dy / dx = 2x。于是，对于x = 100， 2x = 200.

但是，你会问，我们忽略掉了一个数字。

那么试一试让dx取一个更小的数。

试试让dx = 1 / 10。那么x + dx = 100.1，则：

(x + dx)² = 100.1 * 100.1 = 10020.01

最后一个数字只是10000的百万分之一，完全可以忽略，我们就略去末尾的小数，取整10020。因为得到dy = 20，dy / dx = 20 / 0.1 = 200，依然是2x。

### 例2

试试用同样的方式微分 y = x³。

让y增长到 y + dy，x增长到x + dx。得到：

y + dy = (x + dx)³

展开立方得到：

y + dy = x³ + 3x * dx + 3x(dx)² + (dx)³

现在我们知道，dy和dx都无限小时，可以忽略二阶和三阶微量，相比之下(dx)²和(dx)³就变得无限小。忽略它们得到：

y + dy = x³ + 3x² * dx

由于 y = x³，消去后得到：

dy = 3x² * dx

即

dy / dx = 3x²

### 例3

试试微分 y = x⁴。想前面一样，让y和x都增长一点点：

y + dy = (x + dx)⁴

展开4次方后得到：

y + dy = x⁴ + 4x³dx + 6x²(dx)² + 4x(dx)³ + (dx)⁴

所有包含dx高阶指数的项相比之下都可以忽略不计，划去后得到：

y + dy = x⁴ + 4x³dx

消去最初的y = x⁴后得到：

dy = 4x³dx

即

dy/dx = 4x³

---

这些例子都相当简单。我们把结果收集起来，看看能不能推导出什么一般规律。把结果放到一个表格中，一列时y的值，一列时对应的dy/dx的值：

| y   | dy/dx |
| --- | ----- |
| x²  | 2x    |
| x³  | 3x²   |
| x⁴  | 4x³   |

观察这些结果：微分操作的效果似乎是把x的指数降低1，同时乘以这个数字（即最初作为指数出现的那个数字）。看到这个规律后，你可能和容易推测出来其他情况怎么处理。微分x⁵得到5x⁴，x⁶得到6x⁵。如果你拿不准，试一下，看看推测是否正确。

试一下 y = x⁵

y + dy = (x + dx)⁵
       = x⁵ + 5x⁴ dx + 10x³(dx)² + 10x²(dx)³ + 5x(dx)⁴ + (dx)⁵

忽略所有包含高阶微量的项后得到：

y + dy = x⁵ + 5x⁴ dx

消去 y = x⁵ 后得到

dy = 5x⁴ dx

据此

dy/dx = 5x⁴ dx

---

沿着我们的观察可以得出结论，如果想要处理任何更高的指数（假设n），可以按照同样的方式解决。

设

y = xⁿ

可以得到

dy/dx = nx^(n-1)

如，n=8时，y=x^8，微分得到dy/dx = 8x^7。

实际上，当n为正整数时，微分x^n得到结果nx^(n-1)适用于任何情况。（用二项式定理展开(x + dx)^n马上会看到）。当时，n为负数或者分数小数时这一规律是否正确还需进一步研究。

### 负指数

设 y = x^(-2)，按前述方式：

y + dy = (x + dx)^(-2)
       = x^(-2)(1 + dx/x)^(-2)

用二项式定理展开后得到：

忽略微量的高阶微量，得到：

消去最初的y = x^(-2)，得到：

dy = -2x^(-3)dx,
dy/dx = -2x^(-3)

依然符合上述推断的规律。

### 分数指数

设 y = x^(1/2)，如前展开：


消去最初的 y = x^(1/2)，略去高阶指数得到：

依然符合通用规则。

### 总结

现在我们看看学到了什么。我们已经总结出如下规则：要微分x^n，乘以指数的数字，指数减去一，结果为 `nx(n-1)`。