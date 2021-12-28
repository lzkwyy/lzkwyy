# Java 实现排序算法
<h1 align = "right">created by Lzkwyy</h1>
[TOC]
# 排序算法

本文提供有关各种排序算法的**Java**实现
图形化排序可以看看*[VisuAlgo](https://visualgo.net/en/sorting)*

> 需要实现终端输入可以使用如下代码
> ```java
> Scanner input = new Scanner(System.in);
> int len = input.nextInt();
> int[] a = new int[len];
> for (int i = 0;i < len;i++) {
> 	a[i] = input.nextInt();
> }
> ```

---

## 1.冒泡排序
```java
//冒泡排序
static void bubbleSort(int[] arr) {
    int temp;
    for (int i = 0;i < arr.length - 1;i++) {
        for (int j = 0;j < arr.length - 1 - i;j++) {
            if (arr[j] > arr[j + 1]) {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```
___
## 2.选择排序
```java
//选择排序
static void sectionSort(int[] arr) {
    int min;
    int temp;
    for (int i = 0;i < arr.length - 1;i++) {
        min = i;
        for (int j = i + 1;j < arr.length;j++) {
            if (arr[j] < arr[min]) {
                min = j;
            }
        }
        temp = arr[min];
        arr[min] = arr[i];
        arr[i] = temp;
    }
}
```

___
## 3.插入排序
```java
//插入排序
static void insertSort(int[] arr) {
    int next, i, j;
    for (i = 0;i < arr.length - 1;i++) {
        next = arr[i + 1];
        for (j = i;j >= 0;j--) {
            if (next < arr[j]) {
                arr[j + 1] = arr[j];
            }
            else break;
        }
        arr[j + 1] = next;
    }
}
```
___
## 4.归并排序
```java
//归并排序
static void mergeSort(int[] arr) {
    int[] temp = new int[arr.length];
    internalMergeSort(arr, temp, 0, arr.length - 1);
}
static void internalMergeSort(int[] arr, int[] temp, int left, int right) {
    if (left < right) {
        int middle = (left + right) / 2;
        internalMergeSort(arr, temp, left, middle); //先排序好左边
        internalMergeSort(arr, temp, middle + 1, right);    //再排序右边
        mergeSortedArray(arr, temp, left, middle, right);   //合并左右
    }
}
static void mergeSortedArray(int[] arr, int[] temp, int left, int middle, int right) {
    int i = left;
    int j = middle + 1;
    int k = left;
    while (i <= middle && j <= right) {
        if (arr[i] <= arr[j]) { temp[k++] = arr[i++]; }
        else { temp[k++] = arr[j++]; }
    }   //合并开始
    while (i <= middle) { temp[k++] = arr[i++]; }
    while (j <= right) { temp[k++] = arr[j++]; }    //把没合并好的放进去
    while (k > 0) { arr[--k] = temp[k]; }   //排序好的数据放回原数字
}
```
___
## 5.快速排序
```java
//快速排序
static void quickSort(int[] arr) {
    qSort(arr, 0, arr.length - 1);
}
static void qSort(int[] arr, int left, int right) {
    if (left >= right) return;
    int pivot = partition(arr, left, right);
    qSort(arr, left, pivot);
    qSort(arr, pivot + 1, right);
}
static int partition(int[] arr, int left, int right) {
    int pivot = arr[left];
    while (left < right) {
        while (left < right && arr[right] >= pivot) { right--; }
        arr[left] = arr[right];
        while (left < right && arr[left] <= pivot) { left++; }
        arr[right] = arr[left];
    }
    arr[left] = pivot;
    return left;
}
```


___
## 6.希尔排序
```java
//希尔排序
    //Donald Shell增量
static void shellSort(int[] arr) {
    int next, i, j, delta;
    for (delta = arr.length/2;delta >= 1;delta /= 2) {
        for (i = 0;i < arr.length - delta;i++) {
            next = arr[i + delta];
            for (j = i;j >= 0;j -= delta) {
                if (arr[j] > next) {
                    arr[j + delta] = arr[j];
                } else {
                    break;
                }
            }
            arr[j + delta] = next;
        }
    }
}
    //O(n3/2) by Knuth。Knuth增量算法优化
static void shellSort2(int[] arr) {
    int delta = 1;
    while (delta < arr.length/3) {
        delta = delta * 3 + 1;
    }
    int i, j, next;
    for (;delta >= 1;delta /= 3) {
        for (i = 0;i < arr.length - delta;i++) {
            next = arr[i + delta];
            for (j = i;j >= 0;j -= delta) {
                if (arr[j] > next) {
                    arr[j + delta] = arr[j];
                }
                else break;
            }
            arr[j + delta] = next;
        }
    }
}
```
___
## 7.堆排序
```java
//堆排序
class ArrayHeap {
    private int[] arr;
    public ArrayHeap(int[] arr) { this.arr = arr; }
    private int getParentIndex(int child) { return (child - 1) / 2; }
    private int getLeftChildIndex(int parent) { return parent * 2 + 1; }
    public int[] getArr() { return arr; }

    private void swap(int i, int j) {
        int temp;
        temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    private void heapAdjust(int i, int len) {
        int left, right, j;
        left = getLeftChildIndex(i);
        while (left <= len) {
            right = left + 1;
            j = left;
            if (j < len && arr[left] < arr[right]) {
                j++;
            }
            if (arr[i] < arr[j]) {
                swap(i, j);
                i = j;
                left = getLeftChildIndex(i);
            }
            else { break; }
        }
    }
    public void sort() {
        int last = arr.length - 1;
        for (int i = getParentIndex(last);i >= 0;i--) {
            heapAdjust(i, last);
        }
        while (last >= 0) {
            swap(0, last--);
            heapAdjust(0, last);
        }
}
```
}

___
## 8.计数排序
```java
//计数排序 only for integer but not for float
static void countSort(int[] arr, int max, int min) {
    int[] countList = new int[max - min + 1];
    for (int i = 0;i < countList.length;i++) {
        countList[i] = 0;
    }
    for (int i:arr) {
        countList[i - min]++;
    }
//        for (int i = 0;i < countList.length;i++) {
//            System.out.println((i+min) + " count:" + countList[i]);
//        }
    int arrIndex = 0, countListIndex = 0;
    while (countListIndex < countList.length && arrIndex < arr.length) {
        while (countList[countListIndex] > 0 && arrIndex < arr.length) {
            arr[arrIndex] = countListIndex + min;
            arrIndex++;
            countList[countListIndex]--;
        }
        countListIndex++;
    }
}
```
___
## 9.桶排序
```java
//桶排序
static void bucketSort(int[] arr) {
    int maxValue = arr[0], minValue = arr[0];
    for (int i:arr) {
        if (i > maxValue) { maxValue = i; }
        if (i < minValue) { minValue = i; }
    }
    // 未实现智能定数量
    int bucketNum = 5;

    int step = (maxValue - minValue) / bucketNum + 1;
    ArrayList<ArrayList<Integer>> bucketList = new ArrayList<ArrayList<Integer>>();
    for (int i = 0;i < bucketNum;i++) {
        bucketList.add(new ArrayList<Integer>());
    }
    int index;
    for (int i : arr) {
        index = i / step;
        bucketList.get(index).add(i);
    }
    int rawIndex = 0, processedIndex = 0;
    for (ArrayList<Integer> bucketArray : bucketList) {
        Collections.sort(bucketArray);
        for (processedIndex = 0;processedIndex < bucketArray.size();processedIndex++) {
            arr[rawIndex] = bucketArray.get(processedIndex);
            rawIndex++;
        }
    }
```
___
## 10.基数排序
```java
//基数排序
static void radixSort(int[] arr) {
    int maxValue = arr[0];
    for (int i:arr) {
        if(i > maxValue) { maxValue = i; }
    }
    int level = maxValue;
    for (int i = 1;(int) (maxValue / (Math.pow(10, i))) % 10 > 0;i++) {
        ArrayList<ArrayList<Integer>> radixList = new ArrayList<ArrayList<Integer>>();
        for (int j = 0;j < 10;j++) {
            radixList.add(new ArrayList<Integer>());
        }
        for (int num:arr) {
            int index = (int) (num / Math.pow(10,i)) % 10;  //先除以10再取模即可到达取高位效果
            radixList.get(index).add(num);
        }
        System.out.println(i);
        int rawIndex = 0, processedIndex = 0;
        for (ArrayList<Integer> radixArray : radixList) {
            Collections.sort(radixArray);
            for (processedIndex = 0;processedIndex < radixArray.size();processedIndex++) {
                arr[rawIndex] = radixArray.get(processedIndex);
                rawIndex++;
            }
        }
    }
}
```