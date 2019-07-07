## POJ3417 Network

发现如果删除了一条主要边的话，剩下的附加边可以分情况讨论为以下3种：

1. 没有附加边影响它，这时随便删就行。
2. 有一条附加边，那么显然要把这个割了。
3. 有两条及以上，显然不行。

然后直接树上差分算每一个的贡献就好了。

```cpp
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<math.h>
#include<algorithm>
#include<queue>
#include<set>
#include<map>
#include<iostream>
using namespace std;
#define re register
#define ll long long
inline int gi(){
	int f=1,sum=0;char ch=getchar();
	while(ch>'9' || ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0' && ch<='9'){sum=(sum<<3)+(sum<<1)+ch-'0';ch=getchar();}
	return f*sum;
}
const int N=100010;
int n,m,front[N],cnt;
struct node{int to,nxt;}e[N<<1];
void Add(int u,int v){e[++cnt]=(node){v,front[u]};front[u]=cnt;}
int f[N][20],dep[N];//18
void dfs(int u,int ff){
	dep[u]=dep[ff]+1;f[u][0]=ff;
	for(int i=front[u];i;i=e[i].nxt){
		int v=e[i].to;if(v==ff)continue;
		dfs(v,u);
	}
}
int getlca(int u,int v){
	if(dep[u]<dep[v])swap(u,v);
	for(int i=18;~i;i--)
		if(dep[u]-(1<<i)>=dep[v])u=f[u][i];
	if(u==v)return u;
	for(int i=18;~i;i--)
		if(f[u][i]!=f[v][i])
			u=f[u][i],v=f[v][i];
	return f[u][0];
}
struct edge{int u,v;}edg[N];
ll ans;int p[N];
int solve(int u,int ff){
	for(int i=front[u];i;i=e[i].nxt){
		int v=e[i].to;if(v==ff)continue;
		int tmp=solve(v,u);
		p[u]+=tmp;
		if(!tmp)ans+=m;
		else if(tmp==1)ans++;
	}
	return p[u];
}
int main(){
	n=gi();m=gi();
	for(int i=1;i<n;i++){
		int u=gi(),v=gi();
		Add(u,v);Add(v,u);
	}
	dfs(1,1);
	for(int j=1;j<=18;j++)
		for(int i=1;i<=n;i++)
			f[i][j]=f[f[i][j-1]][j-1];
	for(int i=1;i<=m;i++){
		int u=gi(),v=gi(),lca=getlca(u,v);
		p[u]++;p[v]++;p[lca]--;p[lca]--;
	}
	solve(1,1);
	printf("%lld\n",ans);
	return 0;
}
```

