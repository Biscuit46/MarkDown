## CF741D Arpa’s letter-marked tree and Mehrdad’s Dokhtar-kosh paths

重新排列后组成回文串意味着路径上出现奇数次的最多1个，那么可以$dsu\ on\ tree$搞一下了。。。

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
#define file(a) freopen(a".in","r",stdin);freopen(a".out","w",stdout)
inline int gi(){
	int f=1,sum=0;char ch=getchar();
	while(ch>'9' || ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0' && ch<='9'){sum=(sum<<3)+(sum<<1)+ch-'0';ch=getchar();}
	return f*sum;
}
const int N=500010;
int n,dep[N],siz[N],son[N],front[N],cnt,dfn[N],Time,low[N],dis[N],id[N],ans[N],mx,f[1<<22];
struct node{int to,nxt,w;}e[N];
void Add(int u,int v,int w){e[++cnt]=(node){v,front[u],w};front[u]=cnt;}
void dfs(int u,int ff){
	id[dfn[u]=++Time]=u;
	siz[u]=1;dep[u]=dep[ff]+1;
	for(int i=front[u];i;i=e[i].nxt){
		int v=e[i].to;dis[v]=dis[u]^e[i].w;
		dfs(v,u);siz[u]+=siz[v];
		if(siz[v]>siz[son[u]])son[u]=v;
	}
	low[u]=Time;
}
void dfs(int u,int ff,int kp){
	for(int i=front[u];i;i=e[i].nxt){
		int v=e[i].to;if(v==son[u])continue;
		dfs(v,u,0);ans[u]=max(ans[u],ans[v]);
	}
	if(son[u])dfs(son[u],u,1),ans[u]=max(ans[u],ans[son[u]]);
	if(f[dis[u]])ans[u]=max(ans[u],f[dis[u]]-dep[u]);
	for(int i=0;i<22;i++)
		if(f[dis[u]^(1<<i)])ans[u]=max(ans[u],f[dis[u]^(1<<i)]-dep[u]);
	f[dis[u]]=max(f[dis[u]],dep[u]);
	for(int E=front[u];E;E=e[E].nxt){
		int v=e[E].to;if(v==son[u])continue;
		for(int i=dfn[v];i<=low[v];i++){
			if(f[dis[id[i]]])ans[u]=max(ans[u],f[dis[id[i]]]+dep[id[i]]-(dep[u]<<1));
			for(int j=0;j<22;j++)
				if(f[dis[id[i]]^(1<<j)])
					ans[u]=max(ans[u],f[dis[id[i]]^(1<<j)]+dep[id[i]]-(dep[u]<<1));
		}
		for(int i=dfn[v];i<=low[v];i++)
			f[dis[id[i]]]=max(f[dis[id[i]]],dep[id[i]]);
	}
	if(!kp)for(int i=dfn[u];i<=low[u];i++)f[dis[id[i]]]=0;
}
int main(){
	n=gi();
	for(int i=2;i<=n;i++){
		int F=gi();char ch=getchar();while(ch<'a' || ch>'v')ch=getchar();
		Add(F,i,1<<(ch-'a'));
	}
	dfs(1,1);dfs(1,1,1);
	for(int i=1;i<=n;i++)printf("%d%c",ans[i],i==n?'\n':' ');
	return 0;
}
```

