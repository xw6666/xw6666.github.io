---
title: 阻挡广告牌II
date: 2022-03-30 23:56:23
tags: 算法
categories: 每日一题
---

# 阻挡广告牌II

![](/images/阻挡广告牌2/题目.png)

思路：直接找能够让覆盖面积减少的条件，不难发现只有以下五种：

- ![](/images/阻挡广告牌2/QQ图片20220330214941.png)

- ![](/images/阻挡广告牌2/QQ截图20220329232743.png)
- ![](/images/阻挡广告牌2/QQ截图20220330215114.png)
- ![](/images/阻挡广告牌2/QQ截图20220330215128.png)
- ![](/images/阻挡广告牌2/QQ截图20220330215151.png)

所以分类讨论直接代码实现：

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cmath>
using namespace std;

int main()
{
    int x1,y1,x2,y2;
    cin >> x1 >> y1 >> x2 >> y2;
    int s = abs(x1 - x2) * abs(y1 - y2);
    
    int x3,y3,x4,y4;
    cin >> x3 >> y3 >> x4 >> y4;
    
    if(x3 <= x1 && x4 >= x2 && y3 <= y1 && y4 >= y2) s = 0;
    else if(x1 >= x3 && x2 <= x4 && y1 <= y4 && y4 <= y2 && y3 <= y1) s = abs(y2 - y4) * abs(x1 - x2);
    else if(x1 >= x3 && x2 <= x4 && y1 <= y3 && y3 <= y2 && y4 >= y2) s = abs(y3 - y1) * abs(x1 - x2);
    else if(y1 >= y3 && y2 <= y4 && x1 <= x4 && x4 <= x2 && x1 >= x3) s = abs(x4 - x2) * abs(y1 - y2);
    else if(y1 >= y3 && y2 <= y4 && x1 <= x3 && x3 <= x2 && x2 <= x4) s = abs(x3 - x1) * abs(y1 - y2);
    
    cout << s << endl;
    
    return 0;
}
```

