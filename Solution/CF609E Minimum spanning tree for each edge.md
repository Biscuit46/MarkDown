## CF609E Minimum spanning tree for each edge

没有难度的题目+1

直接搞一颗最小生成树出来然后倍增求最大边就行了。

```cpp
/*
  mail: mleautomaton@foxmail.com
  author: MLEAutoMaton
  This Code is made by MLEAutoMaton
*/
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
#define ll long long
#define re register
#define int ll
#define file(a) freopen(a".in","r",stdin);freopen(a".out","w",stdout)
inline int gi(){
	int f=1,sum=0;char ch=getchar();
	while(ch>'9' || ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0' && ch<='9'){sum=(sum<<3)+(sum<<1)+ch-'0';ch=getchar();}
	return f*sum;
}
const int N=200010;
int n,m,front[N],ans,cnt;
struct node{int to,nxt,w;}e[N<<1];
struct edge{int u,v,w,id;}ed[N];
bool cmp1(edge a,edge b){return a.w<b.w;}
bool cmp2(edge a,edge b){return a.id<b.id;}
int fa[N];
int find(int x){
	if(fa[x]!=x)fa[x]=find(fa[x]);
	return fa[x];
}
void Add(int u,int v,int w){e[++cnt]=(node){v,front[u],w};front[u]=cnt;}
int dep[N],f[N][20],mx[N][20];//18
void dfs(int u,int ff){
	f[u][0]=ff;dep[u]=dep[ff]+1;
	for(int i=front[u];i;i=e[i].nxt){
		int v=e[i].to;if(v==ff)continue;
		mx[v][0]=e[i].w;
		dfs(v,u);
	}
}
int getmax(int u,int v){
	if(dep[u]<dep[v])swap(u,v);
	int ret=0;
	for(int i=18;~i;i--)
		if(dep[u]-(1<<i)>=dep[v]){ret=max(ret,mx[u][i]);u=f[u][i];}
	if(u==v)return ret;
	for(int i=18;~i;i--)
		if(f[u][i]!=f[v][i]){
			ret=max(ret,max(mx[u][i],mx[v][i]));
			u=f[u][i],v=f[v][i];
		}
	return max(ret,max(mx[v][0],mx[u][0]));
}
signed main(){
	n=gi();m=gi();
	for(int i=1;i<=m;i++)ed[i].u=gi(),ed[i].v=gi(),ed[i].w=gi(),ed[i].id=i;
	sort(ed+1,ed+m+1,cmp1);
	for(int i=1;i<=n;i++)fa[i]=i;
	for(int i=1;i<=m;i++){
		int u=find(ed[i].u),v=find(ed[i].v);
		if(u!=v){
			fa[v]=u;
			Add(ed[i].u,ed[i].v,ed[i].w);Add(ed[i].v,ed[i].u,ed[i].w);
			ans+=ed[i].w;
		}
	}
	dfs(1,1);
	for(int j=1;j<=18;j++)
		for(int i=1;i<=n;i++){
			f[i][j]=f[f[i][j-1]][j-1];
			mx[i][j]=max(mx[i][j-1],mx[f[i][j-1]][j-1]);
		}
	sort(ed+1,ed+m+1,cmp2);
	for(int i=1;i<=m;i++)
		printf("%lld\n",ans-getmax(ed[i].u,ed[i].v)+ed[i].w);
	return 0;
}
```

