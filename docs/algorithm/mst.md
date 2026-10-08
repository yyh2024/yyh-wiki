# 最小生成树

**英文名：** $Minimum Spanning Tree$ ，$MST$

## 定义

我们定义无向连通图的 **最小生成树** 为边权和最小的生成树．


<blockquote>
<font color='#ffc116'>
<details>
<summary > <strong> 注意 </strong></summary>
  <font color='grey'>
    <h6></h6>

只有 **连通图** 才有生成树．
  
  </font>
</details>
</font>
</blockquote>

---

# Kruskal 算法

**Kruskal** 算法是一种常见并且好写的最小生成树算法，该算法的基本思想是从小到大加入边，是一个贪心算法．

**前置知识**：并查集 ，排序


## 思路

将边从小到大排序后依次加入图中，不断重复此步骤直到图连通

这里我们可以使用 **并查集** 实现加边与判断图是否连通的操作

使用 $O$ $(m$ $log$ $m)$ 的排序算法，并使 $O$ $(max$ $(m$ $,$ $n)$ $)$ 或 $O$ $(m$ $log$ $n)$ 的并查集，就可以得到 $O$ $(m$ $log$ $m)$ 的 **Kruskal** 算法．


## 实现

**代码详解：** 

使用 **并查集** 存图，使用 **结构体 sort排序** 实现从小到大存边，再将边加入图中．

将最小生成树的边权之和输出，如图不连通，则输出 **orz**

<blockquote>
<font color='#52c41a'>
<details>
<summary > <strong>代码实现</strong></summary>
  <font color='grey'>
    <h6></h6>
    
```cpp
#include<bits/stdc++.h>
using namespace std;

//定义
int f[5005],n,m,ans=0,cnt=0;
struct node{
	int x,y,z;
}a[200005];

//并查集操作部分
bool cmp(node px,node py){
	return px.z<py.z;
}
int find(int fx){
	if(f[fx]==fx) return fx;
	return f[fx]=find(f[fx]);
}
bool check(int cx,int cy){
	return find(cx)==find(cy);
}
void unite(int ux,int uy){
	if(!check(ux,uy)) f[find(ux)]=find(uy);
	return ;
}

int main(){
  
  // 输入部分
	cin>>n>>m;
	for(int i=1;i<=n;i++){
		f[i]=i;
	}
	for(int i=1;i<=m;i++){
		cin>>a[i].x>>a[i].y>>a[i].z;
	}

  //对边进行排序，将边以此加入图中
  sort(a+1,a+m+1,cmp);
	for(int i=1;i<=m;i++){
		if(!check(a[i].x,a[i].y)){
			unite(a[i].x,a[i].y);
			ans+=a[i].z;
			cnt++;
		}
	}
	
  if(cnt!=n-1) cout<<"orz"; //判断加入图中的边是否有 n-1 条边，如不是，那么图不连通
	else cout<<ans;
	return 0;
}
```
  
  </font>
  
</details>
</font>
</blockquote>


## 例题

**模板：** 

[洛谷 P3366 【模板】最小生成树](https://www.luogu.com.cn/problem/P3366)

[洛谷 P1195 口袋的天空](https://www.luogu.com.cn/problem/P1195)

**进阶：**

[洛谷 P1550 [USACO08OCT] Watering Hole G](https://www.luogu.com.cn/problem/P1550)

---

# Prim 算法

**Prim** 算法是另一种常见并且好写的最小生成树算法．该算法的基本思想是从一个结点开始，不断加点（而不是 **Kruskal** 算法的加边）．

## 思路

具体来说，每次要选择距离最小的一个结点，以及用新的边更新其他结点的距离．

其实跟 **Dijkstra** 算法一样，每次找到距离最小的一个点，可以暴力找也可以用堆维护．

堆优化的方式类似 **Dijkstra** 的堆优化

<blockquote>
<font color='#52c41a'>
<details>
<summary > <strong> 部分代码实现 </strong></summary>
  <font color='grey'>
    <h6></h6>

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=5005,M=2e5+5;
int n,m,cnt,ans;
struct edge{
	int v,w;
};
vector<edge> g[N];
bool vis[N];
void prim(){
	priority_queue< pair<int,int> >q;
	vis[1]=0;
	q.push({0,1});
	while(!q.empty()){
		int v=q.top().second,dis=q.top().first;
		q.pop();
		if(vis[v])continue;
		ans+=-dis;
		cnt++;
		vis[v]=1;
		for(auto i:g[v]){
			if(vis[i.v])continue;
			q.push({-i.w,i.v});
		}
	}
	if(cnt==n)cout<<ans;
	else cout<<"orz";
}
int main(){
	cin>>n>>m;
	for(int i=1;i<=m;i++){
		int u,v,w;cin>>u>>v>>w;
		g[u].push_back({v,w});
		g[v].push_back({u,w});
	}
	prim();
	return 0;
}

```
  
  </font>
</details>
</font>
</blockquote>

## 例题

**基础：**

[洛谷 P2504 [HAOI2006] 聪明的猴子](https://www.luogu.com.cn/problem/P2504)

[洛谷 P2330 [SCOI2005] 繁忙的都市](https://www.luogu.com.cn/problem/P2330)
