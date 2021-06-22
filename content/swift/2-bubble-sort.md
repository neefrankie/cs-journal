---
title: "数据结构与算法（Swift）1 - 冒泡排序"
date: 2021-06-05T11:07:51+08:00
draft: true
---

假设你的少年棒球队在操场上排成一列，如下图所示。标准的九名队员加一名替补上场训练。你想让队员从低到高（最矮的站左侧）站成一队拍张合照。如何排序呢？

![](/simple-sorting-illustration.png)

人类比计算机程序有优势。人可以一眼看清所有的队员，马上挑出个子最高的一位，不需要费劲测量比较每个人。而且，队员也不需要站在特定的位置。他们推推挤挤就能让出空间，可以站在别人前面或者后面。经过一番调整，很容易就能按顺序排好。

但计算机程序无法用这种方式迅速浏览数据。程序一次只能比较两个选手，因为比较操作符就是这么工作的。算法的这种“隧道视觉”（指看不到边缘视觉的现象）将一再出现。这种事情对人类看起来很简单，但是，算法无法看到全景，因此，必须专注于细节，遵循某些简单的规则。

## 冒泡排序

冒泡排序很慢，但是概念简单，所以我们从冒泡排序开始探索各种排序技巧。

### 用冒泡法对棒球队员排序

设想你眼睛近视（像计算机程序一样），一次只能看到两个棒球队员，这两人相邻，你站得离他们很近。这种情况下，如何对他们排序？假设有N个队员，他们站立的位置从左到右编号，最左侧是0，最右侧是N-1。

冒泡排序这样工作：从队列的最左侧开始，比较位置0和1的两个队员。如果左侧（位置0）的队员高，则交换两人。如果右侧的队员高，则什么都不做。然后向下移动一个位置，比较位置1和位置2上的队员。如果这次左侧队员高，则再次交换。如下图所示。

![](/simple-sorting-swap.png)

规则如下：

1. 比较两个队员；
2. 如果左侧的队员高，则交换；
3. 向右移动一个位置；

沿着队列反复执行下去，直到到达最右侧。此时尚未对所有人完成排序，但是最高的队员已经到了最右侧。因为只要遇到了最高的队员，每次比较两个队员时，最高的这名都会被交换，最后就被换到了队列最右侧。冒泡排序由此得名：随着算法往下执行，最大的元素“冒泡”到数组的顶端。

第一轮遍历数据后，进行了N-1次比较，做的交换次数则根据最初队员的排列，介于0到N-1之间。数组末尾的元素已经排好了顺序，不会再变动。

现在重新开始，从队列最左侧开始新一轮遍历。这次还是向右走，两两比较，需要的时候交换位置。不过这次执行到队列的倒数第二个位置N-2即可，因为最后一个位置N-1已经是最高的队员了。

这一规则可以表述为：一旦遇到一个已经排好顺序的队员，就从队列左侧重新开始。

重复执行上述操作，直到所有的队员都排好顺序。

### 代码

Windows和Linux均可安装Swift，不过可能需要手动设置的地方较多，在OS X上从App Store安装了Xcode即可使用。

以下是OSX Big Sur上的实现，需要安装Xcode来获取Swift工具链，但并不需要了解Xcode用法。简单的编辑器也可以，如VS Code。使用VS Code推荐安装扩展Maintained Swift Development Environment。不管使用Xcode还是文本编辑器，第一次使用Swift编译器可能遇到的问题：

* xcrun: error: unable to find utility "xctest", not a developer tool or in PATH

这个问题是Command Line Tool未设置。在Xcode菜单中，找到 Preferences -> Locations -> Command Line Tool，在下拉菜单中选中Xcode默认推荐的项即可

* error: root manifest not found

Swift所说的“root manifest”是指`Swift.Package`文件，按照`swift package init`创建项目不会出现此问题。

首先创建一个`BubbleSort`项目：

```sh
mkdir BubbleSort
cd BubbleSort
swift package init --type executable
touch Sources/BubbleSort/BubbleSort.swift
code .
```

打开`Sources/BubbleSort/BubbleSort.swift`：

```swift
func bubbleSort(a: [Int]) -> [Int] {

    guard a.count > 1 else {
        return a
    }
    
    var sortedArray = a
    for i in (0..<sortedArray.count).reversed() {
        for j in 0..<i {
            if sortedArray[j] > sortedArray[j+1] {
                sortedArray.swapAt(j, j+1)
            }
        }
    }
    
    return sortedArray
}
```

`Source/BubbleSort/main.swift`:

```swift
print(bubbleSort(a: [77, 99, 44, 55, 22, 88, 11, 00, 66, 33]))
```

Swift函数的参数是不变量，因此不能直接改动传入的数组中的元素。

编译运行程序有三种方式，在Terminal中的当前项目目录下运行：

* 使用`swiftc`:

```sh
swiftc -o .build/BubbleSort Sources/BubleSort/main.swift Sources/BubbleSort/BubbleSort.swift
```

编译好的二进制文件在当前项目下的`.build`目录下。执行它：`./.build/BubbleSort`，终端中可以看到排序完成的数组。

* 使用`swift run`命令编译并运行程序，终端中输出运行结果。

* 使用`swift build --show-bin-path`编译到`.build`目录下，`--show-bin-path`参数会显示编译完的二进制文件所在位置，如`/Users/xxx/BubbleSort/.build/x86_64-apple-macosx/debug`。

### 不变量

很多算法中，有些条件不会随着算法的执行而变化。这些条件称为不变量（invariant）。找出不变量有助于理解该算法。

在冒泡排序中，不变量就是右侧已经排好顺序的数据元素。

### 冒泡排序的效率

以10个元素为例，第一轮要做9次比较，第二轮8次，以此类推，比较的总数为：

9 + 8 + 7 + 6 + 5 + 4 + 3 + 2 + 1 = 45

概括起来，如果数组中有N个元素，第一次循环要做N-1次比较，第二轮N-2次...，求和公式为：

(N-1) + (N-2) + (N-3) + ... + 1 = N*(N-1)/2

当N等于10时，N*(N-1)/2是45。

因此，排序算法进行了(N^2)/2次比较（忽略-1，不会对结果有多大影响，尤其是N取值非常大的时候）。

交换的次数比比较的次数少，因为只有在必要时才交换。如果数据是随机的，交换的次数大约是比较次数的一半，即(N^2)/4次交换。（不过也有最糟糕的情况，即数组初始时倒着排序的，那么每次比较都需要交换）。

交换和比较的次数都和N^2成比例。Big O中不计算常量，可以忽略掉2和4，冒泡排序需要的时间是O(N^2)。

每当看到类似上述代码中的嵌套循环，就可以认为该算法运行时间为O(N^2)。外部循环执行N次，每次内部循环都需执行N次（也可能是N除以某个常数）。执行时间接近N^2。
