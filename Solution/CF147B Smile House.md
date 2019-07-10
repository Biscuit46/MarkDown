## CF147B Smile House

倍增$floyd$,然后二进制贪心选答案。

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
const int N=310,Log=10,L=12,Inf=1e9+10;
int n,m;
struct matrix{
    int a[N][N];
    void init(){
        for(int i=1;i<=n;i++)
            for(int j=1;j<=n;j++)
                a[i][j]=-Inf;
    }
    matrix operator*(const matrix &b)const{
        matrix c;c.init();
        for(int k=1;k<=n;k++)
            for(int i=1;i<=n;i++)
                for(int j=1;j<=n;j++)
                    c.a[i][j]=max(c.a[i][j],a[i][k]+b.a[k][j]);
        return c;
    }
}T[L];
int main(){
    n=gi();m=gi();T[0].init();
    for(int i=1;i<=n;i++)T[0].a[i][i]=0;
    for(int i=1;i<=m;i++){
        int u=gi(),v=gi();
        T[0].a[u][v]=gi();T[0].a[v][u]=gi();
    }
    for(int i=1;i<Log;i++)T[i]=T[i-1]*T[i-1];
    matrix now,o;now.init();o.init();
    for(int i=1;i<=n;i++)now.a[i][i]=0;
    int ans=0;
    for(int i=Log-1;~i;i--){
        o=now*T[i];int flag=0;
        for(int j=1;j<=n;j++)if(o.a[j][j]>0){flag=1;break;}
        if(flag)continue;
        ans+=(1<<i),now=o;
    }
    if(ans>n)puts("0");
    else printf("%d\n",ans+1);
    return 0;
}

```

~~吐槽：zzy是真的麻瓜~~