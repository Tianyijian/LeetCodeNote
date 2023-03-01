# 排序

## 0912. Sort an Array

> :orange_circle:

给定整数数组，将数组升序排序并返回。要求不使用内置函数，O(nlogn)并尽可能少使用空间

### 方法一

* 归并排序 
	*  分治思想，将数组递归拆分为单个元素，然后向上归并两个有序数组，将归并的结果重新存入原数组
	*  归并时有in-place和out-place两种，前者O(n^2) TLE，后者最好最坏平均都是O(nlogn)。T: O(nlogn), S: O(n)

```cpp
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        mergeSort(nums, 0, nums.size() - 1);
        return nums;
    }
private:
    void mergeSort(vector<int>& nums, int low, int high) {
        if (low >= high) return;
        int mid = low + (high - low) / 2;
        mergeSort(nums, low, mid);
        mergeSort(nums, mid + 1, high);
        outPlaceMerge(nums, low, mid, high);
    }

    void outPlaceMerge(vector<int>& nums, int low, int mid, int high) {
        int l = low, r = mid + 1, k = 0, size = high - low + 1;
        vector<int> sorted(size, 0);
        while (l <= mid && r <= high) {
            sorted[k++] = nums[r] < nums[l] ? nums[r++] : nums[l++];
        }
        while (l <= mid) sorted[k++] = nums[l++];
        while (r <= high) sorted[k++] = nums[r++];
        for (int k = 0; k < size; k++) nums[k + low] = sorted[k];
    }
};
```

### 方法二

- 计数排序（CountingSort）
  - 非比较型排序算法，适用于数值范围小的数组。统计每个元素的频率，按照元素值及频率放置每个元素
  - 哈希表统计频率，找到最大最小值，放置每个元素。最好最坏平均都是O(n + k)，T: O(n + k), S: O(n), k是取值范围

```cpp
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        countingSort(nums);
        return nums;
    }
private:
    void countingSort(vector<int>& nums) {
        unordered_map<int, int> counts;
        int minVal = *min_element(nums.begin(), nums.end());
        int maxVal = *max_element(nums.begin(), nums.end());
        for (int& val : nums) counts[val]++;
        int index = 0;
        for (int i = minVal; i <= maxVal; i++) {
            int k = counts[i];
            while (k--) nums[index++] = i;
        }
    }
};
```

### 方法三

*  快速排序
  *  分治思想，平均O(nlogn)，最坏O(n^2)
  *  挑出一个元素作为基准（pivot）；单趟排序，比基准小的在前，基准大的在后，称为分区操作（partition）；递归把基准前的子序列和基准后的子序列排序。
  * 基准的选择
  	*  固定位置（最左或最右），随机位置，三数取中
  	*  理想情况，选择的基准恰好将序列分为两个等长子序列
  	*  固定位置时，完全有序的数组以及重复数组，每趟排序后，pivot的值在边缘，每层递归只能固定一个数，时间O(n^2)
  	* 三数取中：取最左，最右，中间的三个元素，值在中间的元素作为基准
  	* 随机位置比三数取中略差，两者都能解决有序数组问题，但不能解决重复数组问题
  * 三数取中+插排+聚集相等元素，性能基本与STL的sort函数差不多
*  本题采用随机位置快排，过不去重复数组的测试用例（TLE）

```cpp
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        quickSort(nums, 0, nums.size() - 1);
        return nums;
    }
private:
    void quickSort(vector<int>& nums, int low, int high) {
        if (low >= high) return;
        int pivot = partition(nums, low, high);
        quickSort(nums, low, pivot - 1);
        quickSort(nums, pivot + 1, high);
    }

    int partition(vector<int>& nums, int low, int high) {
        srand(time(nullptr));
        int pivot = rand() % (high - low + 1) + low;
        swap(nums[low], nums[pivot]);
        pivot = low;
        while (low < high) {
            while (low < high && nums[high] >= nums[pivot]) high--;
            while (low < high && nums[low] <= nums[pivot]) low++;
            swap(nums[high], nums[low]);
        }
        swap(nums[pivot], nums[low]);
        return low;
    }
};
```

## 0215. Kth Largest Element in an Array

> :orange_circle:

给定一维数组和整数K，返回数组中排好序的第K个最大的元素，注意不是第K个独特元素，要求O(n)以内

### 方法一

* 改进快速排序。快排降序，找到下标为k-1的元素。依据排好的下标与目标下标的大小，选择左右递归区间

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        quickSelect(nums, 0, nums.size() - 1, k - 1);
        return nums[k - 1];
    }
private:
    void quickSelect(vector<int>& nums, int low, int high, int target) {
        srand(time(nullptr));
        int pivot = rand() % (high - low + 1) + low;
        swap(nums[low], nums[pivot]);
        pivot = low;
        int index = low;
        for (int i = low + 1; i <= high; i++) {
            if (nums[i] >= nums[pivot]) {
                swap(nums[i], nums[index + 1]);
                index++;
            }
        }
        swap(nums[pivot], nums[index]);
        if (index > target) quickSelect(nums, low, index - 1, target);
        else if (index < target) quickSelect(nums, index + 1, high, target);
    }
};
```

### 方法二

- 堆排序，大顶堆：所有元素建立，弹出到第k个元素，O(nlogn)

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int> que(nums.begin(), nums.end());
        for (int i = 0; i < k - 1; i++) {
            que.pop();
        }
        return que.top();
    }
};
```
* 小顶堆：维护K个元素，greater<int>, O(nlogk)

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> que;
        for (int num : nums) {
            que.push(num);
            if (que.size() > k) que.pop();
        }
        return que.top();
    }
};
```

### 拓展

* 模板函数需要的比较参数是函数名，模板类需要的比较参数是类型名
  * 模板函数：sort, merge...；模板类：set, map, priority_queue...

```cpp
private:
static bool cmp(const int& a, const int& b) {
    return a > b;
}

class mycomparison {
    public:
    bool operator()(const int& lhs, const int& rhs) {
        return lhs > rhs;  
    }
};

// 内置关系仿函数
// tmplate<class T> bool greater<T>                    大于
// tmplate<class T> bool less<T>                       小于
// less<T> 是各模板函数和模板类的默认比较函数，从小到大排序

sort(nums.begin(), nums.end(), cmp);
sort(nums.begin(), nums.end(), greater<int>());
sort(nums.begin(), nums.end(), mycomparison());
set<int, greater<int>> s;
priority_queue<int, vector<int>> que; // 大顶堆，优先弹出最大的元素
priority_queue<int, vector<int>, greater<int>> que; //小顶堆，优先弹出最小的元素
priority_queue<int, vector<int>, mycomparison> que;
```

