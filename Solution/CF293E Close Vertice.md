## CF293E Close Vertice

如果没有边数限制就是裸的淀粉质，如果有了加上一个树状数组记边数就行了。

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
const int N=400010;
int n,w,front[N],cnt,rt,siz[N],vis[N],tot,f[N];
struct node{int to,nxt,w;}e[N];
void Add(int u,int v,int w){e[++cnt]=(node){v,front[u],w};front[u]=cnt;}
void getroot(int u,int ff){
	siz[u]=1;f[u]=0;
	for(int i=front[u];i;i=e[i].nxt){
		int v=e[i].to;if(vis[v] || v==ff)continue;
		getroot(v,u);
		siz[u]+=siz[v];
		f[u]=max(f[u],siz[v]);
	}
	f[u]=max(f[u],tot-siz[u]);
	if(f[u]<f[rt])rt=u;
}
ll ans;int dis[N],dep[N];
struct thing{
	int dis,dep;
	bool operator<(const thing &b)const{return dis<b.dis;}
}p[N];
void getdis(int u,int ff){
	p[++cnt]=(thing){dis[u],dep[u]};
	for(int i=front[u];i;i=e[i].nxt){
		int v=e[i].to;if(v==ff || vis[v])continue;
		dep[v]=dep[u]+1;dis[v]=dis[u]+e[i].w;
		getdis(v,u);
	}
}
int c[N],L;
int lowbit(int x){return x&(-x);}
void Add(int x,int d){while(x<=n+1){c[x]+=d;x+=lowbit(x);}}
int query(int x){if(x<=0)return 0;int ret=0;while(x){ret+=c[x];x-=lowbit(x);}return ret;}
ll calc(int u,int ds,int dp){
	dis[u]=ds;dep[u]=dp;cnt=0;
	getdis(u,u);
	sort(p+1,p+cnt+1);
	for(int i=1;i<=cnt;i++)Add(p[i].dep+1,1);
	int l=1,r=cnt;ll ret=0;
	while(l<r){
		if(p[l].dis+p[r].dis<=w){
			Add(p[l].dep+1,-1);
			ret+=query(L-p[l].dep+1);
			l++;
		}
		else{
			Add(p[r].dep+1,-1);
			r--;
		}
	}
	Add(p[l].dep+1,-1);
	return ret;
}
void solve(int u){
	ans+=calc(u,0,0);
	vis[u]=1;
	for(int i=front[u];i;i=e[i].nxt){
		int v=e[i].to;if(vis[v])continue;
		ans-=calc(v,e[i].w,1);
		rt=0;tot=siz[v];
		getroot(v,v);
		solve(rt);
	}
}
int main(){
	n=gi();L=gi();w=gi();
	for(int i=2;i<=n;i++){int fa=gi(),v=gi();Add(fa,i,v);Add(i,fa,v);}
	tot=f[0]=n;
	getroot(1,0);
	solve(rt);
	printf("%lld\n",ans);
	return 0;
}
```

