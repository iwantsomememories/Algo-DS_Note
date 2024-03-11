# 队列

## 数组模拟队列

循环队列思路分析：

front与rear初始值均为0

1. 入队先存数据，然后rear+1，出队先取数据，然后front-1
2. 判空：rear==front
3. 判满：（rear+1）%maxsize==front

代码实现：

```java
public class circlearrayqueue {
    private int maxsize;//数组最大容量
    private int front;//队列头,指向队首数据
    private int rear;//队列尾，指向队尾数据的后一个位置
    private int[] arr;//用于存放数据

    public circlearrayqueue(int maxsize){
        this.maxsize = maxsize;
        this.arr = new int[maxsize];
        this.front = 0;
        this.rear = 0;
    }

    public boolean isFull(){
        return (rear+1)%maxsize == front;
    }

    public boolean isEmpty(){
        return rear == front;
    }

    public void addQueue(int n){
        if(isFull()){
            System.out.println("队列已满");
            return;
        }
        arr[rear] = n;
        rear = (rear+1)%maxsize;
    }

    public int getQueue(){
        if(isEmpty())
            throw new RuntimeException("队列为空");
        int value = arr[front];
        front = (front+1)%maxsize;
        return value;
    }

    public void showQueue(){
        if(isEmpty()){
            System.out.println("队列为空");
            return;
        }
        //从front开始遍历
        for (int i = 0; i < size(); i++) {
            System.out.printf("arr[%d]=%d\n", (front+i)%maxsize, arr[(front+i)%maxsize]);
        }
    }
    
    public int size(){
        return (rear+maxsize-front)%maxsize;
    }

    public int peekQueue(){
        if(isEmpty()){
            throw new RuntimeException("队列为空");
        }
        return arr[front];
    }
}
```

