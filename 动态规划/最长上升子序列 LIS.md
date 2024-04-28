
## 朴素循环

```cpp
//
// Created by dlut2102 on 2024/4/23.
//
#include<bits/stdc++.h>
using namespace std;
const int MAX_N=5000+10;
//表示以f[i]表示以a[i]结尾的LIS长度
//初始条件
vector<int>f(MAX_N,1);
vector<int>a(MAX_N);
int main()
{
    int n;
    cin>>n;
    for(int i=0;i<n;i++)
        cin>>a[i];
    int ans=0;
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<i;j++)
            if(a[j]<a[i])
            		//状态转移方程
                f[i]=max(f[j]+1,f[i]);
        ans=max(f[i],ans);
    }
        
    cout<<ans;
}
```
n方数量级

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240423195844.png)

输出最长上升子序列：

```cpp
//
// Created by dlut2102 on 2024/4/23.
//
#include<bits/stdc++.h>
using namespace std;
const int MAX_N=5000+10;
//表示以f[i]表示以a[i]结尾的LIS长度
vector<int>f(MAX_N,1);
vector<int>a(MAX_N);
//需要记录每个最长上升子序列的前缀元素
vector<int>pre(MAX_N,-1);
int main()
{
    int n;
    cin>>n;
    for(int i=0;i<n;i++)
        cin>>a[i];
    int ans=0,end=0;
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<i;j++)
            if(a[j]<a[i]&&f[i]<f[j]+1)
            {
                f[i]=f[j]+1;
                pre[i]=j;
            }
        if(f[i]>ans)
        {
            ans=f[i];
            end=i;
        }
    }
    
    while(pre[end]!=-1)
    {
        cout<<a[end]<<" ";
        end=pre[end];
    }
    cout<<a[end]<<" ";
    
    cout<<endl;
    cout<<ans;
}
```

## 二分优化

内层循环是比较耗时的，如果内层是一个二分查找的话，就可以优化成nlogn级别，但是二分的前提是数组有序，如果让数组有序？新增一个储存上升子序列的数组
- 大则直接入队
- 小则替换掉第一个大于等于该元素的元素

```cpp
//
// Created by dlut2102 on 2024/4/23.
//
#include<bits/stdc++.h>
using namespace std;
const int MAX_N=5000+10;
//表示以f[i]表示以a[i]结尾的LIS长度
vector<int>lis(MAX_N);
//第一个大于等于k的元素位置
int gtk(vector<int>arr,int n,int k)
{
    if(k>arr[n-1])
        return -1;
    int l=0,r=n-1,m;
    while(l<=r)
    {
        m=(l+r)>>1;
        if(k>arr[m]) l=m+1;
        else r=m-1;
    }
    return l;
}
int main()
{
    int n,t,len=0;
    cin>>n;
    for(int i=0;i<n;i++)
    {
        if(i==0)
            cin>>lis[len++];
        else
        {
            cin>>t;
            if(t>lis[len-1])
                lis[len++]=t;
            else
            {
                int idx=gtk(lis,len,t);
                lis[idx]=t;
            }
        }
    }
    cout<<len;
    
  
}
```