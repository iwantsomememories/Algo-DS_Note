# 哈希表

常见实现：数组+链表、数组+二叉树

## 数组+链表实现实例

```java
class HashTab{
    private EmpLinkedList[] empLinkedListArr;
    private int size;

    public HashTab(int size){
        this.empLinkedListArr = new EmpLinkedList[size];
        for (int i = 0; i < empLinkedListArr.length; i++) {
            empLinkedListArr[i] = new EmpLinkedList();
        }
        this.size = size;
    }

    public void add(employer e){
        //根据员工id得到该员工应当添加到哪条链表
        int empLinkedListNo = hashFun(e.getId());
        empLinkedListArr[empLinkedListNo].add(e);
    }

    public void list(){
        for (int i = 0; i < empLinkedListArr.length; i++) {
            empLinkedListArr[i].list(i);
        }
    }

    //编写散列函数，此处使用取模法
    public int hashFun(int id){
        return id%size;
    }

    public void find(int id){
        int No = hashFun(id);
        employer e = empLinkedListArr[No].find(id);
        if(e != null){
            System.out.println("找到该雇员， id = "+id+", name = "+e.getName());
        } else {
            System.out.println("没有找到该雇员");
        }
    }

    public void delete(int id){
        int No = hashFun(id);
        if(empLinkedListArr[No]==null){
            System.out.println("该员工不存在");
            return;
        }
        var d = empLinkedListArr[No].delete(id);
        if(d==null){
            System.out.println("该员工不存在");
        } else
            System.out.println(d+"已被删除");
    }
}

class EmpLinkedList{
    employer head;

    //假定id自增长
    public void add(employer emp){
        if(head == null){
            head = emp;
            return;
        }
        employer t = head;
        while (t.getNext()!=null)
            t = t.getNext();
        t.setNext(emp);
    }

    public void list(int no){
        if(head == null){
            System.out.println("第"+(no+1)+"条链表为空");
            return;
        }
        System.out.print("第"+(no+1)+"条链表的信息为=>");
        employer t = head;
        while (true){
            System.out.println(t);
            if(t.getNext()==null)
                break;
            t = t.getNext();
        }
    }

    //根据id查找雇员
    public employer find(int id){
        //判断链表是否为空
        if(head == null){
            return null;
        }
        employer t = head;
        while (true){
            if(t.getId() == id)
                return t;
            if(t.getNext() == null)
                return null;
            t = t.getNext();
        }
    }

    public employer delete(int id){
        if(head == null){
            return null;
        }
        employer t = head;
        if(head.getId()==id){
            head = t.getNext();
            //System.out.println(t+"已被删除");
            return t;
        }
        while (true){
            if(t.getNext() == null)
                return null;
            if(t.getNext().getId() == id){
                employer p = t.getNext();
                t.setNext(p.getNext());
                //System.out.println(p+"已被删除");
                return p;
            }
            t = t.getNext();
        }
    }
}

class employer {
    private int id;
    private String name;
    private employer next;

    public employer() {
    }

    public employer(int id, String name){
        this.id = id;
        this.name = name;
    }

    public employer(int id, String name, employer next) {
        this.id = id;
        this.name = name;
        this.next = next;
    }

    /**
     * 获取
     * @return id
     */
    public int getId() {
        return id;
    }

    /**
     * 设置
     * @param id
     */
    public void setId(int id) {
        this.id = id;
    }

    /**
     * 获取
     * @return name
     */
    public String getName() {
        return name;
    }

    /**
     * 设置
     * @param name
     */
    public void setName(String name) {
        this.name = name;
    }

    /**
     * 获取
     * @return next
     */
    public employer getNext() {
        return next;
    }

    /**
     * 设置
     * @param next
     */
    public void setNext(employer next) {
        this.next = next;
    }

    public String toString() {
        return "employer{id = " + id + ", name = " + name +  "}";
    }
}
```

