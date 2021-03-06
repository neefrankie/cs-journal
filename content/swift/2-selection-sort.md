---
title: "选择排序"
date: 2021-06-11T16:47:12+08:00
draft: true
---

选择排序（Selection sort）改进了冒泡排序，把必须的交换次数从O(N²)减少到O(N)。不过，比较的次数仍然是O(N²)。然而，如果数据较大，又需要在内存中移动，导致交换比比较耗时更多，选择排序仍有显著改善。

## 对棒球队员实行选择排序

再以棒球队员为例。使用选择排序，不能再比较相邻的两个队员。需要记住某个队员的身高，可以拿个记事本记下来。找条绛红色的毛巾做标记会很方便。

### 简述

在选择排序中，需要遍历一遍所有队员，挑出（或者选择，这种排序法由此得名）最矮的那名队员，和队列最左侧（0位置）的队员交换。现在，最左侧的队员排好了顺序，不需要再移动了。注意，在这个算法中，排好序的队员在左侧积聚，冒泡排序则在右侧积聚。

下一轮再遍历队列时，从位置1开始，找到最小的，和位置1交换。重复这一过程，直到所有队员排好顺序。

### 详述

从队列左侧开始。在记事本上记下最左侧队员的身高，把红毛巾扔到这名队员脚下的地板上。拿这名队员右侧队员的身高和记事本上的记录比较。如果右侧这名队员矮，则划掉第一位队员的身高，换成第二名队员的身高。移动毛巾，放在这名新的（目前）“最矮的”队员身前。沿着队列依次向右，每一名都和最矮的记录作比较。每次找到一个名更矮的队员，把记事本上的最小记录换成这名队员的身高。数完一轮，毛巾就放在最矮的队员面前。

把这名最矮的队员和队列最左侧的队员交换。现在就给一名队员排好了顺序。执行了N-1次比较但只有一次交换。

下一轮完全重复同样的步骤，不过这次可以完全忽略最左侧那名队员，因为这名队员已经排好了顺序。这一轮算法从位置1开始。接下来每遍历一轮，则左侧多了一名排好顺序的队员。

## Swift实现

创建项目：

```sh
mkdir SelectionSort
swift package init --type executable
```

`main.swift`:

```swift
func selectionSort(arr: [Int]) -> [Int] {
    guard arr.count > 1 else {
        return arr
    }

    var sortedArray = arr

    // 外层循环
    for outerIndex in 0..<(sortedArray.count - 1) {
        // 最小值的位置
        var minIndex = outerIndex

        // 内层循环
        for innerIndex in (outerIndex + 1)..<sortedArray.count {
            // 每一个元素和此前保存的最小值作比较
            if sortedArray[innerIndex] < sortedArray[minIndex] {
                // 发现了新的最小值，替换
                minIndex = innerIndex
            }
        }

        // 内层循环结束，minIndex指向最小值的位置，交换
        sortedArray.swapAt(outerIndex, minIndex)
    }

    return sortedArray
}

print(selectionSort(arr: [77, 99, 44, 55, 22, 88, 11, 00, 66, 33]))
```

执行`swift run`看运行结果。

## 选择排序的效率

选择排序和冒泡排序的比较次数相同：N*(N-1)/2。如果有10个元素，则比较45次。不过，10个元素需要交换的次数少于10。如果有100个元素，需要比较4950次，但交换次数少于100。如果N的值很大，时间主要用于比较，我们可以说选择排序耗时O(N²)，和冒泡排序相同。然而，选择排序无疑更快，因为交换次数少得多。如果N取值较小，选择排序算法实际上可能更快，尤其在交换时间比比较时间耗时更多的情况下。
