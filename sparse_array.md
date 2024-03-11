# 稀疏数组

一般用于节约内存空间。

二维数组转稀疏数组：

1. 遍历原始二维数组，得到有效数据个数sum
2. 根据sum创建稀疏数组sparseArr\[sum+1][3]
3. 将二维数组的有效数据存入稀疏数组

稀疏数组转二维数组：

1. 读取稀疏数组的第一行，根据第一行创建二维数组
2. 读取稀疏数组后几行的数据，并赋给创建的二维数组即可

代码实现：

```java
public static void main(String[] args) throws IOException {
    /*
    二维数组 -> 稀疏数组
     */

    //创建一个原始的二维数组
    //0：表示没有棋子 1：黑子 2：白子
    int chessArr1[][] = new int[11][11];
    chessArr1[1][2] = 1;
    chessArr1[2][3] = 2;
    int count = 0;
    for (int[] row : chessArr1) {
        for (int i : row) {
            if(i!=0)
                count++;
        }
    }

    //创建稀疏数组
    int[][] sparseArr1 = new int[count+1][3];
    sparseArr1[0][0] = 11;
    sparseArr1[0][1] = 11;
    sparseArr1[0][2] = count;
    count = 0;

    //遍历二维数组，将非零值存储到sparseArr1中
    for (int i = 0; i < chessArr1.length; i++) {
        for (int j = 0; j < chessArr1[i].length; j++) {
            if(chessArr1[i][j] != 0){
                count++;
                sparseArr1[count][0] = i;
                sparseArr1[count][1] = j;
                sparseArr1[count][2] = chessArr1[i][j];
            }
        }
    }

    //写入本地文件
    BufferedWriter bw = new BufferedWriter(new FileWriter(new File("myFile\\map.data")));
    for (int[] row : sparseArr1) {
        String str = row[0] + "&" + row[1] + "&" + row[2];
        bw.write(str);
        bw.newLine();
    }
    bw.close();


    /*//打印稀疏数组
    System.out.println("稀疏数组为~~~");
    for (int[] row : sparseArr1) {
        System.out.printf("%d\t%d\t%d\t\n",row[0], row[1], row[2]);
    }*/


    /*
    稀疏数组 -> 二维数组
     */

    //读取本地文件
    BufferedReader br = new BufferedReader(new FileReader(new File("myFile\\map.data")));
    String line = br.readLine();
    var arr = line.split("&");
    int[][] chessArr2 = new int[Integer.parseInt(arr[0])][Integer.parseInt(arr[1])];
    while ((line = br.readLine())!=null){
        arr = line.split("&");
        chessArr2[Integer.parseInt(arr[0])][Integer.parseInt(arr[1])] = Integer.parseInt(arr[2]);
    }

    //打印读取结果
    for (int[] row : chessArr2) {
        for (int i : row) {
            System.out.print(i+" ");
        }
        System.out.println();
    }
}
```

