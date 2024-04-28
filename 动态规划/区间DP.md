**题面：** `N(<=100)`堆石子摆成一条线，要将石子合并，每次只能选相邻的两堆合并成新的一堆，并将新堆的石子数记为该次操作的代价，求将所有堆合并为一堆的最小代价。


```text
状态:
f[L,R]表示把从L合并到R合并成一堆的最小代价

先把区间[L,R]切分为两部分[L,K],[K+1,R]，K是切分点
再把两部分合并到一起(代价prefix[R]-prefix[L-1])

转移：
f[L,R]=min(f[L,R],f[L,K]+f[K+1,R]+prefix[R]-prefix[L-1])


初值：
f[i,i]=0 

目标
f[1,n]
```

```cpp
//
// Created by dlut2102 on 2024/4/28.
//
#include<bits/stdc++.h>
using namespace std;
const int MAX_N=100;
int arr[MAX_N];
int prefix[MAX_N];
int f[MAX_N][MAX_N];
int main()
{
    int n;
    cin>>n;
    //初始化
    memset(f,0x3f,sizeof(f));
    for(int i=1;i<=n;i++)
    {
        cin>>arr[i];
        prefix[i]=prefix[i-1]+arr[i];
        f[i][i]=0;
    }
    //枚举区间长度
    for(int len=2;len<=n;len++)
    {
        //枚举区间左端点
        for(int l=1;l+len-1<=n;l++)
        {
            //区间右端点
            int r=l+len-1;
            //枚举分割点
            for(int k=l;k<r;k++)
            {
                f[l][r]=min(f[l][r],f[l][k]+f[k+1][r]+prefix[r]-prefix[l-1]);
                        
            }
        }
    }
    cout<<f[1][n]<<endl;
    
    return 0;
}
```

