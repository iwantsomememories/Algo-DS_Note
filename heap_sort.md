# 堆排序

大顶堆——每个节点的值都大于或等于其左右孩子节点的值，一般用于升序排序

小顶堆——每个节点的值都小于或等于其左右孩子节点的值，一般用于降序排序

两者均可视为数组表示的完全二叉树

## 基本思想

1. 将待排序列构成一个大顶堆
2. 此时对顶根节点为整个序列的最大值
3. 将根节点与末尾元素进行交换，此时末尾为最大值
4. 然后将剩余元素重新构成一个大顶堆。如此反复执行，得到升序序列。

## 代码实现

```java
public static void heapSort(int[] arr){
    //初始化大顶堆
    for (int i = arr.length/2-1; i >= 0; i--) {
        heapCons(arr, i, arr.length);
    }
    //System.out.println(Arrays.toString(arr));
    int temp;
    //交换并调整
    for (int i = arr.length-1; i > 0; i--) {
        temp = arr[i];
        arr[i] = arr[0];
        arr[0] = temp;
        heapCons(arr, 0, i);
    }
    //System.out.println(Arrays.toString(arr));
}

/*
@param arr 待调整序列
@param i 非叶子节点下标
@param length 待调整序列长度
 */
public static void heapCons(int[] arr, int i, int length){
    int temp = arr[i];
    for (int k = i*2+1; k < length; k = k*2+1){
        if(k+1 < length && arr[k] < arr[k+1])
            k++;
        if(arr[k] > temp){
            arr[i] = arr[k];
            i = k;
        } else {
            break;
        }
    }
    arr[i] = temp;
}
```

