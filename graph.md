# 图

## 图的创建

### 邻接矩阵

```java
public class Graph {
    private ArrayList<String> vertexList;
    private int[][] edges;
    private int numOfEdges;

    public static void main(String[] args) {
        int n = 5;
        String[] VertexValue = {"a", "b", "c", "d", "e"};
        //创建图对象
        var graph = new Graph(n);
        for (String v : VertexValue) {
            graph.insertVertex(v);
        }
        //添加边
        graph.insertEdge(0, 1, 1);
        graph.insertEdge(0, 2, 1);
        graph.insertEdge(1, 2, 1);
        graph.insertEdge(1, 3, 1);
        graph.insertEdge(1, 4, 1);

        graph.show();
    }

    public Graph(int n){
        edges = new int[n][n];
        vertexList = new ArrayList<String>(n);
        numOfEdges = 0;
    }

    public void insertVertex(String vertex){
        vertexList.add(vertex);
    }

    public void insertEdge(int v1, int v2, int weight){
        //无向图
        edges[v1][v2] = weight;
        edges[v2][v1] = weight;
        numOfEdges++;
    }

    public int getNumOfEdges(){
        return numOfEdges;
    }

    public int getNumOfVertex(){
        return vertexList.size();
    }

    public String getValueByIndex(int i){
        return vertexList.get(i);
    }

    public int getWeight(int v1, int v2){
        return edges[v1][v2];
    }

    public void show(){
        for (int[] link : edges){
            System.out.println(Arrays.toString(link));
        }
    }
}
```

## 图的遍历

### 深度优先遍历DFS

```java
//DFS
public void dfs(int i){
    System.out.println(getValueByIndex(i));
    isVisited[i] = true;
    int w = getFirstNeighbor(i);
    while (w != -1) {
        if(!isVisited[w]) {
            dfs(w);
        }
        w = getNextNeighbor(i, w);
    }
}

//重载dfs
public void dfs(){
    for (int i = 0; i < getNumOfVertex(); i++) {
        if(!isVisited[i])
            dfs(i);
    }
}
```

### 广度优先遍历BFS

```java
//BFS
public void bfs(boolean[] isVisited, int i){
    Queue<Integer> q = new LinkedList<>();
    q.offer(i);
    //入队时进行标记
    isVisited[i] = true;
    int w;
    int v;
    while(!q.isEmpty()){
        int sz = q.size();
        for (int k = 0; k < sz; k++) {
            v = q.poll();
            System.out.println(getValueByIndex(v));
            w = getFirstNeighbor(v);
            while(w != -1){
                if(!isVisited[w]) {
                    q.offer(w);
                    isVisited[w] = true;
                }
                w = getNextNeighbor(v, w);
            }
        }
    }
}

public void bfs(){
    bfs(isVisited, 0);
}
```