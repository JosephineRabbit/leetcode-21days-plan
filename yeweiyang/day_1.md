## 605.Can Place Flowers
1. 这道题比较简单，是使用贪心算法的一道题。
2. 其中力扣官方的题解对于这道题来说并不是最佳的解法，但解题思路值得学习。
3. 笔者采用的是从左到右依次遍历，符合条件就种植，并且使用一个容器flowered_temp去更新种下该朵花之后的情况。
4. 题解中一个大神的“奇思妙解”我认为是最巧妙也最直接的一种解法，不过并不具有普适性。

下面仅copy这位大神的解法作为参考：

**首先这里我用的是连跳两格的方法，因为如果遇到1,那么下一格子一定是0，这是毋庸置疑的（规则限定），所以如果遇到最后一个格子，或者下个格子不是1，果断填充**

```cpp
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        // 每次跳两格
         for (int i = 0; i < flowerbed.size(); i += 2) {
             // 如果当前为空地
            if (flowerbed[i] == 0) {
                // 如果是最后一格或者下一格为空
                if (i == flowerbed.size() - 1 || flowerbed[i + 1] == 0) {
                    n--;
                } else {
                    i++;
                }
            }
        }
        return n <= 0;
    }
};
```

## 452.Minimum Number of Arrows to Burst Balloons(Medium)
这道题的解题思路比较容易想到，但做的过程中出错的原因是，我初始化弓箭数量ans=0。循环中没有用STL的范围for循环，还是用的基于vector二维数组的普通循环，按照右边界开始遍历之后，从左到右依次开始找共同区间，一直到找不到为止，然后更新弓箭数量，将数组顺移。当最后一只弓箭可以射掉最后一个气球时。循环结束之后判断条件没有处理好。如果弓箭数量初始化为1，就会减少一次for循环不满足的判断。

1.知识点弥补：
- 基于范围的for循环(C++11)
基于范围的for循环是为用于STL而设计的，
```cpp
double prices[5] = {4.99,10.99,6.87,7.99,8.49}
for(double x:prices)
    cout<<x<<std::endl;
```
在这种for循环，括号内的代码声明一个类型与容器存储的内容相同的变量，然后指出了容器的名称。接下来，循环体使用指定的变量依次访问容器的每个元素。
不同于for_each()，基于范围的for循环课修改容器的内容。

2. 附leetcod官方题解：
```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        if (points.empty()) {
            return 0;
        }
        sort(points.begin(), points.end(), [](const vector<int>& u, const vector<int>& v) {
            return u[1] < v[1];
        });
        int pos = points[0][1];
        int ans = 1;
        for (const vector<int>& balloon: points) {
            if (balloon[0] > pos) {
                pos = balloon[1];
                ++ans;
            }
        }
        return ans;
    }
};
```
