# 二叉树

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

#### **First**

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

#### **Second**

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