# 线索化二叉树

包含n个节点的二叉链表中共有（n+1）个空指针域。利用空指针域指向某种遍历次序下的前驱和后继节点，称为线索。

1. 包含n个节点的二叉链表中共有（n+1）个空指针域。利用空指针域指向某种遍历次序下的前驱和后继节点，称为线索。
2. 线索化后，节点的左指针域可能指向左子树或前驱节点，右指针域可能指向右子树或后继节点

## 前序线索化

```java
public void threadedNodesPre(HeroNode node){
    if(node == null)
        return;
    //先线索化当前节点
    //处理当前节点的前驱节点
    if(node.getLeft()==null){
        //当前节点的左指针指向前驱节点
        node.setLeft(pre);
        node.setLeftType(1);
    }
    //处理后继节点
    if(pre!=null&&pre.getRight()==null){
        pre.setRight(node);
        pre.setRightType(1);
    }
    pre = node;
    //线索化左子树
    if(node.getLeftType()==0)
        threadedNodesPre(node.getLeft());
    //线索化右子树
    if(node.getRightType()==0)
        threadedNodesPre(node.getRight());
}
```

## 前序线索化的遍历

```java
//遍历前序线索化二叉树的方法
public void preOrder(){
    //存储遍历的当前节点
    HeroNode node = root;
    while (node != null){
        while (node.getLeftType()==0){
            //左指针不是线索，边打印边左移
            System.out.println(node);
            node = node.getLeft();
        }
        System.out.println(node);
        //此时左子树不存在，则右指针若非空，无论是否为线索都指向其后继
        node = node.getRight();
    }
}
```

## 中序线索化

```java
//编写对二叉树进行中序线索化的方法
public void threadedNodes(HeroNode node){
    if(node == null)
        return;
    //先线索化左子树
    threadedNodes(node.getLeft());
    //再线索化当前节点
    //处理当前节点的前驱节点
    if(node.getLeft()==null){
        //当前节点的左指针指向前驱节点
        node.setLeft(pre);
        node.setLeftType(1);
    }
    //处理后继节点
    if(pre!=null&&pre.getRight()==null){
        pre.setRight(node);
        pre.setRightType(1);
    }
    pre = node;
    //最后线索化右子树
    threadedNodes(node.getRight());
}
```

## 中序线索化二叉树的遍历

```java
//遍历中序线索化二叉树的方法
public void infixOrder(){
    //存储遍历的当前节点
    HeroNode node = root;
    while (node != null){
        //找到中序遍历情况下以node为根节点的子树应输出的首个节点
        while (node.getLeftType() == 0){
            node = node.getLeft();
        }
        System.out.println(node);
        //若当前节点的右指针指向的是后继节点，则一直输出
        while (node.getRightType()==1){
            node = node.getRight();
            System.out.println(node);
        }
        node = node.getRight();
    }
}
```

## 后续线索化

```java
//编写对二叉树进行后序线索化的方法
public void threadedNodesPost(HeroNode node){
    if(node == null)
        return;
    //先线索化左子树
    threadedNodesPost(node.getLeft());
    //然后线索化右子树
    threadedNodesPost(node.getRight());
    //最后线索化当前节点
    //处理当前节点的前驱节点
    if(node.getLeft()==null){
        //当前节点的左指针指向前驱节点
        node.setLeft(pre);
        node.setLeftType(1);
    }
    //处理后继节点
    if(pre!=null&&pre.getRight()==null){
        pre.setRight(node);
        pre.setRightType(1);
    }
    pre = node;

}
```

## 后续线索化的遍历（难点）

```java
public void postOrder(){
    //存储遍历的当前节点
    HeroNode node = root;
    //寻找开始节点
    while (node != null && node.getLeftType()==0)
        node = node.getLeft();
    HeroNode preNode = null;
    while (node != null){
        //右节点是线索
        if(node.getRightType()==1){
            System.out.println(node);
            preNode = node;
            node = node.getRight();
        } else {
            //如果上个处理的节点是当前节点的右子节点
            if(node.getRight() == preNode){
                System.out.println(node);
                if(node == root)
                    return;
                preNode = node;
                node = node.getParent();
            } else {
                node = node.getRight();
                while (node != null && node.getLeftType() == 0)
                    node = node.getLeft();
            }
        }

    }
}
```