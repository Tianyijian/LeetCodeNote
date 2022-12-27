# Weekly Contest 325
> 2022.12.25

## 2515. Shortest Distance to Target String in a Circular Array
> :green_circle:

给一个环形字符串数组`words`和字符串`target`，`words[i]`的下一个元素是`words[(i+1)%n]`，上一个元素是`words[(i-1+n)%n]`。从`startIndex`出发，返回到达`target`的最短距离，若`target`不存在返回-1

### 方法

- 遍历走的步数，如果到达`target`，记录移动的距离，以及反向移动的距离。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int closetTarget(vector<string>& words, string target, int startIndex) {
        int n = words.size();
        int ans = INT_MAX;
        for (int i = 0; i < n; i++) {
            int j = (i + startIndex) % n;
            if (words[j] == target) {
                ans = min(ans, i);
                ans = min(ans, n - i);
            }
        }
        return ans == INT_MAX ? -1 : ans;
    }
};
```

