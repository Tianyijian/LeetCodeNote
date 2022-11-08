# 设计数据结构

## 146. LRU Cache
> :orange_circle:

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

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

