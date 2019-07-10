## AT2306 Rearranging

有一个显然的，就是不互质的数的相对位置是不会改变的，那么我们把它们放到一个连通块里面去，然后我交换就是交换两个里面最小的对吧。直接连起来然后跑$TopSort$就行了。

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
const int N=2010;
int a[N],link[N][N],vis[N],d[N],n;
vector<int>G[N];
void dfs(int u){
	vis[u]=1;
	for(int i=1;i<=n;i++)
		if(link[u][i] && !vis[i]){
			d[i]++;
			G[u].push_back(i);
			dfs(i);
		}
}
priority_queue<int>Q;
int main(){
	n=gi();for(int i=1;i<=n;i++)a[i]=gi();
	sort(a+1,a+n+1);
	for(int i=1;i<n;i++)
		for(int j=i+1;j<=n;j++)
			if(__gcd(a[i],a[j])!=1)link[i][j]=link[j][i]=1;
	for(int i=1;i<=n;i++)if(!vis[i])dfs(i);
	for(int i=1;i<=n;i++)if(!d[i])Q.push(i);
	while(!Q.empty()){
		int u=Q.top();Q.pop();
		printf("%d ",a[u]);
		for(auto v:G[u])Q.push(v);
	}
	puts("");
	return 0;
}
```

