# 递归

## 案例一：迷宫

```java
public class MiGong {
    public static void main(String[] args) {
        //创建地图
        int[][] map = new int[8][7];
        //使用1表示墙,将上下置为1
        for (int i = 0; i < 7; i++) {
            map[0][i] = 1;
            map[7][i] = 1;
        }
        //将左右置为1
        for (int i = 0; i < 8; i++) {
            map[i][0]=1;
            map[i][6]=1;
        }
        //设置挡板
        map[3][1] = 1;
        map[3][2] = 1;
        /*map[1][2] = 1;
        map[2][2] = 1;*/
        //输出地图
        System.out.println("地图的情况");
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 7; j++) {
                System.out.print(map[i][j]+" ");
            }
            System.out.println();
        }

        //使用递归回溯
        setWay2(map, 1, 1);
        System.out.println("小球走过，并标识过的地图情况");
        //输出新地图,走过并标识过的地图
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 7; j++) {
                System.out.print(map[i][j] + " ");
            }
            System.out.println();
        }
    }

    //使用递归回溯来寻路
    //map为地图，i和j表示起点
    //如果能到达map[6][5]，则说明通路找到
    //若map[i][j]=0表示该点没有走过，为1表示墙，2表示通路可以走，3表示该点已经走过，但是走不通
    //走迷宫时的策略为下-》右-》上-》左,如果走不通则回溯
    public static boolean setWay(int[][] map, int i, int j){
        if(map[6][5] == 2){//通路已找到
            return true;
        } else {
            if (map[i][j] == 0){//如果还没有走过这个点
                map[i][j] = 2; //假定可以走通
                if(setWay(map, i+1, j)){ //向下走
                    return true;
                } else if (setWay(map, i, j+1)) {
                    return true;
                } else if (setWay(map, i-1, j)) {
                    return true;
                } else if (setWay(map, i, j-1)) {
                    return true;
                } else {
                    map[i][j] = 3;
                    return false;
                }
            } else  {
                return false;
            }
        }
    }

    //修改寻路策略，改成上->右->下->左
    public static boolean setWay2(int[][] map, int i, int j){
        if(map[6][5] == 2){//通路已找到
            return true;
        } else {
            if (map[i][j] == 0){//如果还没有走过这个点
                map[i][j] = 2; //假定可以走通
                if(setWay2(map, i-1, j)){ //向上走
                    return true;
                } else if (setWay2(map, i, j+1)) {
                    return true;
                } else if (setWay2(map, i+1, j)) {
                    return true;
                } else if (setWay2(map, i, j-1)) {
                    return true;
                } else {
                    map[i][j] = 3;
                    return false;
                }
            } else  {
                return false;
            }
        }
    }
}
```

## 案例二：八皇后

```java
public class EightQueen {
    int max  = 8;
    //数组array来保存皇后位置
    int[] array = new int[max];

    static int count = 0;
    static int judgeCount = 0;

    public static void main(String[] args) {
        var eightQueen = new EightQueen();
        eightQueen.check(0);
        System.out.println("一共有"+count+"种解法");
        System.out.println("judge调用次数"+judgeCount);
    }

    private void check(int n){
        if(n == max){
            print();
            count++;
            return;
        }

        //依次放入皇后，并判断是否冲突
        for (int i = 0; i < max; i++) {
            //先把当前皇后放到该行的第一列
            array[n] = i;
            //判断当放置第n个皇后到i列时，是否冲突
            if(judge(n)){
                check(n+1);
            }
            //如果冲突，就继续执行array[n]=i
        }
    }

    //查看当我们放置第n个皇后，检测是否冲突
    private boolean judge(int n){
        judgeCount++;
        for (int i = 0; i < n; i++) {
            if(array[i] == array[n] || Math.abs(n-i) == Math.abs(array[n] - array[i]))
                return false;
        }
        return true;
    }

    //写一个方法，可以将皇后摆放位置打印出来
    private void print(){
        for (int i = 0; i < array.length; i++) {
            System.out.print(array[i]+" ");
        }
        System.out.println();
    }
}
```