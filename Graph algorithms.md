# Graph algorithms

## Graph traversal

### DFS

```cpp
vector<int> adj[N];
bool vis[N];

void dfs(int u){
  if(vis[u]) return;
  vis[u]=true;
  for(auto v:adj[u]) dfs(v);
  // recursive thing can be done here!
}
```

### BFS

```cpp
queue<int> q; // we can use stack for dfs
q.push(u);
while(!q.empty()){
  int v=q.front();
  q.pop();
  if(vis[v]) continue;
  vis[v]=true;
  for(int x:adj[v]) q.push(x);
}
```

## Topological sort

```cpp
vector<int> adj[N],top;
bool vis[N];

void dfs(int u){
	if(vis[u]) return;
	vis[u]=true;
	for(auto v:adj[u]) dfs(v);
	top.push_back(u);
}

void topsort(){
	for(int i=1;i<=n;i++)
    if(!vis[i]) dfs(i);
	reverse(top.begin(),top.end());
}
```

```cpp
int indeg[N];
vector<int> adj[N],top;
queue<int> q;
.
.
.
for(int i=1;i<=n;i++){
  if(indeg[i]==0) q.push(i);
}
while(!q.empty()){
  int u=q.top();
  q.pop();
  top.push_back(u);
  for(auto v:adj[u]){
    indeg[v]--;
    if(indeg[v]==0) q.push(v);
  }
}
```

## Shortest paths

### Dijkstra

```cpp
#define pii pair<int,int>
#define f first
#define s second
vector<int> d(N,INF);
vecor<pii> adj[N];
.
.
.
priority_queue<pii,vector<pii>,greater<pii>> q;
q.push({0,starting_node});
d[starting_node]=0;
while(!q.empty()){
  int d_u=q.top().f, u=q.top().s;
  q.pop();
  if(vis[u]) continue;
  vis[u]=true;
  for(auto vw:adj[u]){
    // additional states can be done here!
    if(d[v]>d[u]+w){
      d[v]=d[u]+w;
      q.push({d[v],v});
    }
  }
}
```

### Bellman-Ford

```cpp
struct edge{
  int u,v,w;
};
vector<edge> edges;
vector<int> d(N,INF);
.
.
.
// update the distance from edge list n-1 times
d[starting_node]=0;
// can run one more time to detect a negative cycle 
for(int i=0;i<n-1;i++){
	bool change=false;
  for(auto e:edges){
    int u=e.u, v=e.v, w=e.w;
    if(d[v]>d[u]+w){
      d[v]=d[u]+w;
      action=true;
    }
  }
  if(!action) break;
}
```

### Floyd-Warshall

```cpp
int d[N][N];

for(int i=1;i<=n;i++)
  for(int j=1:j<=n;j++)
    d[i][j]=(i==j)?0:INF;

for(int i=1;i<=m;i++){
  cin >> u >> v >> w;
  d[u][v]=w;
}

// like range dp: loop must be ordered as KIJ
for(int k=1;k<=n;k++)
	for(int i=1;i<=n;i++)
		for(int j=1;j<=n;j++)
			d[i][j]=min(d[i][j],d[i][k]+d[k][j]);
```

## Minimum spanning trees

### Kruskal

```cpp
// DSU-structure
struct edge{
	int u,v,w;
	bool operator <(const edge&o)const{
		// min or max
		retunrn w>o.w;
	}
};
vector<edge> edges;
int parent[N],size[N];

void initial(){
	for(int i=1;i<=n;i++){
		parent[i]=i;
		size[i]=1;
	}
}
int find(int u){
	return parent[u]=u==parent[u]?u:_find(parent[u]);
}
void unite(int u,int v,int w){
	u=find(u);
	v=find(v);
	if(u!=v){
		if(size[u]<size[v]) swap(u,v);
		parent[v]=u;
		size[u]+=size[v];
		// MST can be applied here
		mst+=w;
	}
}
.
.
.
initial();
sort(edges.begin(),edges.end());
for(auto x:edges){
	int u=x.u,v=x.v,w=x.w;
	if(parent[u]!=parent[v]) unite(u,v,w);
}
```

### Prim

```cpp
// O(N^2) for a complete graph
// collect a graph as adj matrix
int min_e[N],adj[N][N],u=starting_node,mst;
// min_e is an array collecting a minimum edge from each node
fill_n(min_e,N,INF);
for(int i=0;i<n;i++){
	// v is a new node which has a minimum length from current node (u)
	// and mn is that minimum length
	int mn=INF,v=-1;
	min_e[u]=-1;
	for(int j=0;j<n;i++){
		// min_e == -1 means visited
		if(min_e[j]==-1) continue;
		min_e[j]=min(min_e[j],adj[u][j]);
		if(mn>min_e[j]) mn=min_e[j],v=j;
	}
	mst+=mn;
	u=v;
}
```

```cpp
// O(NlogM) for the sprase graph
// the implementation looks like dijktra
// but dis[N] collects minimum edge adjacent to each node
priority_queue<pii,vector<pii>,greater<pii>> pq;
pq.push({0,1});
dis[1]=0;
while(!pq.empty()){
	int d_u=pq.top().first,u=pq.top().second;
	pq.pop();
	if(vis[u]) continue;
	vis[u]=true;
	mst+=d_u;
	for(auto vw:adj[u]){
		int v=vw.first,w=vw.second;
		if(!vis[v] && dis[v]>w) pq.push({dis[v]=w,v});
	}
}
```