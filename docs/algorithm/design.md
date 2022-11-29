# 设计数据结构

## 0146. LRU Cache
> :orange_circle:

设计数据结构实现LRU缓存

### 思路

- LRU缓存机制，最近最少使用策略
- get方法：不存在返回-1；存在，将其设为最近使用，返回值
- put方法：存在key，设为最近使用，更新val；不存在key，cache满删除最近最少使用，加为最近使用
- 查找要求O(1)，采用哈希表map。要维护使用顺序，采用链表，修改删除也尽量O(1)，采用双向链表

### 方法一

- list<pair<int, int>> 存储key和value，并且最新信息在头节点
- unordered_map<int, list<pair<int, int>>::iterator>进行快速搜索，存迭代器是方便调用链表的splice函数将迭代器对应的节点移动到链表的头节点
- list.splice(iterator position, list2, iterator first, iterator last)函数：将list2的first和last之间的元素拼接到list的position位置
  - list.splice(list.begin(), list, it): 将it位置的元素移动到list的表头
- map.find(key) 返回指向该元素的map迭代器，不存在则为map.end()

```cpp
class LRUCache {
public:
    LRUCache(int capacity) {
        ios_base::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
        cap = capacity;
    }
    
    int get(int key) {
        if (map.find(key) == map.end()) return -1;
        cache.splice(cache.begin(), cache, map[key]);
        map[key] = cache.begin();
        return map[key]->second;
    }
    
    void put(int key, int value) {
        if (map.find(key) != map.end()) {
            map[key]->second = value;
            cache.splice(cache.begin(), cache, map[key]);
            map[key] = cache.begin();
        } else {
            if (cache.size() == cap) {
                map.erase(cache.back().first);
                cache.pop_back();
            }
            // cache.insert(cache.begin(), make_pair(key, value));
            cache.push_front(make_pair(key, value));
            map[key] = cache.begin();
        }
    }

private:
    list<pair<int, int>> cache;
    unordered_map<int, list<pair<int, int>>::iterator> map;
    int cap;
};
```

### 方法二

自己定义双向列表的节点，实现删除尾节点、向头节点添加元素、移动元素到头节点等函数

```cpp
struct Node {
    int key;
    int value;
    Node* prev;
    Node* next;
    Node() : key(0), value(0), prev(nullptr), next(nullptr) {}
    Node(int key, int value) : key(key), value(value), prev(nullptr), next(nullptr) {}
};

class LRUCache {
public:
    LRUCache(int capacity) {
        cap = capacity;
        head = new Node();
        tail = new Node();
        head->next = tail;
        tail->prev = head;
    }

    int get(int key) {
        if (map.find(key) == map.end()) return -1;
        moveToHead(map[key]);
        return map[key]->value;
    }

    void put(int key, int value) {
        if (map.find(key) != map.end()) {
            Node* node = map[key];
            node->value = value;
            moveToHead(node);
        } else {
            if (map.size() == cap) {
                Node* node = removeTail();
                map.erase(node->key);
            }
            Node* node = new Node(key, value);
            map[key] = node;
            addToHead(node);
        }
    }

private:
    unordered_map<int, Node*> map;
    Node* head;
    Node* tail;
    int cap;

    void addToHead(Node* node) {
        node->next = head->next;
        node->prev = head;
        head->next->prev = node;
        head->next = node;
    }

    void removeNode(Node* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    void moveToHead(Node* node) {
        removeNode(node);
        addToHead(node);
    }

    Node* removeTail() {
        Node* node = tail->prev;
        removeNode(node);
        return node;
    }
};
```

## 0380. Insert Delete GetRandom O(1)

> :orange_circle:

实现`RandomizedSet`类，`bool insert(int val)`：值存在时返回false，不存在将其插入。`bool remove(int val)`：值不存在返回fasle，存在将其删除。`int getRandom()`：以相等概率随机返回一个值。每个函数都平均O(1)时间复杂度

### 方法

- 数组可以O(1)插入但查找为O(n)，删除元素需要先查找O(n)，删除后向左移动后续元素也需要O(n)，调用rand()函数返回随机值为O(1)
- 额外采用map，记录每个值的下标，则查找为O(1)。删除时将待删除元素与数组最后一个元素交换，删除最后一个元素即可O(1)，无需移动
- emplace_back() 相比push_back()函数，在用户自定义对象时，插入更高效

```cpp
class RandomizedSet {
public:
    RandomizedSet() {   
    }
    bool insert(int val) {
        if (map.find(val) != map.end()) return false;
        map[val] = nums.size();
        nums.push_back(val); 
        return true;
    }
    bool remove(int val) {
        if (map.find(val) == map.end()) return false;
        int valId = map[val];
        nums[valId] = nums.back();
        map[nums.back()] = valId;
        nums.pop_back();
        map.erase(val);
        return true;
    }
    int getRandom() {
        return nums[rand() % nums.size()];
    }
private:
    vector<int> nums;
    unordered_map<int, int> map;
};
```

