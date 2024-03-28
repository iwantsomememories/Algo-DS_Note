# 二叉树

## 前序遍历及查找

```java
public void preOrder(){
    System.out.println(this);
    if(this.left != null){
        this.left.preOrder();
    }
    if(this.right != null){
        this.right.preOrder();
    }
}
```

```
public HeroNode preOrderSearch(int no){
    System.out.println("进入前序遍历查找~~");
    if(this.getNo() == no){
        return this;
    }
    HeroNode res = null;
    if(this.getLeft() != null){
        res = this.getLeft().preOrderSearch(no);
    }
    if(res != null){
        return res;
    } else if (this.getRight()!=null) {
        res = this.getRight().preOrderSearch(no);
    }
    return res;
}
```

## 中序遍历及查找

```java
public void infixOrder(){
    if(this.left != null){
        this.left.infixOrder();
    }
    System.out.println(this);
    if(this.right != null){
        this.right.infixOrder();
    }
}
```

```java
public HeroNode infixOrderSearch(int no){
    HeroNode res = null;
    if(this.getLeft() != null){
        res = this.getLeft().infixOrderSearch(no);
    }
    if(res != null)
        return res;
    System.out.println("中序遍历查找~~");
    if(this.getNo() == no){
        return this;
    }
    if (this.getRight()!=null) {
        res = this.getRight().infixOrderSearch(no);
    }
    return res;
}
```

## 后序遍历及查找

```java
public void postOrder(){
    if(this.left != null){
        this.left.postOrder();
    }
    if(this.right != null){
        this.right.postOrder();
    }
    System.out.println(this);
}
```

```java
public HeroNode postOrderSearch(int no){
    HeroNode res = null;
    if(this.getLeft() != null){
        res = this.getLeft().postOrderSearch(no);
    }
    if(res != null)
        return res;
    if (this.getRight()!=null) {
        res = this.getRight().postOrderSearch(no);
    }
    if(res != null)
        return res;
    System.out.println("后序遍历查找~~");
    if(this.getNo() == no){
        return this;
    }
    return res;
}
```

## 删除节点

```java
//删除节点为叶子节点，直接删除
//删除节点非叶子节点，删除该子树
public void delNode(int no){
    if(this.left != null && this.left.getNo() == no){
        this.setLeft(null);
        return;
    }
    if(this.right != null && this.right.getNo() == no){
        this.setRight(null);
        return;
    }
    if(this.left != null){
        this.left.delNode(no);
    }
    if(this.right != null){
        this.right.delNode(no);
    }
}
```