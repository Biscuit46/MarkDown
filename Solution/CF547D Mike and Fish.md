## CF547D Mike and Fish

构造题,3000的题，假的。

考虑$x$相等的和$y$相等的直接匹配，剩下来的不用管，然后跑二分图匹配就行了。

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
const int N=200010;
int col[N],n,lu[N],lv[N],front[N],cnt;
struct node{int to,nxt;}e[N<<1];
void Add(int u,int v){
	e[++cnt]=(node){v,front[u]};front[u]=cnt;	
}
void dfs(int u,int C){
	col[u]=C;
	for(int i=front[u];i;i=e[i].nxt){
		int v=e[i].to;
		if(col[v]==-1)dfs(v,C^1);
	}
}
int main(){
	n=gi();for(int i=1;i<=n;i++)col[i]=-1;
	for(int i=1;i<=n;i++){
		int x=gi(),y=gi();
		if(lu[x]){Add(lu[x],i);Add(i,lu[x]);lu[x]=0;}
		else lu[x]=i;
		if(lv[y]){Add(lv[y],i);Add(i,lv[y]);lv[y]=0;}
		else lv[y]=i;
	}
	for(int i=1;i<=n;i++)if(col[i]==-1)dfs(i,0);
	for(int i=1;i<=n;i++)printf("%c",col[i]?'r':'b');
	puts("");
	return 0;
}
```

