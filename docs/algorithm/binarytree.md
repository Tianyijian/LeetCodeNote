# 二叉树

## 0144. Binary Tree Preorder Traversal

> :green_circle:

二叉树的前序遍历

### 方法一

- 递归遍历。前序遍历是中左右。T: O(n), S: O(n)

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        traversal(root, res);
        return res;
    }
private:
    void traversal(TreeNode* node, vector<int>& vals) {
        if (node == NULL) return;
        vals.push_back(node->val);
        traversal(node->left, vals);
        traversal(node->right, vals);
    }
};
```

### 方法二

- 迭代遍历，采用栈，处理当前节点，将其右子节点和左子节点一次入栈。T: O(n), S: O(n)

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        while (!st.empty()) {
            TreeNode* cur = st.top();
            st.pop();
            res.push_back(cur->val);
            if (cur->right) st.push(cur->right);
            if (cur->left) st.push(cur->left);
        }
        return res;
    }

};
```

### 方法三

- 统一迭代遍历。主要解决遍历节点和处理节点不一致的情况，前序遍历是一致的

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        while (!st.empty()) {
            TreeNode* cur = st.top();
            st.pop();
            if (cur != NULL) {
                if (cur->right) st.push(cur->right);
                if (cur->left) st.push(cur->left);
                st.push(cur);
                st.push(NULL);
            } else {
                cur = st.top();
                st.pop();
                res.push_back(cur->val);
            }
        }
        return res;
    }
};
```

## 0094. Binary Tree Inorder Traversal

> :green_circle:

二叉树的中序遍历

### 方法一

- 递归遍历

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        traversal(root, res);
        return res;
    }
private:
    void traversal(TreeNode* node, vector<int>& vals) {
        if (node == NULL) return;
        traversal(node->left, vals);
        vals.push_back(node->val);
        traversal(node->right, vals);
    }
};
```

### 方法二

- 迭代遍历：借助指针的遍历帮助访问节点，栈用来处理节点元素

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> st;
        TreeNode* cur = root;
        while (cur != NULL || !st.empty()) {
            if (cur != NULL) {
                st.push(cur);
                cur = cur->left;
            } else {
                cur = st.top();
                st.pop();
                res.push_back(cur->val);
                cur = cur->right;
            }
        }
        return res;
    }
};
```

### 方法三

- 统一迭代遍历：解决访问节点（遍历节点）和处理节点（将元素放进结果集）不一致的情况
- 将访问的节点放入栈中，把要处理的节点也放入栈中但是要紧接着放入一个空指针做标记

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        while (!st.empty()) {
            TreeNode* cur = st.top();
            if (cur != NULL) {
                st.pop();
                if (cur->right) st.push(cur->right);
                st.push(cur);
                st.push(NULL);
                if (cur->left) st.push(cur->left);
            } else {
                st.pop();
                cur = st.top();
                st.pop();
                res.push_back(cur->val);
            }
        }
        return res;
    }
};
```

## 0101. Symmetric Tree

> :green_circle:

判断二叉树是否是自己的镜像，即中心对称

### 方法一

- 迭代法，成对比较，添加时按照外侧和内侧的顺序，成对加入成对取出，因此采用栈也可以

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        queue<TreeNode*> que;
        if (root != NULL) {
            que.push(root->left);
            que.push(root->right);
        };
        while (!que.empty()) {
            TreeNode* leftNode = que.front();
            que.pop();
            TreeNode* rightNode = que.front();
            que.pop();
            if (leftNode && rightNode) {
                if (leftNode->val != rightNode->val) {
                    return false;
                }
                que.push(leftNode->left);
                que.push(rightNode->right);
                que.push(leftNode->right);
                que.push(rightNode->left);                    
            } else if (!leftNode && !rightNode) {
                continue;
            } else {
                return false;
            }
        }
        return true;
    }
};
```

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
        dfs(root);
        return res;
    }
private:
    int res = INT_MIN;
    int dfs(TreeNode* node) {
        if (node == NULL) return 0;
        int left = max(dfs(node->left), 0);
        int right = max(dfs(node->right), 0);
        res = max(res, left + right + node->val);
        return max(left, right) + node->val;
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

## 0543. Diameter of Binary Tree

> :green_circle:

给定二叉树，计算直径。二叉树的直径是树中两个节点的最长路径的长度，长度是两个节点间的边的数量，路径可能不经过根节点

### 方法

- 类似124题，二叉树最大路径和。到达一个节点时，左右都加是否能打败最大值
- 加左子树或加右子树，该值可以继续向上加，将其返回

```cpp
class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        diameter(root);
        return maxRes;
    }
private:
    int maxRes = 0;
    int diameter(TreeNode* root) {
        if (root == NULL) return -1;
        int left = diameter(root->left);
        int right = diameter(root->right);
        maxRes = max(maxRes, left + right + 2);
        return max(left, right) + 1;
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

* 节点值大于左子树最大值（最右节点），小于右子树最小值（最左节点）
* 空节点是二叉搜索树
* 递归法
  * 中序遍历，记录数组，判断数组是否增序
  * 中序遍历，一直更新最大值，判断是否小于中间节点值
  	* 最小值long long maxVal = LONG_MIN; 防止测试数据中有int最小值
  * 中序遍历，记录前一个节点

```cpp
class Solution {
public:
    TreeNode* pre = NULL;
    bool isValidBST(TreeNode* root) {
        if (root == NULL) return true;
        bool left = isValidBST(root->left);
        
        if (pre != NULL && pre->val >= root->val) return false;
        pre = root;
        
        bool right = isValidBST(root->right);
        return left && right;
    }
};
```
