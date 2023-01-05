---
title: 社交距离I
date: 2022-03-23 23:26:57
tags: 算法
categories: 每日一题
---

# 社交距离I

![](/images/社交距离1/QQ截图20220329232743.png)

思路：我们可以通过二分答案来判断此答案下是否能放下两头牛

具体看代码注释

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;
const int N = 1e5 + 10, INF = 0x3f3f3f3f;

int a[N];
char s[N];    
int ne[N];
int n;

bool check(int mid)
{
    //遍历一遍，如果当前位置是x，则x前面有牛的位置和x距离要大于等于mid，后面有牛的位置与x距离要大于等于mid就能放下一头牛
    int cnt = 0, last = -INF;
    for(int i = 1;i <= n;i++)
    {
        if(a[i] == 0 && i - last >= mid && ne[i] - i >= mid)
        {
            cnt++;    //每放一头牛cnt++
            last = i;
        }
        
        if(cnt == 2) return true;   //能放两头就是合格
        if(a[i] == 1) last = i;     //不断更新上一头牛的坐标
    }
    
    return 0;
}

int main()
{
    cin >> n >> s + 1;
    memset(ne,0x3f,sizeof(ne));
    int d = n, last = -INF;
    for(int i = 1;i <= n;i++)
    {
        a[i] = s[i] - '0';
        if(a[i] == 1)    //如果有牛，检查一下能不能更新d，并记录这个牛的位置
        {
            d = min(d, i - last);
            last = i;
        }
    }
    
    //ne[i]表示i处的下一个有牛的位置是ne[i]
    for(int i = n;i >= 1;i--)
    {
        if(a[i] == 0)
        {
            if(a[i + 1] == 1) ne[i] = i + 1;
            else ne[i] = ne[i + 1];
        }
    }
    
    int l = 1, r = d;    //对最小距离1和现在距离d进行二分答案
    while(l < r)
    {
        int mid = l + r + 1 >> 1;
        if(check(mid)) l = mid;
        else r = mid - 1;
    }
    
    cout << l << endl;
    
    return 0;
}
```

