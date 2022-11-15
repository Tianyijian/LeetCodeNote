# 二叉树

## 0222. Count Complete Tree Nodes

> :orange_circle:

统计完全二叉树的节点个数，要求时间复杂度少于O(n)

### 方法一

- 当做普通二叉树
  * 递归法，后序遍历。时间O(n)，空间O(logn)
  * 迭代法，层序遍历，求出节点数目总和 。时间O(n)，空间O(n)

```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if (root == NULL) return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```

### 方法二

* 完全二叉树性质
  * 底层第h层有1~2^h-1个节点，且靠左
  * 两种情况：满二叉树（2^树深-1），未满。情况2可以递归左右子树，直至有满二叉树
  * 判断满二叉树：O(logn) 递归向左右遍历，两边深度一样
  * 时间O(logn x logn)，空间O(logn)

```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if (root == NULL) return 0;
        TreeNode* left = root->left;
        TreeNode* right = root->right;
        int level = 0;
        while (left && right) {
            left = left->left;
            right = right->right;
            level++;
        }
        if (!left && !right) return pow(2, level + 1) - 1;
        return countNodes(root->left) + countNodes(root->right) + 1;
    }
};
```

## 0236. Lowest Common Ancestor of a Binary Tree

> :orange_circle:

给定二叉树，返回树中两个节点的最近公共祖先（lowest common ancestor, LCA）

### 方法

* 从底向上遍历：二叉树后序遍历可实现（回溯）
* 找到一个节点，左子树有节点p或q，右子树有节点q或p，找到即返回，否则返回空
* 层层返回找到的节点

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == NULL || root == p || root == q) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if (left && right) return root;
        if (left == NULL) return right;
        return left; 
    }
};
```

## 0124. Binary Tree Maximum Path Sum

> :red_circle:

给一个二叉树，返回最大的路径和。二叉树中的路径是相连的一系列节点（每个节点只出现一次，不一定包含根节点）

### 方法一

*  递归
	*  树形DP其实包含的有递归思想，不显式定义DP数组
	*  到达一个节点时，选择如下：加左子树或不加，加右子树或不加，左右都加是否能打败最大值
	*  返回能继续向上的值
```cpp
class Solution {
public:
    int maxPathSum(TreeNode* root) {
        int res = INT_MIN;
        traversal(root, res);
        return res;
    }
private:
    int traversal(TreeNode* root, int& res) {
        if (root == NULL) return 0;
        int left = max(traversal(root->left, res), 0);
        int right = max(traversal(root->right, res), 0);
        res = max(res, root->val + left + right);
        return max(left, right) + root->val;;
    }
};
```

### 方法二

*  树形DP
  *  `dp[i][0]`：该节点不可以继续向上的最大路径和，`dp[i][1]`该节点可以继续向上的最大路径和
  *  后序遍历，先计算左右子树，再根据返回值进行处理
  *  `dp[i][0]`：不用该节点，子节点最大的，以及用该节点做桥梁（取最大）
  *  `dp[i][1]`：选择左边向上，还是右边向上，还是自己向上（取最大）

```cpp
class Solution {
public:
    int maxPathSum(TreeNode* root) {
        vector<int> res = traversal(root);
        return max(res[0], res[1]);
    }
private:
    vector<int> traversal(TreeNode* root) {
        if (root == NULL) return {-1001, -1001};
        vector<int> dpLeft = traversal(root->left);
        vector<int> dpRight = traversal(root->right);
        int dp0 = max({dpLeft[0], dpRight[0], dpLeft[1], dpRight[1], dpLeft[1] + dpRight[1] + root->val});
        int dp1 = max(max(dpLeft[1], dpRight[1]), 0) + root->val;
        return {dp0, dp1};
    }
};
```

## 0297. Serialize and Deserialize Binary Tree

> :red_circle:

将二叉树序列化为字符串，并反序列化为二叉树

### 思路
考察二叉树的遍历与构造。在记录空子节点的情况下，前序、后序、层序遍历均可序列化并反序列化

### 方法一
- 前序遍历，空指针用'#'
- 根据前序遍历结果递归构造二叉树
- 写法一：用','分割，利用string.find() 和 substr() 分割字符串
- 写法二：用stringstream进行分割
  - 需要头文件`<sstream>`，进行流的输入输出操作，可用于数据类型转换，多个字符串拼接，分割字符串
  - 默认遇到空格、tab、回车换行会停止字节流输出，配合getline(ss, token, ',')可实现其它字符分割

<!-- tabs:start -->

####**First**

```cpp
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string res = "";
        preOrderTraversal(root, res);
        res.pop_back();
        return res;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        vector<string> values = split(data);
        int start = 0;
        return constructTree(values, start);
    }

private:
    void preOrderTraversal(TreeNode* node, string& res) {
        if (node == NULL) {
            res += "#,";
            return;
        }
        res += to_string(node->val) + ",";
        preOrderTraversal(node->left, res);
        preOrderTraversal(node->right, res);
    }

    TreeNode* constructTree(vector<string>& values, int& start) {
        if (values[start] == "#") {
            start++;
            return NULL;
        }
        TreeNode* root = new TreeNode(stoi(values[start]));
        start++;
        root->left = constructTree(values, start);
        root->right = constructTree(values, start);
        return root;
    }
    
    vector<string> split(string s) {
        vector<string> res;
        for (int i = 0; i < s.size(); i++) {
            int pos = s.find(',', i);
            if (pos < s.size()) {
                res.push_back(s.substr(i, pos - i));
                i = pos;
            } else {
                res.push_back(s.substr(i));
                break;
            }
        }
        return res;
    }
};
```

####**Second**

```cpp
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string res = "";
        preOrderTraversal(root, res);
        res.pop_back();
        return res;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        stringstream ss(data);
        return constructTree(ss);
    }

private:
    void preOrderTraversal(TreeNode* node, string& res) {
        if (node == NULL) {
            res += "# ";
            return;
        }
        res += to_string(node->val) + " ";
        preOrderTraversal(node->left, res);
        preOrderTraversal(node->right, res);
    }

    TreeNode* constructTree(stringstream& ss) {
        string first;
        ss >> first;
        if (first == "#") return NULL;
        TreeNode* root = new TreeNode(stoi(first));
        root->left = constructTree(ss);
        root->right = constructTree(ss);
        return root;
    }
};
```

<!-- tabs:end -->

## 0450. Delete Node in a BST

> :orange_circle:

给定BST和key，删除二叉搜索树中值为key的节点（每个节点的值唯一）

### 方法

- 依据BST的特性，找到待删除的节点，根据情况决定删除方式（删除后仍然为BST）
- 待删除节点左子节点为空，返回右子节点；右子节点为空，返回左子节点
- 左右子节点都不为空时，可将右子树的最左节点的值移到待删除节点

```cpp
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == NULL) return root;
        if (root->val == key) {
            if (!root->left) return root->right;
            else if (!root->right) return root->left;
            else {
                TreeNode* cur = root->right;
                TreeNode* pre = root;
                while (cur->left) {
                    pre = cur;
                    cur = cur->left;
                }
                root->val = cur->val;
                if (pre != root) pre->left = cur->right;
                else root->right = cur->right;
                delete cur;
                return root;
            }
        }
        else if (root->val > key) root->left = deleteNode(root->left, key);
        else root->right = deleteNode(root->right, key);
        return root;
    }
};
```

## 0098. Validate Binary Search Tree

> :orange_circle:

给定一个二叉树，验证其是否是二叉搜索树

### 方法
