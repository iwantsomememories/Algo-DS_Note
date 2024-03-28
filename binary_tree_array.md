# 二叉树的顺序存储

特点：顺序存储二叉树通常只考虑完全二叉树

## 前序遍历

```java
//前序遍历
public void preOrder(int index){
    //如果数组为空，或者arr.length=0
    if(array == null || array.length == 0){
        System.out.println("数组为空");
        return;
    }
    //输出当前元素
    System.out.println(array[index]);
    //向左递归遍历
    if((index*2+1)<array.length){
        preOrder(2*index+1);
    }
    //向右递归遍历
    if((index*2+2)<array.length){
        preOrder(2*index+2);
    }
    return;
}
```

## 中序遍历

```java
//中序遍历
public void infixOrder(int index){
    //如果数组为空，或者arr.length=0
    if(array == null || array.length == 0){
        System.out.println("数组为空");
        return;
    }
    //向左递归遍历
    if((index*2+1)<array.length){
        infixOrder(2*index+1);
    }
    //输出当前元素
    System.out.println(array[index]);
    //向右递归遍历
    if((index*2+2)<array.length){
        infixOrder(2*index+2);
    }
    return;
}
```

## 后序遍历

```java
//后序遍历
public void postOrder(int index){
    //如果数组为空，或者arr.length=0
    if(array == null || array.length == 0){
        System.out.println("数组为空");
        return;
    }
    //向左递归遍历
    if((index*2+1)<array.length){
        postOrder(2*index+1);
    }
    //向右递归遍历
    if((index*2+2)<array.length){
        postOrder(2*index+2);
    }
    //输出当前元素
    System.out.println(array[index]);
    return;
}
```