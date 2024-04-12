# 平衡二叉树

定义：任意结点左右两棵子树的高度差不超过1的二叉搜索树

插入平衡二叉树时，要根据高度实时进行调整（旋转）

代码实例：

```java
public int leftHeight(){
    if(left == null)
        return 0;
    return left.height();
}

public int rightHeight(){
    if(right == null)
        return 0;
    return right.height();
}

//返回以该结点为根节点的树的高度
public int height(){
    return Math.max(left == null ? 0 : left.height(), right == null ? 0 : right.height()) + 1;
}

//左旋转
private void leftRotate(){
    Node newNode = new Node(this.value);
    newNode.left = this.left;
    newNode.right = this.right.left;
    this.value = this.right.value;
    this.right = this.right.right;
    this.left = newNode;
}

//右旋转
public void rightRotate(){
    Node newNode = new Node(this.value);
    newNode.right = this.right;
    newNode.left = this.left.right;
    this.value = this.left.value;
    this.left = this.left.left;
    this.right = newNode;
}

public void add(Node node){
    if(node == null){
        return;
    }

    if(node.value < this.value){
        if(this.left == null)
            this.left = node;
        else
            this.left.add(node);
    } else {
        if(this.right == null)
            this.right = node;
        else
            this.right.add(node);
    }

    if(rightHeight() - leftHeight() > 1){
        //若（右子树的高度-左子树的高度）> 1
        if(this.right != null && this.right.leftHeight() < this.right.rightHeight() ){
            //若当前结点的右子树的右子树的高度大于它的左子树的高度，直接左旋
            leftRotate();
        } else {
            //若当前结点的右子树的右子树的高度小于等于它的左子树的高度，先对其右子树进行右旋，再对当前结点进行左旋
            this.right.rightRotate();
            leftRotate();
        }
        return;
    }
    if(leftHeight() - rightHeight() > 1){
        //若（左子树的高度-右子树的高度）> 1
        if(this.left != null && this.right.leftHeight() > this.right.rightHeight()){
            //若当前结点的左子树的右子树的高度小于它的左子树的高度，直接右旋
            rightRotate();
        } else {
            //若当前结点的左子树的右子树的高度大于等于它的左子树的高度，先对其左子树进行左旋，再对当前结点进行右旋
            this.left.leftRotate();
            rightRotate();
        }
    }
}
```