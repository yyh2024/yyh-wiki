# 最短路

$Shortest$ $Path$

<font color='#3498db'>
<details>
<summary > <strong>声明</strong></summary>
  <font color='gray'>
    <h6></h6>
    
为了方便叙述，这里先给出下文将会用到的一些记号的含义．
  
$n$ 为图上点的数目，$m$ 为图上边的数目；

$s$ 为最短路的源点；


  
  </font>
</details>
</font>


### 定义

在图论中，**最短路径** 是指在一个加权图中，从起始顶点到目标顶点的所有可能路径中，各边上权值之和最小的那条路径

<font color='#ffc116'>
<details>
<summary > <strong>特殊情况</strong></summary>
  <font color='grey'>
    <h6></h6>
    
**负环** ：如果图中存在一个环路，且该环路上所有边的权值之和为负数（称为“负环”），那么在这个环里无限循环会使路径总代价不断减小，此时两点之间的最短路径将不存在（趋于无穷小）
  
  </font>
  
</details>
</font>

---

## Floyd 算法

**前置知识** ：DP（动态规划）

是一种 **多源最短路算法** ，用来求任意两个结点之间的最短路的．

复杂度比较高，但是常数小，容易实现（只有三个 for）．

适用于任何图，不管有向无向，边权正负，但是最短路必须存在．（不能存在负环）

### 实现

我们定义一个数组 `f[k][i][j]`，表示只允许经过结点 $1$ 到 $k$ （也就是通过 $1$ 到 $k$ 之中的结点作为中转节点，但 $i$ 或 $j$ **不一定**在 $1$ 到 $k$ 之间），结点 $i$ 到结点 $j$ 的最短路长度．

很显然，`f[n][i][j]` 就是结点 $i$ 到结点 $j$ 的最短路长度

接下来考虑如何求出 `f` 数组的值．

`f[0][i][j]` 的数值为： $i$ 与 $j$ ，或 $0$ ，或 $+∞$ 


<font color='#3498db'>
<details>
<summary > <strong> 定义明细 </strong></summary>
  <font color='grey'>
    <h6></h6>

当 $i$ 与 $j$ 间有直接相连的边的时候，为它们的边权；

当 $i=j$ 的时候为零，因为到本身的距离为零；

当 $i$ 与 $j$ 没有直接相连的边的时候，为 $+∞$
    
  </font>
</details>
</font>

所以 `f` 数组的状态转移方程为：`f[k][i][j] = min(f[k-1][i][j], f[k-1][i][k]+f[k-1][k][j])` 

其中，`f[k-1][i][j]` 为不经过 $k$ 点的最短路径，`f[k-1][i][k]+f[k-1][k][j]` 为经过了 $k$ 点的最短路

**即**

```cpp
for (k = 1; k <= n; k++) {
  for (i = 1; i <= n; i++) {
    for (j = 1; j <= n; j++) {
      f[k][i][j] = min(f[k - 1][i][j], f[k - 1][i][k] + f[k - 1][k][j]);
    }
  }
}
```

因为第一维（`f[k][i][j]` 中的 `k` 维）对结果无影响，我们可以发现数组的第一维是可以省略的，于是可以直接改成 `f[i][j] = min(f[i][j], f[i][k]+f[k][j])`．

<font color='#52c41a'>
<details>
<summary > <strong> 部分代码实现 </strong></summary>
  <font color='grey'>
    <h6></h6>

```cpp
for (k = 1; k <= n; k++) {
  for (i = 1; i <= n; i++) {
    for (j = 1; j <= n; j++) {
      f[i][j] = min(f[i][j], f[i][k]+f[k][j])
    }
  }
}
```
  
  </font>
</details>
</font>

综上，时间复杂度是 $𝑂$ $( n^3 )$
，空间复杂度是 $O$ $( n^2 )$．

---

## Dijkstra 算法

**Dijkstra** 算法于 $1956$ 年发现，$1959$ 年公开发表．是一种求解 非负权图 上单源最短路径的算法．

### 实现

将结点分成两组：已确定最短路长度的点集（记为 $S$ 集合）的和未确定最短路长度的点集（记为 $T$ 集合），一开始所有的点都属于 $T$ 集合．

初始化 `dis[s]=0` ，其他点的 `dis[]` 均为 $+\infty$

然后重复这些操作：

  1. 从 $T$ 集合中，选取一个最短路长度最小的结点，移到 $S$ 集合中．
  2. 对刚刚被加入 $S$ 集合的结点的所有出边执行松弛操作．

直到 $T$ 集合为空，算法结束．

## 时间复杂度

朴素的方法每次执行 **操作2** 后，直接在 $T$ 集合中暴力寻找最短路长度最小的结点．

**操作2** 总时间复杂度为 $O(m)$ 

**操作1** 总时间复杂度为 $O(n^2)$ 

全过程的时间复杂度为 $O$ $(n^2 + m)$ $=$ $O$ $(n^2)$．

<font color='#52c41a'>
<details>
<summary > <strong>朴素实现</strong></summary>
  <font color='grey'>
    <h6></h6>

```cpp
struct edge {
  int v, w;
};

vector<edge> e[MAXN];
int dis[MAXN], vis[MAXN];

void dijkstra(int n, int s) {
  memset(dis, 0x3f, (n + 1) * sizeof(int));
  dis[s] = 0;
  for (int i = 1; i <= n; i++) {
    int fu = 0, t = INT_MAX;
    for (int j = 1; j <= n; j++)
      if (!vis[j] && dis[j] < t) fu = j, t = dis[j];
    vis[fu] = true;
    for (auto fe : e[u]) {
      int fv = fe.v, fw = fe.w;
      if (dis[fv] > dis[fu] + fw) dis[fv] = dis[fu] + fw;
    }
  }
  return ;
}
```
  
  </font>
</details>
</font>

<font color='#52c41a'>
<details>
<summary > <strong>优先队列优化</strong></summary>
  <font color='grey'>
    <h6></h6>

可以使用 **优先队列 （STL）** 维护，通过每次松弛时将结点入队，且弹出时检查该结点是否已被松弛过，若是则跳过

```cpp
struct edge {
  int v, w;
};

struct node {
  int dis, u;

  bool operator>(const node& a) const { return dis > a.dis; }
};

vector<edge> e[MAXN];
int dis[MAXN], vis[MAXN];
priority_queue<node, vector<node>, greater<node>> q;

void dijkstra(int n, int s) {
  memset(dis, INT_MAX, (n + 1) * sizeof(int));
  memset(vis, 0, (n + 1) * sizeof(int));
  dis[s] = 0;
  q.push({0, s});
  while (!q.empty()) {
    int fu = q.top().u;
    q.pop();
    if (vis[fu]) continue;
    vis[fu] = 1;
    for (auto fe : e[u]) {
      int fv = fe.v, fw = fe.w;
      if (dis[fv] > dis[fu] + fw) {
        dis[fv] = dis[fu] + fw;
        q.push({dis[fv], fv});
      }
    }
  }
  return ;
}
```
  
  </font>
</details>
</font>

综上，

**朴素实现** 时间复杂度是 $𝑂$ $( n^2 )$

**优先队列做法实现** 时间复杂度是 $𝑂$ $( m$ $log$ $m )$

---

## Bellman–Ford 算法

**Bellman–Ford** 算法是一种基于 **松弛（relax）** 操作的最短路算法，可以求出有负权的图的最短路，并可以对最短路不存在的情况进行判断．

在国内 **OI** 界，下文要讲的的 **SPFA** ，就是 **Bellman–Ford** 算法的一种实现．

### 过程

**Bellman–Ford** 算法需要用到 **松弛操作**（ **Dijkstra** 算法也会用到）．

对于边 $( u,v )$，松弛操作对应下面的式子：
 $dis(v) = \min(dis(v), dis(u) + w(u, v))$．

我们尝试用 $S \to u \to v$ （其中
 $S \to u$ 的路径取最短路）这条路径去更新
$v$ 点最短路的长度，如果这条路径更优，就进行更新．

**Bellman–Ford** 算法所做的，就是不断尝试对图上每一条边进行松弛．

我们每进行一轮循环，就对图上所有的边都尝试进行一次松弛操作，当一次循环中没有成功的松弛操作时，算法停止．

每次循环是 $O(m)$ 的，

且在最短路存在的情况下，由于一次松弛操作会使最短路的边数至少 $+1$，而最短路的边数最多为 $n-1$，因此整个算法最多执行 $n-1$ 轮松弛操作．故总时间复杂度为 $O(nm)$．

但还有一种情况，如果从 $S$ 点出发，抵达一个负环时，**松弛** 操作会无休止地进行下去．

前面的论证中已经说明了，对于最短路存在的图，松弛操作最多只会执行 $n-1$ 轮，因此如果第 $n$ 轮循环时仍然存在能松弛的边，说明从 $S$ 点出发，能够抵达一个 **负环**．

<font color='#ffc116'>
<details>
<summary > <strong>负环判断中存在的常见误区</strong></summary>
  <font color='gray'>
    <h6></h6>
    
需要注意的是，以 $S$ 点为源点跑 **Bellman–Ford** 算法时，如果没有给出存在负环的结果，只能说明从 $S$ 点出发不能抵达一个负环，而不能说明图上 **不存在负环**．

因此如果需要判断整个图上是否存在负环，最严谨的做法是建立一个 **超级源点**，向图上每个节点连一条权值为 0 的边，然后以超级源点为起点执行 **Bellman–Ford** 算法．

  </font>
</details>
</font>

### 实现

<font color='#52c41a'>
<details>
<summary > <strong>部分简单实现</strong></summary>
  <font color='gray'>
    <h6></h6>

```cpp
struct edge {
  int u,v,w;
};
vector<edge> edge;
int dis[MAXN],u,v,w;
const int INF=0x3f3f3f3f;

bool bellmanford(int n, int s){
  memset(dis,0x3f,(n+1)*sizeof(int));
  dis[s]=0;
  bool bol=false;
  for(int i=1;i<=n;i++){
    bol=false;
    for(int j=0;j<edge.size();j++){
      u=edge[j].u,v=edge[j].v,w=edge[j].w;
      if(dis[u]==INF) continue;
      if(dis[v]>dis[u]+w){
        dis[v]=dis[u]+w;
        bol=true;
      }
    }
    if(!bol) break;
  }
  return bol;
}
```

  </font>
</details>
</font>

---

## SPFA

即 **Shortest Path Faster Algorithm** ．

**Bellman–Ford** 算法的队列优化版．

很多时候我们并不需要那么多 **无用的** 松弛操作．

很显然，只有 **上一次** 被松弛的结点，所连接的边，才有可能引起 **下一次** 的松弛操作．

那么我们用队列来维护 **哪些结点可能会引起松弛操作** ，就能只访问必要的边了．

**SPFA** 也可以用于判断 $s$ 点是否能抵达一个 **负环**，只需记录最短路经过了多少条边，当经过了至少 $n$ 条边时，说明 $s$ 点可以抵达一个 **负环**．

<font color='#52c41a'>
<details>
<summary > <strong> 部分代码实现 </strong></summary>
  <font color='grey'>
    <h6></h6>
    
```cpp
struct edge {
  int v,w;
};
vector<edge> e[MAXN];
int dis[MAXN],cnt[MAXN],vis[MAXN];
queue<int> q;

bool spfa(int n,int s) {
  memset(dis,0x3f,(n+1)*sizeof(int));
  dis[s]=0,vis[s]=1;
  q.push(s);
  while(!q.empty()){
    int u=q.front();
    q.pop(),vis[u]=0;
    for(auto te : e[u]){
      int v=te.v,w=te.w;
      if(dis[v]>dis[u]+w){
        dis[v]=dis[u]+w;
        cnt[v]=cnt[u]+1;
        if(cnt[v]>=n) return false;
        if(!vis[v]){
        	q.push(v);
			vis[v]=1;
		}
      }
    }
  }
  return true;
}
```

  </font>
</details>
</font>

虽然在 **大多数** 情况下 **SPFA** 跑得很快，但其最坏情况下的时间复杂度为 $O(nm)$，将其卡到这个复杂度也是不难的，所以考试时要 **谨慎使用**（在没有负权边时最好使用 **Dijkstra** 算法）
