# 区间问题

## 0056. Merge Intervals

> :orange_circle:

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

## 0452. Minimum Number of Arrows to Burst Balloons

> :orange_circle:

2D整数数组points，`points[i] = [xstart, xend]`代表气球的横坐标跨度。弩箭垂直射出，打破`xstart <= x <= xend`的所有气球。求打破所有气球所需的最少弩箭

### 方法

- 将气球按照起始位置排序，当前气球的起始位置大于前一个气球的结束位置时，需要一只弩箭打破前一个气球。记录当前气球和前一个气球结束位置的最小值。T: O(nlogn), S: O(logn)

```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        int ans = 1;
        sort(points.begin(), points.end());
        for (int i = 1; i < points.size(); i++) {
            if (points[i][0] > points[i - 1][1]) {
                ans++;
            } else {
                points[i][1] = min(points[i][1], points[i - 1][1]);
            }
        }
        return ans;
    }
};
```

