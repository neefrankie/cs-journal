---
title: "Merge Sort"
date: 2021-06-25T15:57:33+08:00
draft: true
---

## 归并排序(mergesort)

归并排序使用递归，是一种更加有效的排序方法。冒泡排序、插入排序和选择排序耗时O(N²)，归并排序则是O(N*logN)。

归并排序实现起来相当容易，理解起来也比快速排序和希尔排序更容易。

归并排序的缺点是需要需要在内存中创建了一个额外的数组，和被排序的数组同样大小。如果内存存储原始数组就很勉强，归并排序没法用。不过，如果内存充足，归并排序就是个不错的选择。

### 合并两个已排序数组

归并排序算法的核心是合并两个已经排好顺序的数组。合并两个排序数组A和B到新创建的第三个数组C，C包含了A和B的所有元素，并且排好了顺序。我们先看合并过程，稍后再看如何在排序中使用。

假设有两个已排序数组，长度不必相同。假设数组A有4个元素，数组B有6个元素。它们将被合并到有10个空位置的数组C中。下图展示了这些数组。

![Merging two arrays](/static/algorithms/mergesort-merging-two-arrays.md)

图1

图中带圈的数字表示元素从A和B转移到C的顺序。下表显示了为确定哪个元素需要复制而做进行的必要比较。每次比较后，较小的元素被复制到了C。

注意，第8步以后，数组B空了，不再需要更多比较，数组A中的剩余元素直接复制到了数组C。

### 实现

### 通过归并进行排序

归并排序的思想是，把一个数组一分为二，分别给两个二分之一数组排序，然后使用`merge()`方法把两个二分之一数组合并到一个单一的排序数组中。每个二分之一数组又怎么排序呢？二分之一再分为两个四分之一，分别对每个四分之一数组排序，合并成一个排好顺序的二分之一数组。

同样，每对八分之一数组合并为一个排好顺序的四分之一数组，每对十六分之一的数组合并为一个八分之一数组。反复分割数组，直到某个子数组只有一个元素，这就是基准条件：只有一个元素的数组已经排好了顺序。

我们看到，一般地，每次一个递归函数调用自己，某种东西的大小就变小，每次函数返回后再一层一层地积累起来。在`mergeSort()`中，每次范围分为一半，每次返回就把两个较小的范围合并为较大的一个。

`mergeSort()`从寻找两个由一个元素构成的数组返回后，就把这两个数组合并成由两个排好顺序的元素构成的数组。由此构成的每一对两元素数组又合并成四元素数组。这一过程持续下去，数组越来越大，直到整个数组都排序完成。当原始数组长度是2的指数时，容易看出这一过程。如下图所示。

![Merging larger and alrger arrays](/static/algorithms/mergesort-merging-larger-and-larger-arrays.png)

图2

首先，在数组底部，子数组0-0和子数组1-1合并进了子数组0-1。子数组0-0和1-1是基准条件。同样，2-2和3-3合并进了2-3。然后0-1和2-3合并进了0-3.

在数组的上半部分，4-4和5-5合并进了4-5，6-6和7-7合并进了6-7，而4-5和6-7又合并进了4-7.最终，上半部分0-3和下半部分4-7合并进了整个数组0-7，数组由此完成排序。

如果数组大小不是2的指数，必须合并不同大小的数组。例如，下图中数组长度是12，长度为2的数组必须和长度为1的数组合并成长度为3的数组。

![Array sixz not a power of 2](/static/algorithms/mergesort-array-size=not-a-power-of-two.png)

图3

首先，长度为1的数组0-0和1-1合并到2个元素的数组0-1。然后，0-1和1个元素的2-2合并。这就创建了3个元素的0-2，然后同3个元素的3-5合并。这个过程持续下去，直到数组完成排序。

注意，在归并算法中，我们不会像上面展示的那样，把两个单独的数组合并到第三个中，而是把数组的一部分合并到它自己里面。

你可能会想，这些子数组都放在内存的什么地方？在这个算法中，会创建一个同原始数组同样大小的工作区数组。子数组存储在工作区数组的一部分区域。这意味着，原始数组中的子数组会被复制到工作区数组的恰当位置。在每次合并后，工作区数组被复制回原始数组。

### 实现

### 排序算法的效率

归并排序耗时O(N*logN)。怎么知道的呢？我们来看看，在归并算法中，如何分析一个数据元素必须复制的次数和必须与另一数据元素比较的次数。我们假设复制和比较是最耗时的操作，递归调用和返回并不增加多少开销。

#### 复制的次数

在图2中，顶部下面的每一格都代表从数组中向工作区复制一个元素。

把所有格子加起来（7个步骤中的所有格子），给8个元素排序需要24次复制。需要分割的次数是log₂8即3，每次分割8个元素都需要复制一遍，所以 8 x log₂8 = 24。这表明，对于8个元素，复制的次数同 N x log₂8 成正比。

换个角度看这个计算方法：给8个元素排序需要3层，每一层需要复制8次。一层指所有元素复制到同样大小的工作区。第一层有4组2个元素的子数组，第二层有2组4个元素的子数组，第三层有1个8元素的子数组。每一层有8个元素，那就是 3 * 8 = 24。

实际上，元素不仅被复制进了工作区，还要从工作区复制回原始数组。因此复制次数翻番。

如果N不是2的倍数，就更加难以计算复制和比较的次数，不过次数在N是2的指数的情况之间。对于12个元素，复制总次数是88，对于100个元素，复制总次数是1344.

#### 比较次数

在归并排序中，比较次数总是少于复制次数。少多少？假设元素数量是2的指数，对于每一次合并操作，最大比较次数总是比被合并的元素数量少一个，最小次数则是被合并元素数量的一半。下图显示了尝试合并2个4元素数组时的两种可能，可以验证这一结论。

![Maximum and minimum comparisons](/static/algorithms/mergesort-maximum-and-minimum-comparisons.png)

第一种情况下，元素交叉，必须做7次比较才能合并所有元素。第二种情况下，一个数组的所有元素都比另一个数组的所有元素小，所以只需4次比较即可。

每一次排序都有很多次合并，所以必须把每一轮比较都加起来。参考图2可以看出，给8个元素排序需要7次合并操作。被合并的元素数量和由此产生的比较次数如下表所示。


![Comparisons involved in sorting 8 items](/static/algorithms/mergesort-comparisons-involved-in-sorting-8-items.png)

对于每一次合并，最多比较的次数是元素数量减一，全部加起来总次数是17.

最少比较次数总是被合并元素数量的一半，加起来总数是12次比较。给特定数组排序，实际需要比较的次数取决于最初数据是怎么排放的，但介于最大和最小值之间。
