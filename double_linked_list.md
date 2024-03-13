# 双向链表

遍历方法和单链表一样，不同之处在于可以向前查找。

代码实现（增删改查）：

```java
//打印链表
public void list(){
    if(head.next == null){
        System.out.println("链表为空");
        return;
    }
    HeroNode2 temp = head.next;
    while(temp != null){
        System.out.println(temp);
        temp = temp.next;
    }
    System.out.println();
}

//添加节点
//不考虑编号顺序
//1.找到链表的尾节点
//2.将尾节点的next域指向新节点
public void add(HeroNode2 heroNode){
    HeroNode2 temp = head;
    while(temp.next != null){
        temp = temp.next;
    }
    temp.next = heroNode;
    heroNode.prev = temp;
}

public void addByOrder(HeroNode2 h){
    HeroNode2 temp = head;
    int n = h.no;
    boolean flag = false;
    while (true){
        if(temp.next == null)
            break;
        if(temp.next.no > n)
            break;
        if(temp.next.no == n){
            flag = true;
            break;
        }
        temp = temp.next;
    }
    if(flag){
        System.out.println("编号为"+n+"的节点已存在");
    } else{
        h.next = temp.next;
        if(temp.next != null)
            temp.next.prev = h;
        temp.next = h;
        h.prev = temp;
    }
}

//修改节点信息，根据编号修改，不能修改编号
public void update(HeroNode2 heroNode){
    if(head.next == null)
        System.out.println("链表为空");
    int n = heroNode.no;
    HeroNode2 temp = head;
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

//删除节点
//1.找到待删除节点的前一个节点temp
//2.令temp.next = temp.next.next
public void delete(int no){
    HeroNode2 temp = head.next;
    boolean flag = false;
    while (true){
        if(temp == null)
            break;
        if(temp.no == no){
            flag = true;
            break;
        }
        temp = temp.next;
    }
    if(flag){
        temp.prev.next = temp.next;
        if(temp.next!=null)
            temp.next.prev = temp.prev;
    } else {
        System.out.println("不存在编号为"+no+"的节点");
    }
}

class HeroNode2{
    public int no;
    public String name;
    public String nickname;
    public HeroNode2 next; //指向后一个节点
    public HeroNode2 prev; //指向前一个节点

    public HeroNode2(int no, String name, String nickname){
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