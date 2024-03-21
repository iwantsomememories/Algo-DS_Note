# 排序

1. 内部排序：将需要处理的数据都加载到内存中进行排序
2. 外部排序：数据量过大，无法全部加载到内存中，需要借助外部存储进行排序

常见内部排序算法：

1. 插入排序：直接插入排序、希尔排序
2. 选择排序：简单选择排序、堆排序
3. 交换排序：冒泡排序、快速排序
4. 归并排序
5. 基数排序



时间复杂度分析：忽略常数项、低次项以及系数

## 冒泡排序

基本思想：对待排序序列从前向后，依次比较相邻元素的值，若发现逆序则交换，共进行n-1次循环。

优化思路：当发现在某趟排序中，未发生元素交换，则终止循环。

时间复杂度——O(n^2)，稳定

```java
private static void bubbleSort(int[] arr) {
    int temp = 0;
    boolean flag = false;
    for (int i = 0; i < arr.length-1; i++) {
        flag = false;
        for (int j = 0; j < arr.length-1-i; j++) {
            if(arr[j] > arr[j+1]){
                flag = true;
                temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
        //System.out.println("第"+(i+1)+"趟排序后的数组");
        //System.out.println(Arrays.toString(arr));
        if(!flag)
            break;
    }
}
```

## 选择排序

基本思想：进行n-1趟排序，第i趟排序从待排序列中找出最小值并与arr[i-1]交换。

时间复杂度——O(n^2)，不稳定，但优于冒泡排序

```java
public static void selectSort(int[] arr){
    int temp = 0;
    int min_index;
    for (int i = 0; i < arr.length-1; i++) {
        min_index = i;
        for (int j = i+1; j < arr.length; j++) {
            if(arr[j] < arr[min_index])
                min_index = j;
        }
        temp = arr[min_index];
        arr[min_index] = arr[i];
        arr[i] = temp;
        //System.out.println("第"+(i+1)+"趟排序后的数组");
        //System.out.println(Arrays.toString(arr));
    }
}
```

## 插入排序

基本思想：把n个待排序元素看作一个有序表和一个无序表，开始时有序表只包含一个元素，无序表中包含（n-1）个元素。排序过程中每趟从无序表中取出一个元素，把它插入到有序表的适当位置，形成新的有序表。

时间复杂度——O(n^2)，稳定，但一般优于选择排序

```java
public static void insertionSort(int[] arr){
    int i,insertIndex,insertval;
    for (i = 1; i < arr.length; i++) {
        insertval = arr[i];
        insertIndex = i-1;
        while (insertIndex >= 0 && insertval < arr[insertIndex]){
            arr[insertIndex+1] = arr[insertIndex];
            insertIndex--;
        }
        arr[insertIndex+1] = insertval;
        //System.out.println("第"+i+"趟排序后的数组");
        //System.out.println(Arrays.toString(arr));
    }
}
```

## 希尔排序

基本思想：插入排序的一种改进版本，把元素按下标的一定增量进行分组排序。按不同的步长进行多次分组排序得到最终结果。

时间复杂度——平均为O(nlogn)，不稳定

```java
public static void shellSort(int[] arr){
    int temp = 0;
    int count = 0;
   for (int gap = arr.length/2; gap > 0; gap/=2){
       for (int i = gap; i < arr.length; i++) {
           int j = i;
           temp = arr[j];
           if(arr[j] < arr[j - gap]){
               while (j - gap >=0 && temp < arr[j-gap]){
                   arr[j] = arr[j-gap];
                   j -= gap;
               }
           }
           arr[j] = temp;
       }
   }
}
```

## 快速排序

基本思想：是对冒泡排序的一种改进，每一趟排序将待排序列分为两个部分，其中一部分所有的数都比另一个部分大。”分而治之“（递归）

时间复杂度——O(nlogn)，不稳定

```java
public static void quickSort(int[] arr, int left, int right){
    if(left >= right)
        return;
    int l = left;
    int r = right;
    int pivot = arr[left];
    int temp;
    while (l < r){
        while (arr[r] >= pivot && r > l) r--;
        while (arr[l] <= pivot && l < r) l++;
        if (l < r) {
            temp = arr[l];
            arr[l] = arr[r];
            arr[r] = temp;
        }
    }
    arr[left] = arr[l];
    arr[l] = pivot;
    quickSort(arr, left, l-1);
    quickSort(arr, l+1, right);
}
```

## 归并排序

基本思想：利用归并的思想实现的排序方法，该算法采用经典的分治策略然后递归求解。（递归）

核心部分为合并有序序列。

时间复杂度——O(nlogn)，稳定，占用额外内存

```java
public static void mergeSort(int[] arr, int left, int right, int[] temp){
    if(left < right){
        int mid = (left+right)/2;
        mergeSort(arr, left, mid, temp);
        mergeSort(arr, mid+1, right, temp);
        merge(arr, left, mid, right, temp);
    }
}

public static void merge(int[] arr, int left, int mid, int right, int[] temp){
    int i = left;
    int j = mid + 1;
    int t = 0;
    while(i <= mid && j <= right){
        if(arr[i] <= arr[j]){
            temp[t++] = arr[i++];
        } else
            temp[t++] = arr[j++];
    }
    while (i <= mid) temp[t++] = arr[i++];
    while (j <= right) temp[t++] = arr[j++];
    t = 0;
    while (left <= right) arr[left++] = temp[t++];
}
```

## 基数排序

基本思想：将所有待排元素数值统一为相同的数位长度d，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后，数列就变成一个有序序列。对于负数可以采取先取绝对值后反转来处理。

时间复杂度——O(n*d)，稳定，占用额外内存

```java
public static void radixSort(int[] arr){
    //得到最大数的位数
    int max = arr[0];
    for (int i = 0; i < arr.length; i++) {
        if(arr[i] > max)
            max = arr[i];
    }
    int maxlength = (max+"").length();

    //定义二位数组，表示10个桶，每个桶即为一个一维数组，大小为arr.length
    int[][] bin = new int[10][arr.length];
    //定义一个一维数组,记录每个桶中实际存放数据个数
    int[] binCount = new int[10];

    for (int k = 0, n = 1; k < maxlength; k++, n *= 10) {
        for (int i = 0; i < arr.length; i++) {
            int digitOfElement = arr[i]/n%10;
            bin[digitOfElement][binCount[digitOfElement]++] = arr[i];
        }
        int index = 0;
        for (int i = 0; i < bin.length; i++) {
            if(binCount[i]!=0){
                for (int j = 0; j < binCount[i]; j++) {
                    arr[index++] = bin[i][j];
                }
            }
            binCount[i] = 0;
        }
        //System.out.println("第"+(k+1)+"轮的结果为："+Arrays.toString(arr));
    }
}
```