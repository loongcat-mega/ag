
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240423212037.png)

```cpp
//
// Created by dlut2102 on 2024/4/23.
//
#include<bits/stdc++.h>
using namespace std;
const int MAX_N=1e4+10;
//vector<vector<int>>dp(MAX_N,vector<int>(MAX_N));
//dp[i][j]代表的是 s1 [:i] 与 s2[:j]的最长公共子序列
int dp[MAX_N][MAX_N];
int main()
{
    int n;
    cin>>n;
    vector<int>s1(MAX_N);
    vector<int>s2(MAX_N);
    for(int i=0;i<n;i++)
        cin>>s1[i];
    for(int i=0;i<n;i++)
        cin>>s2[i];
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=n;j++)
        {
            if(s1[i-1]==s2[j-1])
                dp[i][j]=dp[i-1][j-1]+1;
            else
                dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
        
        }

    }
    cout<<dp[n][n]<<endl;
}
```



这样利用二维数组存储信息的话，有这些缺点：
- 内存占用比较大
- 不太好列出LCS

优化方向：利用一维滚动数组实现

