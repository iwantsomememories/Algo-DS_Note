# 栈

## 数组模拟栈

```java
class ArrayStack{
    private int maxSize;//栈的大小
    private int[] stack;//数组
    private int top = -1;//栈顶,指向栈顶数据

    public ArrayStack(int maxSize){
        this.maxSize = maxSize;
        stack = new int[this.maxSize];
    }

    //判断栈满
    public boolean isFull(){
        return top == maxSize - 1;
    }

    public boolean isEmpty(){
        return top == -1;
    }

    public void push(int value){
        if(isFull()){
            System.out.println("栈满");
            return;
        }
        top++;
        stack[top] = value;
    }

    public int pop(){
        if(isEmpty()) {
            throw new RuntimeException("栈空");
        }
        int value = stack[top];
        top--;
        return value;
    }

    //遍历栈，从栈顶开始显示数据
    public void list(){
        if(isEmpty()){
            System.out.println("栈空，没有数据~");
        }
        for (int i = top; i >= 0; i--) {
            System.out.printf("stack[%d]=%d\n", i, stack[i]);
        }
    }
}
```

## 链表模拟栈

```java
class linkedlistStack{
    StackNode top;
    public boolean isEmpty(){
        return top == null;
    }

    public void push(int value){
        if(isEmpty()){
            top = new StackNode(value);
            return;
        }
        StackNode stackNode = new StackNode(value);
        stackNode.setNext(top);
        top = stackNode;
    }

    public int pop(){
        if(isEmpty())
            throw new RuntimeException("栈空");
        StackNode temp = top;
        top = top.getNext();
        return temp.getValue();
    }

    public void show(){
        if(isEmpty()){
            System.out.println("栈空");
            return;
        }
        StackNode temp = top;
        while(temp!=null){
            System.out.println(temp.getValue());
            temp = temp.getNext();
        }
    }
}

class StackNode{
    int value;
    StackNode next;


    public StackNode(int value){
        this.value = value;
    }
    public StackNode() {
    }

    public StackNode(int value, StackNode next) {
        this.value = value;
        this.next = next;
    }

    /**
     * 获取
     * @return value
     */
    public int getValue() {
        return value;
    }

    /**
     * 设置
     * @param value
     */
    public void setValue(int value) {
        this.value = value;
    }

    /**
     * 获取
     * @return next
     */
    public StackNode getNext() {
        return next;
    }

    /**
     * 设置
     * @param next
     */
    public void setNext(StackNode next) {
        this.next = next;
    }

    public String toString() {
        return "StackNode{value = " + value + ", next = " + next + "}";
    }
}
```

## 计算器

中缀表达式的计算，创建两个栈：数栈、符号栈

表达式扫描过程：

1. 若为数字，直接入数栈

2. 若为符号
   1. 若当前符号栈为空或者栈顶元素为左括号，直接入栈
   2. 否则
      1. 若当前操作符的优先级小于或者等于栈顶的操作符，从数栈中pop出两个数，再从符号栈中pop出一个符号，进行运算，将结果入数栈，最后将当前操作符入符号栈。
      2. 否则直接入符号栈

3. 若为左括号

   直接入符号栈

4. 若为右括号

   依次从数栈和符号栈中弹出数与符号进行运算，并将运算结果再次压入数栈，反复多次直到将左括号弹出

扫描完毕后，依次从数栈和符号栈中弹出数与符号进行运算，并将运算结果再次压入数栈，反复多次直到数栈中只有一个数据。

```java
class Calculator{
    private ArrayStack numStack = new ArrayStack(10);
    private ArrayStack chaStack = new ArrayStack(10);

    public int calculate(String sentence){
        int length = sentence.length();
        int index = 0;
        int num1 = 0;
        int num2 = 0;
        int res = 0;
        int oper = 0;
        char ch = ' ';
        while (true){
            ch = sentence.charAt(index);
            //System.out.println(ch);
            //判断c为符号还是数字
            if(ArrayStack.isOper(ch)){
                if(chaStack.isEmpty() || chaStack.peek() == '(')
                    chaStack.push(ch);
                else {
                    if(ArrayStack.priority(ch) <= ArrayStack.priority(chaStack.peek())){
                        oper =  chaStack.pop();
                        num2 = numStack.pop();
                        num1 = numStack.pop();
                        res = ArrayStack.cal(num1, num2, oper);
                        numStack.push(res);
                        //System.out.println(num1+", "+num2+", "+(char)oper+","+res);
                        //ch重新与栈中符号比较，重复上述过程
                        continue;
                    } else {
                        chaStack.push(ch);
                    }
                }
            } else if (ch == '(') {
                chaStack.push(ch);
            } else if (ch == ')') {
                while (true){
                    oper = chaStack.pop();
                    if(oper == '(')
                        break;
                    num2 = numStack.pop();
                    num1 = numStack.pop();
                    res = ArrayStack.cal(num1, num2, oper);
                    numStack.push(res);
                }
            } else {
                //处理多位数
                StringBuilder sb = new StringBuilder();
                while (true) {
                    sb.append(sentence.charAt(index));
                    if((index+1)>=length || ArrayStack.isOper(sentence.charAt(index+1)) || sentence.charAt(index+1) == '(' || sentence.charAt(index+1) == ')'){
                        break;
                    }else{
                        index++;
                    }
                }
                numStack.push(Integer.parseInt(sb.toString()));
            }
            index++;
            if(index >= length)
                break;
        }
        while (!chaStack.isEmpty()){
            oper = chaStack.pop();
            num2 = numStack.pop();
            num1 = numStack.pop();
            res = ArrayStack.cal(num1, num2, oper);
            //System.out.println(num1+", "+num2+", "+(char)oper+","+res);
            numStack.push(res);
        }
        return numStack.pop();
    }
}
```

## 前缀、中缀、后缀表达式

前缀表达式求值：

从右至左扫描表达式，若遇到数字则入数字栈，遇到符号则弹出两个数字进行运算，并将结果压入栈中。重复该步骤直到得到最终结果。

中缀表达式求值：常见的运算表达式，求值参考综合计算器。

后缀表达式求值：与前缀表达式相似，只是运算符位于操作数之后。从左至右扫描表达式，遇到数字时，则将数字压入栈中，遇到运算符时，弹出两个数字进行运算，并将结果压入栈中。重复以上过程直到表达式最右端。

中缀表达式转后缀表达式：

1. 初始化两个栈：运算符栈s1和存储中间结果的栈s2
2. 从左至右扫描中缀表达式
3. 遇到数字则压入s2
4. 遇到操作符
   1. 若s1为空或栈顶元素为（，则将操作符压入s1
   2. 否则，若其优先级高于栈顶操作符，则将操作符压入s1
   3. 否则，将s1栈顶的操作符弹出并压入s2，并将当前操作符与栈顶元素进行新一轮比较
5. 遇到括号
   1. 若为（，直接压入s1
   2. 若为），依次弹出s1栈顶的操作符并压入s2，直到遇到（，丢弃一对括号
6. 重复上述步骤，直到扫描结束
7. 将s1中剩余操作符弹出并压入s2
8. 依次将s2中元素弹出，结果的逆序即为后缀表达式

```java
public class PolandNotation {
    public static void main(String[] args) {
        //定义逆波兰表达式
        //String suffixExpression = "3 4 + 5 * 16 -";
        //String suffixExpression = "4 5 * 8 - 60 + 8 2 / +";
        String suffixExpression = "( ( 3 + 5 ) * 2 - 3 + 4 ) * ( 4 - 2 ) + 18";
        //思路：1.先将表达式存入ArrayList中
        //2.遍历ArrayList，配合栈完成计算
        List<String> listString = getListString(suffixExpression);
        List<String> l = mid2last(listString);
        System.out.println("后缀表达式为"+l);
        int res = cal(l);
        System.out.println("计算的结果是"+res);
    }

    public static List<String> getListString(String s){
        //将表达式分割
        String[] split = s.split(" ");
        List<String> list = new ArrayList<>();
        Collections.addAll(list, split);
        return list;
    }

    //计算后缀表达式
    public static int cal(List<String> list){
        //创建栈
        Stack<String> strings = new Stack<>();
        int num1;
        int num2;
        for (String s : list) {
            if(s.matches("\\d+")){
                //数字入栈
                strings.push(s);
            } else {
                num2 = Integer.parseInt(strings.pop());
                num1 = Integer.parseInt(strings.pop());
                switch (s){
                    case "+":
                        strings.push((num1+num2)+"");
                        break;
                    case "-":
                        strings.push((num1-num2)+"");
                        break;
                    case "*":
                        strings.push((num1*num2)+"");
                        break;
                    case "/":
                        strings.push((num1/num2)+"");
                        break;
                    default:
                        throw new RuntimeException("无效字符");
                }
            }
        }
        return Integer.parseInt(strings.pop());
    }

    public static List<String> mid2last(List<String> mid){
        Stack<String> ops = new Stack<>();
        Stack<String> last = new Stack<>();
        String op;
        for (String s : mid) {
            if(s.matches("\\d+"))
                last.push(s);
            else if (s.equals("(")){
                ops.push(s);
            } else if(s.equals(")")){
                while (true){
                   op = ops.pop();
                   if(op.equals("("))
                       break;
                   last.push(op);
                }
            } else {
                while (true) {
                    if(ops.empty() || ops.peek().equals("("))
                        ops.push(s);
                    else if (Priority(s) > Priority(ops.peek())) {
                        ops.push(s);
                    } else {
                        last.push(ops.pop());
                        continue;
                    }
                    break;
                }
            }
        }
        while(!ops.empty())
            last.push(ops.pop());
        Stack<String> temp = new Stack<>();
        while (!last.empty())
            temp.push(last.pop());
        List<String> res = new ArrayList<>();
        while (!temp.empty())
            res.add(temp.pop());
        return res;
    }

    public static int Priority(String oper){
        if (oper.equals("*") || oper.equals("/")) {
            return 1;
        } else if (oper.equals("+") || oper.equals("-")) {
            return 0;
        } else
            return -1;
    }
}
```