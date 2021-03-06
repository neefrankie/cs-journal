---
title: "简易微积分：第3章 论相对增长"
date: 2021-06-18T09:54:41+08:00
draft: true
---

## 第3章 论相对增量

在微积分中，从头到尾都在处理增长的量和增长的比率。数量可以分成两类：常量(constant)和变量(variable)。固定的值称为常量，通常用字母表开始的字母表示，如a、b或c。可以增长的量，或者如数学家所言的“变量”，使用字母表末尾的字母表示，如x、y、z、u、v、w，有时也用t。

此外，通常一次需要处理的变量不止一个，一个变量会依赖其他变量：例如，抛射出去的物体能达到的高度，取决于达到这一高度所需的时间。又如这样的问题：一个给定面积的长方形，长度增长，则宽度必须相应缩短。亦或是一个梯子的斜率变化会导致它所能达到的高度不同。

假设我们有两个这样的变量相互依赖，这种依赖关系导致一个变量变化会引发另一个变量变化。假设我们称其中一个变量是x，另一个依赖x的变量是y。

假设我们让x变化，增加的量称为dx，x变成了x+dx。因为x变化了，y也随之变化，成为y+dx。这里的dy可能是正的，也可能是负的，不过都不会和dx相等（除非是奇迹）。

看两个例子。

(1) 设x和y分别是一个直角三角形的底边和高，斜边和底边的夹角是30°。假设这个三角形扩大，角度不变，底边增长到x+dx，高度变为y+dy。这里，x增加导致y增加。小三角形的高是dy，底边是dx，与原始的三角形相似。显然，dy / dx的比值不变，同y / x。由于角度是30°，所以

dy / dx = 1/1.73

![Right angled triangle](/static/calculus-made-easy/chapter3-triangle.png)

(2) 下图中，AB是一架梯子，长度固定，底端距墙的水平距离用x表示，顶端达到墙的高度用y表示。

![Ladder](/static/calculus-made-easy/chapter3-ladder.png)

显然，y依赖x。很容易看出来，如果把底端A拉得离墙远一点，顶端B就会降低一点。我们用科学的语言来表述：如果x增加到x+dx，那么y就成为y - dy。也就是说，x增量为正数，导致y的增量为负数。

没错，不过变化是多少呢？假设梯子很长，底端A距离墙壁19英寸(48.26厘米)时，顶端B距离地面15英尺(4.572米)。现在，如果把底端向外拉1英寸，顶端会降低多少？都换成英寸：x = 19英寸，y = 180英寸(1英尺为12英寸)。x的增量dx是1英寸：x + dx = 20英寸。

y会减少多少？新的高度是y-dy。用欧几里得《几何原本》卷一命题17（即勾股定理）计算高度，就能找出dy是多少。梯子的长度是

√(180²  +19²) = 181 英寸

新的高度y-dy就是

(y - dy)² = 181² - 20² = 32761 - 400 = 32361

y - dy = √32361 = 179.89 英寸

y是180，那么dy就是 180 - 179.89 = 0.11 英寸

我们看到，dx增加1英寸，导致dy减少0.11英寸。

dy比dx就是：

dy / dx = -(0.11 / 1)

也很容易看到，dy和dx的值不同。

现在，我们在微分学中寻寻觅觅，捕捉一个不寻常的东西，一个比例，即当dy和dx无限小时，dy与dx的比值。

需要注意的是，y和x必须以某种方式有关联，这样每当x变化，y也随之变化，只有这种情况下才能找出dy / dx的比例。例如，在上面的第一个例子中，三角形的底边变长，高y就变大。在第二个例子中，如果梯子底边的距离x增加，梯子达到的高度y就随之降低，开始很慢，但随着x增加越来越快。这些例子中，x和y的关系完全确定，可以用数学方法表达，即 y / x = tan30° 和 x² + y² = l²(l是梯子的长度)，dy/dx就有各例中的含义。

假设x依然代表梯子底端到墙的距离，y则不再代表梯子达到的高度，而是墙的水平投影长度，或者墙上砖的数量，或者建成的年数，那么x的任何变化自然不会引发y的的变化。这种情况下dy/dx就没有任何意义，也无法表达这一关系。使用dx、dy、dz等等微分时，就表明x、y、z之间存在某种程度的关系，这种关系称为x、y、z的“函数”。例如，上述两个表达式，即 y / x = tan30° 和 x² + y² = l²，是x和y的函数。此类表达式暗含了可以使用y表达x，或者用x表达y，因此被称为x和y的“隐式函数”(implicit function)。他们可以分别写成公式：

y = x * tan 30° 或 x = y / (tan 30°)

以及 y = √(l² - x²) 或 x = √(l² - y²)

上述表达式显示地表明，x的值可以用y表示，或者y的值可以用x表示，因此被称为x或y的显示函数(explicit function)。例如，x² + 3 = 2y - 7是x和y中的隐式函数，可以写成 y = (x² + 10) / 2（x的显示函数）或者 x = √(2y - 10) (y的显示函数)。我们看到，x、y、z等中的显示函数，就是值随着x、y、z的变化而变化，有的是一个值在变化，有的是几个值变化。因此，显示函数的值被称为因变量(dependent variable)，因为它依赖于函数中其他变量的值；其他的各个变量被称为自变量(independent variable)，因为它们的值不取决于函数采用的值。例如，对于 u = x² * sin θ，x和θ是自变量，u是因变量。

有时，几个量x、y、z之间的确切关系要么未知，要么不便于表述，只是知道或者便于描述这些变量之间存在关系，无法在不影响其他量的情况下单方面改变x、y或z。x、y、z中存在的函数可以用符号 F(x,y,z) 表示（隐式函数），或者表示为 x = F(y,z)、y = F(x,z) 或 z = F(x,y)（显示函数）。有时不用字符F，而是使用 f 或者 Φ，y = F(x)、y = f(x)、y = Φ(x) 含义相同，即y的值取决于x的值，但并未表明具体的方式。

我们把 dy/dx 的比值称为“y对x的微分系数”(differential coefficient)，这是对这个简单事物的严肃的科学称呼。不过我们不要被这种严肃的名称吓倒，事情本身很简单。相反，我们只会诅咒一下起这种拗口名称的蠢行。一旦解除了心智负担，我们继续看这个简单的东西，也就是dy/dx的比值。

在学校时期学的普通代数中，你总在搜寻某些未知的量，称为x或者y；或者有时需要同时寻找两个未知量。现在，你需要学习一种新的搜寻方式，捕捉的猎物既不是x也不是y，而是搜捕称为dy/dx的幼兽。找出dy/dx值的过程叫做“微分”。不知只要记住，我们要找的是当dy和dx无限小时，它们的比值。微分系数表示的是在极限中，当dy和dx无限小时趋近的值。

### 微分怎么读

如果犯中小学生的错误，认为dx表示d乘以x，那是不行的，因为d不是因数，它表示“...的一点点”。dx读作dee-eks。

如果读者无人指导，只需按如下方式读微分系数即可。微分系数

dy/dx 读作 “dee-wy by dee-eks”或者“dee-wy over dee-eks”

后面会学到二阶微分系数，像这样：

d²y / dx² 读作 “dee-two-wy over dee-eks-sqauired”

表示用y对x执行两次微分。

另一个表示一个函数被微分的方式是在函数符号前面加上一种重音符。因此如果 y = F(x)，即y是x的某个未知方程，可以写作F‘(x)。同样，F''(x)表示原始函数F(x)相对于x被微分了两次。
