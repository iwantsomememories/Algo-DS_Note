# 单向环形链表

环形链表代码实现：

```java
class Single_Circle_LinkedList {
    //创建一个first节点，当前无编号
    private boyNode first = new boyNode(-1);

    //添加节点
    public void addBoy(int nums){
        if(nums < 1){
            System.out.println("nums不能小于1");
            return;
        }
        boyNode curboy = null;
        for (int i = 1; i <= nums; i++) {
            boyNode boy = new boyNode(i);
            if(i == 1){
                first = boy;
                first.setNext(first);
                curboy = first;
            } else {
                curboy.setNext(boy);
                boy.setNext(first);
                curboy = boy;
            }
        }
    }

    //遍历环形链表
    public void show(){
        if(first.getNo() == -1){
            System.out.println("链表为空");
            return;
        }
        boyNode curBoy = first;
        while (true){
            System.out.println(curBoy);
            if(curBoy.getNext() == first)
                break;
            curBoy = curBoy.getNext();
        }
    }

}

class boyNode{
    private int no;
    private boyNode next;
    public boyNode(int num){
        this.no = num;
    }

    public boyNode() {
    }

    public boyNode(int no, boyNode next) {
        this.no = no;
        this.next = next;
    }

    /**
     * 获取
     * @return no
     */
    public int getNo() {
        return no;
    }

    /**
     * 设置
     * @param no
     */
    public void setNo(int no) {
        this.no = no;
    }

    /**
     * 获取
     * @return next
     */
    public boyNode getNext() {
        return next;
    }

    /**
     * 设置
     * @param next
     */
    public void setNext(boyNode next) {
        this.next = next;
    }

    public String toString() {
        return "boyNode{no = " + no + "}";
    }
}
```

利用环形链表求解Joseph问题：

```java1
public void countBoy(int startNo, int CountNum, int nums){
    if(first.getNo() == -1 || startNo < 1 || startNo > nums){
        System.out.println("参数输入有误");
        return;
    }
    boyNode helper = first;
    while(true){
        if(helper.getNext() == first)
            break;
        helper = helper.getNext();
    }
    for (int i = 0; i < startNo - 1; i++) {
        first = first.getNext();
        helper = helper.getNext();
    }
    while (true){
        if(helper == first)
            break;
        for (int i = 0; i < CountNum-1; i++) {
            first = first.getNext();
            helper = helper.getNext();
        }
        System.out.println(first+"出圈");
        first = first.getNext();
        helper.setNext(first);
    }
    System.out.println("最后留在圈中的BOY"+first);
}
```