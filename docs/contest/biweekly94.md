# Biweekly Contest 94
> 2022.12.24

## 2511. Maximum Enemy Forts That Can Be Captured

> :green_circle:

长度为n的整数数组代表堡垒的位置，-1空堡垒，0是敌人堡垒，1是我方堡垒。从我方堡垒移动军队到达空堡垒，途中只能路过敌人堡垒，路过的敌人堡垒被攻占。返回可以被攻占的最大敌人堡垒数量

### 方法

- 暴力：遍历我方堡垒，向两侧寻找敌人堡垒，直至遇到空堡垒，记录最大的堡垒数量
- 一遍遍历：计算1和-1之间的0的数量。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int captureForts(vector<int>& forts) {
        int ans = 0, j = 0;
        for (int i = 0; i < forts.size(); i++) {
            if (forts[i] != 0) {
                if (forts[i] == -forts[j]) {
                    ans = max(ans, i - j - 1);
                }
                j = i;
            }
        }
        return ans;
    }
};
```

## 2512. Reward Top K Students

> :orange_circle:

给两个字符串数组代表积极和消极词语，每个积极词语+3分，每个消极词语-1分。给一串学生ID以及评语，找到评分最高的k个学生。评分相同时，ID低的优先。

### 方法

- 将积极和消极词语转化为哈希表便于查找，根据每个学生的评语计算得分，排序得到Top k
- 注意如何从空格分割的单词串中取出单词

```cpp
class MyCmp {
public:
    bool operator()(pair<int, int>& a, pair<int, int>& b) {
        if (a.first == b.first) return a.second < b.second;
        return a.first > b.first;
    }
};

class Solution {
public:
    vector<int> topStudents(vector<string>& positive_feedback, vector<string>& negative_feedback, vector<string>& report, vector<int>& student_id, int k) {
        unordered_set<string> pos(begin(positive_feedback), end(positive_feedback));
        unordered_set<string> neg(begin(negative_feedback), end(negative_feedback));
        priority_queue<pair<int, int>, vector<pair<int, int>>, MyCmp> minHeap;
        for (int i = 0; i < report.size(); i++) {
            int point = 0;
            int j = 0;
            for (int k = 0; k <= report[i].size(); k++) {
                if (k == report[i].size() || report[i][k] == ' ') {
                    string w = report[i].substr(j, k - j);
                    if (pos.count(w)) point += 3;
                    else if (neg.count(w)) point -= 1;
                    j = k + 1;
                }
            }
            minHeap.push({point, student_id[i]});
            if (minHeap.size() > k) minHeap.pop();
        }
        vector<int> ans(k);
        while (k--) {
            ans[k] = minHeap.top().second;
            minHeap.pop();
        }
        return ans;
    }
};
```

## 2513. Minimize the Maximum of Two Arrays

> :orange_circle:

### 方法

## 2514. Count Anagrams

> :red_circle:

### 方法
