# 哈夫曼树

基本定义：给定n个权值作为n个叶子节点，构造一棵二叉树，若该树的带权路径长度达到最小，称这样的二叉树为哈夫曼树

构建步骤：

1. 从小到大进行排序，每个数据都是一个节点，每个节点可以看成是一颗最简单的二叉树
2. 取出根节点权值最小的两棵二叉树
3. 组成一棵新的二叉树，该二叉树根节点权值为步骤2中选出的两棵二叉树之和
4. 将新的二叉树加入序列再次排序，重复上述步骤，直至所有数据都被处理

## 代码实现

```java
/**
 * @param arr 待构建序列
 * @return 哈夫曼树的根节点
 */
public static Node createHuffmanTree(int[] arr){
    //1.遍历数组，将每个元素构成一个Node并存入一个新数组
    ArrayList<Node> nodes = new ArrayList<>();
    for (int i : arr) {
        nodes.add(new Node(i));
    }

    while (nodes.size()>1) {
        Collections.sort(nodes);

        Node leftNode = nodes.get(0);
        Node rightNode = nodes.get(1);

        Node parent = new Node(leftNode.value+rightNode.value);
        parent.left = leftNode;
        parent.right = rightNode;

        nodes.remove(leftNode);
        nodes.remove(rightNode);
        nodes.add(parent);
    }

    return nodes.get(0);
}
```

## 实际应用——哈夫曼编码

基本原理：哈夫曼编码是一种变长编码方式（前缀编码）。它将字符出现的次数作为权值。编码不唯一，但长度唯一。

**字符压缩**

1. 首先生成各个字符对应的节点node（包括字符对应的字节和权值）
2. 然后构建哈夫曼树
3. 利用哈夫曼树得到各个字符对应的哈夫曼编码
4. 最后利用哈夫曼编码和原字节数组得到压缩后的结果

代码实例：

```java
public static List<ByteNode> getNodes(byte[] bytes){
    var byteNodes = new ArrayList<ByteNode>();

    Map<Byte, Integer> counts = new HashMap<>();
    for (byte b : bytes) {
        counts.merge(b, 1, Integer::sum);
    }

    for (Map.Entry<Byte, Integer> entry : counts.entrySet()) {
        byteNodes.add(new ByteNode(entry.getKey(), entry.getValue()));
    }
    return byteNodes;
}

public static ByteNode createHuffmanTree(List<ByteNode> list){
    while (list.size()>1){
        Collections.sort(list);

        ByteNode left = list.get(0);
        ByteNode right = list.get(1);

        //非叶子节点没有data
        ByteNode parent = new ByteNode(null, left.weight+right.weight);
        parent.left = left;
        parent.right = right;

        list.remove(left);
        list.remove(right);
        list.add(parent);
    }
    return list.get(0);
}

//记录各个字符的哈夫曼编码
static Map<Byte, String> huffmanCodes = new HashMap<>();
static StringBuilder sb = new StringBuilder();

/*
生成所有叶子节点的哈夫曼编码，并放入map中
规定左为0，右为1
 */
public static void createHuffmanCode(ByteNode node, String code,StringBuilder stringBuilder){
    var stringBuilder2 = new StringBuilder(stringBuilder);
    stringBuilder2.append(code);
    if(node != null){
        if(node.data == null){ //非叶子节点
            //递归处理
            createHuffmanCode(node.left, "0", stringBuilder2);
            createHuffmanCode(node.right, "1", stringBuilder2);
        } else { //叶子节点
            huffmanCodes.put(node.data, stringBuilder2.toString());
        }
    }
}

//重载更便于调用
public static Map<Byte, String> createHuffmanCode(ByteNode root){
    if(root == null)
        return null;
    createHuffmanCode(root.left, "0", sb);
    createHuffmanCode(root.right, "1", sb);
    return huffmanCodes;
}

/**
 * @param bytes 元素字符串对应的byte[]
 * @param huffmanCodes 生成的哈夫曼编码表
 * @return 哈夫曼编码字符串所对应的字节数组
 */
public static byte[] zip(byte[] bytes, Map<Byte, String>huffmanCodes){
    var stringBuilder = new StringBuilder();
    for (byte b : bytes) {
        stringBuilder.append(huffmanCodes.get(b));
    }
    int len;
    if(stringBuilder.length()%8 == 0){
        len = stringBuilder.length()/8;
    } else{
        len = stringBuilder.length()/8+1;
    }
    byte[] huffmanCodeBytes = new byte[len];
    int index = 0;
    for (int i = 0; i < stringBuilder.length(); i += 8){
        String strByte;
        if(i+8 > stringBuilder.length()) {
            strByte = stringBuilder.substring(i);
        } else
            strByte = stringBuilder.substring(i, i+8);
        huffmanCodeBytes[index++] = (byte) Integer.parseInt(strByte, 2);
    }
    return huffmanCodeBytes;
}

//将前面的方法封装起来，便于调用
public static byte[] huffmanZip(byte[] bytes){
    var nodes = getNodes(bytes);
    var huffmanTree = createHuffmanTree(nodes);
    var huffmanCode = createHuffmanCode(huffmanTree);
    return zip(bytes, huffmanCode);
}
```

**字符解压**

1. 将字节数组先转换为哈夫曼编码对应的0-1字符串
2. 将0-1字符串对照哈夫曼编码重新转化为原字符串

代码实例：

```java
/**
 * 将一个byte转换为二进制字符串
 * @param flag 表示是否需要补高位
 * @param b 传入的byte
 * @return 该byte对应的二进制字符串
 */
public static String byteToBitString(boolean flag, byte b){
    int temp = b;
    if(flag)
        temp |= 256; //补高位
    String str = Integer.toBinaryString(temp); //返回的是temp对应的二进制补码
    if(flag)
        return str.substring(str.length()-8);
    else
        return str;
}

/**
 * 完成对压缩数据的解码
 * @param huffmanCodes 哈夫曼编码表
 * @param huffmanBytes 哈夫曼编码得到的字节数组
 * @return 原字符串对应的字节数组
 */
public static byte[] decode(Map<Byte, String> huffmanCodes, byte[] huffmanBytes){
    //1.先得到huffmanBytes对应的二进制字符串
    var stringBuilder = new StringBuilder();
    //将byte数组转成二进制字符串
    for (int i = 0; i < huffmanBytes.length; i++) {
        //最后一个byte无需补全
        boolean flag = (i != huffmanBytes.length-1);
        stringBuilder.append(byteToBitString(flag, huffmanBytes[i]));
    }
    //System.out.println("哈夫曼字节数组对应的二进制字符串="+stringBuilder.toString());

    //将字符串按照哈夫曼编码表进行解码,为此先将原表进行反转
    var map = new HashMap<String, Byte>();
    for (Map.Entry<Byte, String> entry : huffmanCodes.entrySet()) {
        map.put(entry.getValue(), entry.getKey());
    }

    //创建集合，存放byte
    ArrayList<Byte> list = new ArrayList<>();
    for (int i = 0; i < stringBuilder.length();){
        int count = 1;
        boolean flag = true;
        Byte b = null;

        while (flag){
            String key = stringBuilder.substring(i, i+count); //移动count进行配对
            b = map.get(key);
            if(b != null){
                flag = false;
            } else
                count++;
        }
        i += count;
        list.add(b);
    }

    byte[] res = new byte[list.size()];
    for (int i = 0; i < res.length; i++) {
        res[i] = list.get(i);
    }
    return res;
}
```

**文件压缩**

代码示例：

```java
/**
 * 压缩文件
 * @param srcFile 压缩文件的全路径
 * @param dstFile 压缩后的压缩文件存放目录
 */
public static void zipFile(String srcFile, String dstFile) {
    //创建输入流
    FileInputStream fis = null;
    FileOutputStream fos = null;
    ObjectOutputStream oos = null;
    try {
        fis = new FileInputStream(srcFile);
        fos = new FileOutputStream(dstFile);
        oos = new ObjectOutputStream(fos);
        byte[] b = new byte[fis.available()];
        fis.read(b);
        var bytes = huffmanZip(b); //得到压缩文件
        oos.writeObject(bytes); //以序列化流的方式将字节数组写入本地文件
        oos.writeObject(huffmanCodes); //将哈夫曼编码表也写入本地文件
    } catch (FileNotFoundException e) {
        System.out.println(e.getMessage());
    } catch (IOException e) {
        System.out.println(e.getMessage());
    } finally {
        try {
            fis.close();
            oos.close();
        }catch (Exception e){
            System.out.println(e.getMessage());
        }
    }
```

**文件解压**

代码示例：

```java
/**
 * 解压文件
 * @param zipFile 压缩文件
 * @param dstFile 解压到哪个路径
 */
public static void unzipFile(String zipFile, String dstFile) {
    FileInputStream fis;
    ObjectInputStream ois = null;
    FileOutputStream fos = null;
    try {
        fis = new FileInputStream(zipFile);
        ois = new ObjectInputStream(fis);
        byte[] huffmanBytes = (byte[]) ois.readObject();
        Map<Byte, String> huffmancodes;
        huffmancodes = (Map<Byte, String>)ois.readObject();
        System.out.println(1);
        byte[] decoded = decode(huffmancodes, huffmanBytes);
        fos = new FileOutputStream(dstFile);
        fos.write(decoded);
        System.out.println(2);
    } catch (Exception e){
        System.out.println(e.getMessage());
    } finally {
        try {
            ois.close();
            fos.close();
        } catch (Exception e){
            System.out.println(e.getMessage());
        }
    }
}
```
