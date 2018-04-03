# 2.1 Elementary Sorts

本节介绍两个基础排序算法，以及其中一个算法的变型。

学习这些简单算法的原因如下：

1. 了解排序涉及的术语，以及排序的基本机制；
2. 某些应用中，简单算法比精巧算法更加有效；
3. 简单算法可以用于精巧算法中，以提升其效率；

## 游戏规则

假设有一个数组，其中每个元素都有一个 key，排序的目的是对数组元素重新排列，以使其 key 满足某种预定义的顺序规则（一般数字序 or 字母序）。不同应用中，key 的特征也不同，Java 使用 `Comparable` 接口对 key 进行抽象。

下面的 `Example` 使我们实现排序算法的一般套路，`sort` 实现具体的排序算法，实现过程中仅仅使用 `less` 和 `exch` 对数组元素进行操作：

```Java
public class Exmaple
{
    public static void sort(Comparable[] a) {
        // TODO 排序算法实现
    }

    private static boolean less(Comparable v, Comparable w)
    {
        return v.compareTo(w) < 0;
    }

    private static void exch(Comparable[] a, int i, int j)
    {
        Comparable t = a[i];
        a[i] = a[j];
        a[j] = t;
    }

    private static void show(Comparable[] a)
    {
        for (Comparable x : a)
        {
            System.out.print(x + " ");
        }
        System.out.println();
    }

    public static boolean isSorted(Comparable[] a)
    {
        for (int i = 1; i < a.length; i++)
        {
            if (less(a[i], a[i-1]))
                return false;
        }

        return true;
    }

    // test
    public static void main(String[] args)
    {
        // 从标准输入读取字符串，对其排序、打印
        String[] a;

        sort(a);

        assert isSorted(a);

        show(a);
    }

}
```

### Certification

通过 `assert isSorted(a)` 确认排序是否有效。

### Runing Time

#### Sorting Cost Model

>When studying sorting algorithms, we count **compares** and **exchanges**. For algorithms that does not use exchanges, we count **array accesses**.


### Extra Memory

排序算法使用的内存大小经常与其运行时间一样重要，实际上排序算法可以分为两类：

1. 原地排序（sort in place），除函数调用栈和常量级的实例变量外，不需要额外内存；
2. 需要对原数组进行拷贝；

### Types of Data

我们实现的排序算法可用于任何实现 `Comparable` 接口的类，Java 中的 `Integer` `String` `File` `Url` 等非常多类都实现了 `Comparable`，这使得我们的排序算法非常实用。

若要对自定义类型排序，只要让它实现 `Comparable` 接口即可，`Comparable.compareTo` 方法定义了该类型的 natural order，按照惯例，`v.compareTo(w)` 应该：

1. 若 v < w，则 -1
2. 若 v = w，则  0
3. 若 v > w，则 +1

`compareTo` 必须实现 total order:

1. Reflexive
  + for all v, v = v
2. Antisynmmetric
  + for all v and w, if v < w, then w > v and if v = w, then w = v
3. Transitive
  + for all v, w and x, if v <= w and w <= x, then v <= x

## Selection Sort

最简单的排序算法步骤如下：

1. find the **smallest** item in the array, and **exchange** it with the first entry(itself if the first entry is already the smallest)
2. find the **next smallest** item and exchange it with the second entry
3. continue, until the entire array is sorted

该算法不断地 selecting the smallest remaining item，因此被称为 Selection Sort。

其实现如下：

```Java
public class Selection {
    public static void sort(Comparable[] a) {
        int N = a.length;

        for (int i = 0; i < N; i++) {
            /**
             * 将 a[i] 与 a[i->N] 中的最小值交换
             */
            int min = i;
            for (int j = i; j < N; j++) {
                if (less(a[j], a[min])) min = j;
            }

            exch(a, i, min);
        }
    }
}
```

* 内层循环：查找 `a[i] -> a[N]` 中的最小值；
* 外层循环：将 `a[i]` 与 `a[min]` 交换，交换后 `a[min]` 到达其 **最终位置**；

因此选择排序中：

* exchange 数量为 N
* compare 数量为 N^2/2

选择排序易于理解、实现，且具备如下显著特点：

1. Running time is **insensitive** to input.
  * 每次最小值查找相互独立，无法给下次查找提供有用信息，从而导致以下 3 种场景 **耗时相同**：
    + 随机顺序数组
    + **已排序** 数组
    + **所有 key 相等** 的数组
2. Data movement is minimal.
  * 只需要 N 次交换
  
## Insertion Sort







