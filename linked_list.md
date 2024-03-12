# 单链表

以节点的方式来存储，不一定连续存储，分为带头节点和不带头节点两类。

代码实现（增删改查）：

```java
class SingleLinkedList{
    //初始化头节点
    private HeroNode head = new HeroNode(0, "", "");

    //添加节点
    //不考虑编号顺序
    //1.找到链表的尾节点
    //2.将尾节点的next域指向新节点
    public void add(HeroNode heroNode){
        HeroNode temp = head;
        while(temp.next != null){
            temp = temp.next;
        }
        temp.next = heroNode;
    }


    //按顺序插入节点
    //1.首先找到新添加的节点的位置，通过辅助变量temp
    //2.新的节点.next = temp.next
    //3.temp.next = 新的节点
    public void addByOrder(HeroNode heroNode){
        HeroNode temp = head;
        boolean flag = false;
        int n = heroNode.no;
        while(true){
            if(temp.next == null){
                break;
            }
            if(temp.next.no > n){ //位置找到
                break;
            } else if (temp.next.no == n) {
                flag = true; //编号存在
                break;
            }
            temp = temp.next;
        }
        if(flag){
            System.out.println("编号为"+heroNode.no+"的英雄已存在，无法插入");
        } else {
            heroNode.next = temp.next;
            temp.next = heroNode;
        }
    }

    //修改节点信息，根据编号修改，不能修改编号
    public void update(HeroNode heroNode){
        if(head.next == null)
            System.out.println("链表为空");
        int n = heroNode.no;
        HeroNode temp = head;
        boolean flag = false;
        while (true){
            if(temp == null)
                break;
            if(temp.no == n){
                flag = true;
                break;
            }
            temp = temp.next;
        }
        if(flag){
            temp.name = heroNode.name;
            temp.nickname = heroNode.nickname;
        } else
            System.out.println("不存在编号为"+n+"的节点");
    }

    //删除节点，根据编号删除
    //1.找到待删除节点的前一个节点temp
    //2.令temp.next = temp.next.next
    public void delete(int no){
        HeroNode temp = head;
        boolean flag = false;
        while (true){
            if(temp.next == null)
                break;
            if(temp.next.no == no){
                flag = true;
                break;
            }
            temp = temp.next;
        }
        if(flag){
            temp.next = temp.next.next;
        } else {
            System.out.println("不存在编号为"+no+"的节点");
        }
    }

    //打印单链表
    public void list(){
        if(head.next == null){
            System.out.println("链表为空");
            return;
        }
        HeroNode temp = head.next;
        while(temp != null){
            System.out.println(temp);
            temp = temp.next;
        }
    }

}

//节点定义
class HeroNode{
    public int no;
    public String name;
    public String nickname;
    public HeroNode next;

    public HeroNode(int no, String name, String nickname){
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }

    @Override
    public String toString() {
        return "HeroNode{no = " + no + ", name = " + name + ", nickname = " + nickname + "}";
    }
}
```

相关算法题：

```java
//查找单链表中的倒数第k个节点
//方法形参为head节点以及index
//思路：先得到链表总长度length，然后从第一个节点开始遍历（length-index）个节点
public static HeroNode findLastIndexNode(HeroNode head, int index){
    int length = getLength(head);
    if(length < index || index <= 0){
        System.out.println("找不到倒数第"+index+"个节点");
        return null;
    }
    HeroNode tmp = head.next;
    for (int i = 0; i < length-index; i++) {
        tmp = tmp.next;
    }
    return tmp;
}

//单链表的反转
//除head节点外其余节点均反转
//思路：1.先定义一个新节点，作为新链表的头节点
//2.从头到尾遍历原来的链表，每遍历一个节点，就将其取出并放在新链表的最前端
//3.令原链表的头节点指向新链表的头节点
public static void reverseLinkedList(HeroNode head){
    if(head.next==null || head.next.next==null)
        return;
    HeroNode reverseNode = new HeroNode(0, "", "");
    HeroNode cur = head.next;
    HeroNode next = null; //当前节点cur的下一个节点
    while (cur != null){
        next = cur.next;
        cur.next = reverseNode.next;
        reverseNode.next = cur;
        cur = next;
    }
    head.next = reverseNode.next;
}

//合并两个有序链表
public static void mergeLinkedList(HeroNode head1, HeroNode head2){
    if( head1.next == null || head2.next == null){
        if(head1.next != null)
            head2.next = head1.next;
        else if (head2.next != null)
            head1.next = head2.next;
        else
            return;
    }
    HeroNode newHead = new HeroNode(0, "", "");
    HeroNode cur = newHead;
    HeroNode cur1 = head1.next;
    HeroNode cur2 = head2.next;
    HeroNode next1 = null;
    HeroNode next2 = null;
    while (cur1!=null&&cur2!=null){
        next1 = cur1.next;
        next2 = cur2.next;
        if(cur1.no < cur2.no){
            cur.next = cur1;
            cur1 = next1;
            cur = cur.next;
        } else{
            cur.next = cur2;
            cur2 = next2;
            cur = cur.next;
        }
    }
    if(cur1!=null)
        cur.next = cur1;
    else
        cur.next = cur2;
    head1.next = newHead.next;
    head2.next = newHead.next;
}

//从尾到头打印单链表
//思路：
//方法1：先将单链表进行反转，然后再遍历(破坏了单链表的结构)
//方法2：可以利用栈数据结构，将各个节点压入栈中，然后利用栈的先进后出的特点打印
public static void reversePrint(HeroNode head){
    if(head.next == null)
        return;
    //创建一个栈
    Stack<HeroNode> s = new Stack<>();
    HeroNode cur = head.next;
    //将链表的所有节点压入栈中
    while (cur != null){
        s.push(cur);
        cur = cur.next;
    }
    //将栈中的节点进行打印
    while (s.size()>0){
        System.out.println(s.pop());
    }

}
```