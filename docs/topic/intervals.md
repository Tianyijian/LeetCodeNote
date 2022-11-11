# 区间问题

## 0056. Merge Intervals

> :orange_ciircle:

给定区间数组，合并其中的重叠区间，返回合并后的区间，其是不重叠的且覆盖所有的原始区间

### 方法

- 将区间按照起始位置排序，从左至右遍历，当前与上一个没有重叠，将上一个加入结果；有重叠，修改当前区间为两者最大区间

```cpp
class Solution {
private:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        return a[0] < b[0];
    } 
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if (intervals.size() == 1) return intervals;
        sort(intervals.begin(), intervals.end(), cmp);
        vector<vector<int>> res;
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] > intervals[i - 1][1]) {
                res.push_back(intervals[i - 1]);
            } else {
                intervals[i][1] = max(intervals[i][1], intervals[i - 1][1]);
                intervals[i][0] = min(intervals[i][0], intervals[i - 1][0]);
            }
        }
        res.push_back(intervals[intervals.size() - 1]);
        return res;
    }
};
```

