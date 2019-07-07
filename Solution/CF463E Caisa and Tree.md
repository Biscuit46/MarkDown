## CF463E Caisa and Tree

~~要是你们能和我一样看错题目意思误认为是要求互质的就舒服了。~~

考虑修改很少，所以修改完之后可以暴力遍历树。

那么现在问题转换成了如何求一个点的答案，直接把所有质因子存下来然后用$set$维护即可。

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
int front[N],cnt,n;
struct node{int to,nxt;}e[N<<1];
void Add(int u,int v){e[++cnt]=(node){v,front[u]};front[u]=cnt;}
typedef pair<int,int> pii;
vector<int>divi[N];
pii ans[N];
set<pii>se[2000010];
void get(int u,int k){
	divi[u].clear();
	for(int i=2;i*i<=k;i++)
		if(k%i==0){
			while(k%i==0)k/=i;
			divi[u].push_back(i);
		}
	if(k!=1)divi[u].push_back(k);
}
#define mp make_pair
int dep[N];
void dfs(int u,int ff){
	ans[u]=mp(-1,-1);dep[u]=dep[ff]+1;
	for(int i=0;i<divi[u].size();i++){
		int s=divi[u][i];
		if(se[s].size())ans[u]=max(ans[u],*se[s].rbegin());
		se[s].insert(mp(dep[u],u));
	}
	for(int i=front[u];i;i=e[i].nxt){
		int v=e[i].to;if(v==ff)continue;
		dfs(v,u);
	}
	for(int i=0;i<divi[u].size();i++){
		int s=divi[u][i];
		se[s].erase(mp(dep[u],u));
	}
}
int main(){
	n=gi();int Q=gi();
	for(int i=1;i<=n;i++)get(i,gi());
	for(int i=1;i<n;i++){int u=gi(),v=gi();Add(u,v);Add(v,u);}
	dfs(1,1);
	while(Q--){
		int opt=gi();
		if(opt==1) printf("%d\n",ans[gi()].second);
		else{
			int v=gi(),w=gi();
			get(v,w);
			dfs(1,1);
		}
	}
	return 0;
}
```

