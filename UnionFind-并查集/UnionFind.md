# 并查集-UnionFind

### 模板：
```Java
// 并查集标准模板考虑到平衡性以及路径压缩
class UF {
    // 连通分量的个数
    private int count;
    // 储存一棵树
    private int[] parent;
    // 记录树的的重量即size
    private int[] size;

    // Contructor
    public UF(int n) {
        this.count = n;
        this.parent = new int[n];
        this.size = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }

    // 合并
    public void union (int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        if (rootP == rootQ) return;
        // 小树连接到大树上
        if (size[rootP] > size[rootQ]) {
            parent[rootQ] = rootP;
            size[rootP] += size[rootQ];
        } else {
            parent[rootP] = rootQ;
            size[rootQ] += size[rootP];
        }

        count--;
    }
    // 判断是否连通
    public boolean connected(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        return rootP == rootQ;
    }
    // 查找
    public int find(int x) {
        while (parent[x] != x) {
            //进行路径压缩
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    }

    ///////////////////////////////////////////
    // Get max size
    public int getMaxSize() {
        int maxSize = 0;
        for (int i = 0; i < parent.length; i++) {
            if (parent[i] == i && size[i] > maxSize) {
                maxSize = size[i];
            }
        }
        
        return maxSize;
    }
    
}
```

### 复杂度：
    Union-Find 算法的复杂度可以这样分析：构造函数初始化数据结构需要 O(N) 的时间和空间复杂度；连通两个节点union、判断两个节点的连通性connected、计算连通分量count所需的时间复杂度均为 O(1)。