
这里要求子串必须是连续的

核心思想：
- 如果结尾相等，那么LCS++
- 如果结尾不相等，那么直接清零，因为要求是连续的
```cpp
//
// Created by dlut2102 on 2024/4/23.
//
#include<bits/stdc++.h>
using namespace std;
const int MAX_N=1e3+10;
//dp[i][j]代表的是以 a[i] 和 b[j] 结尾的公共子串的长度
int dp[MAX_N][MAX_N];
int main()
{
    string s1,s2;
    getline(cin,s1);
    getline(cin,s2);
    int ans=0;
    for(int i=1;i<=s1.size();i++)
        for(int j=1;j<=s2.size();j++)
        {
            if(s1[i-1]==s2[j-1])
            {
                dp[i][j]=dp[i-1][j-1]+1;
                ans=max(ans,dp[i][j]);
            }
            else
                dp[i][j]=0;
        }
    cout<<ans;
}
```

