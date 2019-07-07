## CF842E Nikita and game

考虑加入一个点的时候，如果它会让答案变优，一定是到任意直径上的端点。

接着我们仔细思考一下，直径相交部分之外显然在同一侧的长度相同对吧。

那么直接拿两个$vector$维护两边的点就好了，剩下的就是一些细节处理了。

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=300010;
#define ll long long
#define re register
#define file(a) freopen(a".in","r",stdin);freopen(a".out","w",stdout)
inline int gi(){
	int f=1,sum=0;char ch=getchar();
	while(ch>'9' || ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0' && ch<='9'){sum=(sum<<3)+(sum<<1)+ch-'0';ch=getchar();}
	return f*sum;
}
int f[N][22],n,d,dep[N];//20;
void Add(int u,int ff){
	f[u][0]=ff;dep[u]=dep[ff]+1;
	for(int i=1;i<=20;i++)
		f[u][i]=f[f[u][i-1]][i-1];
}
int getlca(int u,int v){
	if(dep[u]<dep[v])swap(u,v);
	for(int i=20;~i;i--)
		if(dep[u]-(1<<i)>=dep[v])u=f[u][i];
	if(u==v)return u;
	for(int i=20;~i;i--)
		if(f[u][i]!=f[v][i])
			u=f[u][i],v=f[v][i];
	return f[u][0];
}
int dist(int u,int v){int lca=getlca(u,v);return dep[u]+dep[v]-2*dep[lca];}
vector<int>s1,s2;
int main(){
	n=gi()+1;
	Add(1,1);s1.push_back(1);s1.reserve(n);s2.reserve(n);
	for(int i=2;i<=n;i++){
		Add(i,gi());
		int d1=s1.empty()?0:dist(s1[0],i);
		int d2=s2.empty()?0:dist(s2[0],i);
		if(max(d1,d2)>d){
			d=max(d1,d2);
			if(d1==d){
				for(auto u:s2)
					if(dist(u,i)==d1)
						s1.push_back(u);
				s2.clear();
			}
			else{
				for(auto u:s1)
					if(dist(u,i)==d2)
						s2.push_back(u);
				s1.clear();				
			}
		}
		if(max(d1,d2)==d)(d1==d?s2:s1).push_back(i);
		printf("%d\n",s1.size()+s2.size());
	}
}
```

