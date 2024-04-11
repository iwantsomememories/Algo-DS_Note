# 二叉排序树

重点——删除结点

1. 若待删除结点为叶子结点，直接删除
2. 若待删除结点为有一棵子树的结点，用子树代替该结点
3. 若待删除结点由两颗子树，首先找到右子树中元素最小的结点（或者左子树中元素最大的结点）并将其删除，然后将值赋给原待删除结点

```java
class BinarySortTree{
    private Node root;
    public void add(Node node){
        if(root == null)
            root = node;
        else
            root.add(node);
    }

    public void infixOrder(){
        if(root != null)
            root.infixOrder();
        else
            System.out.println("二叉树为空");
    }

    public Node find(int key){
        if(root == null)
            return null;
        else
            return root.find(key);
    }

    public Node findParent(int key){
        if(root == null)
            return null;
        else
            return root.findParent(key);
    }

    public int delRightTreeMin(Node node){
        Node target = node;
        while(target.left != null){
            target = target.left;
        }
        delNode(target.value);
        return target.value;
    }

    public void delNode(int value){
        if(root == null)
            return;
        else {
            Node targetNode = this.find(value);
            if(targetNode == null)
                return;
            if(root.left == null && root.right == null){
                root = null;
                return;
            }
            Node parent = this.findParent(value);
            if(targetNode.left == null && targetNode.right == null){
                //叶结点
                if(parent.left.value == targetNode.value) parent.left = null;
                if(parent.right.value == targetNode.value) parent.right = null;
            } else if (targetNode.left != null && targetNode.right != null) {
                //有两棵子树的节点
                int minValue = delRightTreeMin(targetNode.right);
                targetNode.value = minValue;
            } else {
                if (parent != null) {
                    //只有一棵子树的结点
                    if(targetNode.left != null){
                        if(parent.left.value == value){
                            parent.left = targetNode.left;
                        } else {
                            parent.right = targetNode.left;
                        }
                    } else {
                        if(parent.left.value == value){
                            parent.left = targetNode.right;
                        } else {
                            parent.right = targetNode.right;
                        }
                    }
                } else {
                    //待删除结点为根节点
                    if(targetNode.left != null){
                        root = targetNode.left;
                    } else {
                        root = targetNode.right;
                    }
                }
            }
        }
    }
}

class Node{
    int value;
    Node left;
    Node right;

    public Node(int value){
        this.value = value;
    }

    //向树中添加节点
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
    }

    //中序遍历
    public void infixOrder(){
        if(this.left != null){
            this.left.infixOrder();
        }
        System.out.println(this);
        if(this.right != null){
            this.right.infixOrder();
        }
    }

    //查找节点
    public Node find(int key){
        if(this.value == key)
            return this;
        else if (this.value < key) {
            if(this.right == null)
                return null;
            else
                return this.right.find(key);
        } else {
            if(this.left == null)
                return null;
            else
                return this.left.find(key);
        }
    }

    //查找节点的父节点
    public Node findParent(int key){
        if(this.left!=null && this.left.value == key) return this;
        if(this.right!=null && this.right.value == key) return this;
        if(this.value <= key && this.right != null){
            return this.right.findParent(key);
        } else if (this.value > key && this.left != null) {
            return this.left.findParent(key);
        } else
            return null;
    }

    @Override
    public String toString(){
        return "Node[ value = " + this.value + " ]";
    }
}
```