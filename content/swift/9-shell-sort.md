---
title: "Shell Sort"
date: 2021-06-26T20:17:03+08:00
draft: true
---

希尔排序(Shellsort)因唐纳德·L·希尔(Donald L. Shell)而得名。这位计算机科学家于1959年发现了该算法。希尔排序基于插入，排序但增添了一项新特性，极大改进了插入排序的性能。

希尔排序适于中等大小的数组，取决于具体的实现，可能多达数千个元素。它没有快速排序和其他O(N*logN)的排序快，所以并不适合非常大的文件。不过，比选择排序和插入排序这些O(N²)的排序快很多，还很容易实现，代码短而简单。

最糟糕的情况下，性能也不会比平均性能有大幅下降。有些专家推荐，几乎任何排序项目都可以从希尔排序开始，如果实践表明性能太慢，再转而采用更加高级的排序，如快速排序。

## 插入排序：复制次数太多

回忆一下插入排序。在排序过程中，标记左侧的元素内部排好了顺序，右侧则没有排序。算法移除标记位置的元素，存放在临时表中中。然后，从新的空缺位置左侧开始，逐个把已排序的元素向右移动一格，直到临时变量中的元素能够插入到合适的排序位置。

这就是插入排序的问题。假设一个很小的元素放在了最右侧原本应该放大元素的位置。要把这个小元素移动到左侧恰当的位置，所有中间的元素都必须向右移动一格。这个步骤需要接近N次复制，只为了一个元素。不是所有的元素都需要完全移动N个位置，但是元素平均需要移动N/2个位置，总共需要移动 N * (N/2) 次，复制 N²/2次。因此插入排序的性能是O(N²)。

如果可以把较小的元素一次向左移动多个位置，不用移动中间的每个元素，就能提升性能。

## N排序

希尔排序通过增大插入排序的元素间距，实现这种大间隔移动。排序完成后，就缩小一些间距，继续排序。这种排序中元素之间的间距称为“增量”(increment)，历来使用字母h表示。下图显示了增量为4是排序10个元素数组过程中的第一步。这里排序的是索引0、4和8上的元素。

![4-sorting 0, 4 and 8](/static/algorithms/shellsort-4-sorting.png)

再0、4、8排序完成后，算法移动一格，排序1、5、9。这个过程持续下去，直到所有间隔4的元素都排好顺序，也就是说每组间隔为4的元素之间都排好了顺序。

4间隔排序完成后，这个数组可与i认为是由四个子数组构成：(0,4,8)、(1,5,9)、(3,7)，每一组都完全排序了。子数组除了相互交叉外，都是独立的。

注意，在这个例子中，间隔4排序后，如果数组完全排序，每个元素距离其最终位置不超过两格。这就是数组“几乎”排序的含义，也是希尔排序的秘密。通过常见交错的、内部排序的数组元素，我们减少了完成排序所需的工作量。

插入算法操作的数组接近排序完成状态时最有效。如果只需移动元素两三个位置即可完成排序，那么运行时间几乎是O(N)。因此，数组再4排序后，我们可以间隔1，使用正常插入排序。相比于没有初步的4间隔排序而直接使用普通插入排序，4间隔排序和1间隔排序结合起来更快。

## 缩小间隔

我们已经看到给10个元素的数组排序时使用4做间隔。对于更大的数组，初始间隔应该更大。然后间隔反复缩小，直到变为1.

例如，一个1000个元素的数组可以先间隔364排序，再间隔121排序，然后间隔40排序，再间隔4，最后间隔1。产生间隔的数字序列称为间隔序列。这里显示的这个间隔序列由Knuth所创，很流行。从1开始，用递归表达式来产生：

h = 3*h + 1

初始值时1。下表显示了这个公式产生的序列。

![Knuth's interval sequence]()

| h  | 3*h + 1 | (h-1)/3 |
| ---- |------ | ------- |
| 1    | 4     |         |
| 4    | 13    | 1       |
| 13   | 40    | 4       |
| 40   | 121   | 13      |
| 121  | 364   | 40      |
| 364  | 1093  | 121     |
| 1093 | 3280  | 364     |

在这个排序算法中，首先在一个简短的循环中用序列生成公式找出初始间距。h的第一个值时1，公式h=h*3+1用来生成序列1、4、13、40、121、364等等。间距大于数组长度时停止。对于一个1000个元素的数组，第七个数1093太大了。因此，我们使用第六个数字364开始排序。然后，外部循环的每一轮使用相反的公式减小间距：

h = (h-1) / 3

这就是上表第三列的数据。逆公式生成相反的序列364、121、40、13、4、1。从364开始，序列中的每个数字用来对数组间隔n排序。当数组间隔1排序后，算法结束。

希尔排序为何比插入排序快很多？当h很大时，每一轮排序的元素数量很少，元素移动距离很远。这样效率就很高。随着h减小，每一轮排序的元素数量增加，但元素都已经接近最终的位置，这就比插入排序更有效。这些去世的结合让希尔排序非常有效。

注意，后面的排序（h更小时）不会打乱此前的排序（h较大时）。间隔40排序的数组元素，在间隔13排序后依然保持顺序。否则希尔排序就毫无用处。

### 实现

### 其他间隔序列

选择间隔序列有点像黑魔法。目前我们的讨论使用h=h*3+1来产生间隔序列，但其他间隔序列也用过，取得不同程度的成功。一个必须的要求时，逐渐缩小的序列最终值时1，这样最后一轮排序才能使用常规插入排序。

在希尔最初的论文中，他建议初始间距是N/2，也就是每一轮平分。因此，N=100的降序序列是50、25、12、6、3、1。这个方法的优势是在开始排序之前，不需要计算序列就能找出初始间隔。不过，事实证明，这不是最佳序列。虽然对多数数据，它仍比插入排序好，但有时运行时间会降到O(N²)，这不比插入排序好。

这个方法的一个变体是每个间隔除以2.2而不是2。对于100，产生的就是45、10、9、4、1。这比除以2好很多，避免了一些最糟糕情况下出现O(N²)。还需要一些额外的代码来保证，不管N是多少，最终的值都是1.这给出的结果和上表中Knuth序列不相上下。

另一个可能的降序间隔(来自Flamig)是：

```
if (h < 5)
    h = 1
else
    h = (5 * h - 1) / 11
```

通常认为，重要的一点是间隔序列大致应该为质数。也就是说，它们除了1之外，没有公因数。这条限制很可能让每一轮排序都能混合此前排序的所有元素。希尔最初的N/2序列效率低的原因就是没有加持这一原则。

### 希尔排序的效率

目前，除了一些特殊案例，还没有人能从理论上分析希尔算法的效率。根据实验，由不同的估计，从O(N<sup>3/2</sup>)到O(N<sup>7/6</sup>)之间不等。

下表显示了同较慢的插入排序和较快的快速排序相比，部分估计的O()值。

对于多数数据，如N<sup>3/2</sup>等较高的估计可能更加实际。