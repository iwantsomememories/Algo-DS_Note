# 查找

## 顺序查找

```java
public static int seqSearch(int[] arr, int value){
    //线性查找是逐一比对
    for (int i = 0; i < arr.length; i++) {
        if(arr[i] == value){
            return i;
        }
    }
    return -1;
}
```

## 二分查找

待查序列应为有序序列，递归查找

```java
public static int biSearch(int[] arr, int left, int right, int value){
    if(left > right)
        return -1;

    int mid = (left+right)/2;
    int midVal = arr[mid];

    if(value > midVal){
        return biSearch(arr, mid+1, right, value);
    } else if (value < midVal) {
        return biSearch(arr, left, mid-1, value);
    } else
        return mid;
}
```

## 插值查找

类似于二分查找，不同的是每次从**自适应mid**处开始查找

求mid索引公式：
$$
mid = low+\frac{key-a[low]}{a[high]-a[low]} (high-low)
$$
当待查序列数据量较大，且数据分布比较均匀时，插值查找效率较高

```java
public static int insertSearch(int[] arr, int left, int right, int value){
    //防止溢出
    if(left > right || value < arr[0] || value > arr[arr.length-1])
        return -1;

    int mid = left+(right-left)*(value-arr[left])/(arr[right]-arr[left]);
    int midVal = arr[mid];

    if(value > midVal){
        return insertSearch(arr, mid+1, right, value);
    } else if (value < midVal) {
        return insertSearch(arr, left, mid-1, value);
    } else
        return mid;
}
```

## 裴波那契查找算法

原理与前两种类似，仅仅是改变了mid的计算方法，mid位于黄金分割点附近，具体计算如下：
$$
mid=low+F(k-1)+1
$$
其中$F(k)$满足裴波那契数列性质

```java
//非递归
public static int fibonacciSearch(int[] arr, int key){
    int low = 0;
    int high = arr.length-1;
    int k = 0;//表示斐波那契分割数值的下标
    int mid = 0;
    int[] f = fib();
    //找到合适的k
    while (high > f[k] - 1){
        k++;
    }
    //因为f[k]值可能大于数组长度，因此需要使用Arrays类，构造一个新数组
    //不足的部分会用0填充
    int[] temp = Arrays.copyOf(arr, f[k]);
    //实际上需要使用a数组最后的数填充temp
    for (int i = high+1; i < temp.length; i++) {
        temp[i] = temp[high];
    }
    //使用while来循环查找key
    while (low <= high){
        mid = low + f[k-1] - 1;
        if(key < temp[mid]){
            high = mid - 1;
            k--;
            //f[k] = f[k-1]+f[k-2]
            //前半部为f[k-1]部分
        } else if (key > temp[mid]) {
            low = mid + 1;
            k -= 2;
            //后半部为f[k-2]部分
        } else {
            if(mid<=high)
                return mid;
            else
                return high;
        }
    }
    return -1;
}

//生成斐波那契数列
public static int[] fib(){
    int[] f = new int[maxsize];
    f[0] = 1;
    f[1] = 1;
    for (int i = 2; i < maxsize; i++) {
        f[i] = f[i-1] + f[i-2];
    }
    return f;
}
```

