## CF375D Tree and Queries

树上莫队板子题，求个$dfn$求个$low$就行了。

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
int front[N],cnt,bl[N],n,m,c[N],ans[N];
struct node{int to,nxt;}e[N<<1];
void Add(int u,int v){e[++cnt]=(node){v,front[u]};front[u]=cnt;}
struct point{
	int dfn,low,c;
	bool operator<(const point &b)const{return dfn<b.dfn;}
}a[N];
struct ques{
	int l,r,k,id;
	bool operator<(const ques &b)const{return bl[l]==bl[b.l]?r<b.r:l<b.l;}
}q[N];
int Time;
void dfs(int u,int ff){
	a[u].dfn=++Time;a[u].c=c[u];
	for(int i=front[u];i;i=e[i].nxt){
		int v=e[i].to;if(v==ff)continue;
		dfs(v,u);
	}
	a[u].low=Time;
}
int B,sum[N],tot[N];
void Add(int x){sum[++tot[x]]++;}
void Del(int x){sum[tot[x]--]--;}
int main(){
	n=gi();m=gi();B=sqrt(n);for(int i=1;i<=n;i++)c[i]=gi();
	for(int i=1;i<=n;i++)bl[i]=(i-1)/B+1;
	for(int i=1;i<n;i++){int u=gi(),v=gi();Add(u,v);Add(v,u);}
	dfs(1,1);
	for(int i=1;i<=m;i++){int u=gi(),k=gi();q[i].l=a[u].dfn;q[i].r=a[u].low;q[i].k=k;q[i].id=i;}
	sort(q+1,q+m+1);sort(a+1,a+n+1);
	for(int i=1,l=1,r=0;i<=m;i++){
		while(l<q[i].l)Del(a[l++].c);
		while(l>q[i].l)Add(a[--l].c);
		while(r<q[i].r)Add(a[++r].c);
		while(r>q[i].r)Del(a[r--].c);
		ans[q[i].id]=sum[q[i].k];
	}
	for(int i=1;i<=m;i++)printf("%d\n",ans[i]);
	return 0;
}
```

