#### 1、字符串

##### （1）KMP算法（字符串匹配）

当出现字符串不匹配时，可以记录一部分之前已经匹配的文本内容，利用这些信息避免从头再去做匹配。** 

给定一个文本串  aabaabaaf 、一个模式串   aabaaf

判断文本串中是否包含模式串

前缀表：前缀表是用来回退的，它记录了模式串与主串(文本串)不匹配的时候，模式串应该从哪里开始重新匹配。

记录下标i之前（包括i）的字符串中，有多大长度的相同前缀后缀。

以模式串为例：前缀：a    aa   aab    aaba   aabaa  

aabaaf不是前缀

后缀： f   af   aaf   baaf   abaaf    

aabaaf不是后缀

什么是最长相等前后缀？

根据最长相等前后缀得出前缀表

a 0

aa 1

aab  0

aaba  1

aabaa 2

aabaaf 0

如何进行匹配？

找到的不匹配的位置，此时我们要看前一个字符的前缀表的数值是多少，找前面字符串的最长相同的前缀和后缀。然后 把下标移动到下标为（前一个字符的前缀表的数值）的位置继续比配。 

#### 2、栈和队列

```c++
queue.push();  //将一个元素放入队列尾部
queue.peek();  // 返回队列首部的元素
queue.pop();   // 从队列首部移除元素
queue.empty(); // 返回队列是否为空
stack.push();  //将一个元素放入栈顶
stack.pop();   // 从栈顶移除元素
stack.top();   // 返回栈顶元素
stack.empty(); // 返回栈是否为空
```

#### 3、二叉树的递归遍历

##### 3.1递归的三要素：

1、确定递归函数的参数和返回值

确定哪些参数是递归过程中需要处理的，那么就在递归函数里加上这个参数，并且明确每次递归的返回值是什么进而确定递归函数的返回类型。

2、确定终止条件

操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。

3、确定单层递归的逻辑

确定每一层递归需要处理的信息。重复调用自己实现递归的过程。

##### 3.2递归法前序遍历

```c++
// 1、确定递归函数的参数和返回值
void traversal(TreeNode *cur, vector<int>& vec)
// 2、确定终止条件
if (cur == NULL) return;
// 3、确定单层递归的逻辑  
vec.push_back(cur->val);    // 中
traversal(cur->left, vec);  // 左
traversal(cur->right, vec); // 右

class Solution {
public:
    void traversal(TreeNode* cur, vector<int>& vec){
        if(cur == NULL) return;
        vec.push_back(cur-<val);
        traversal(cur->left, vec);
        traversal(cur->right, vec);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```

递归法遍历的实现：每一次递归调用把函数的局部变量、参数值和返回地址等压入调用栈中，然后递归返回的时候，从栈顶弹出上一次递归的各项参数。

 用栈也可以是实现二叉树的前后中序遍历 

##### 3.3迭代法遍历

```c++
//前序遍历
class Solution {
public:
    vector<int>preorderTraversal(TreeNode* root) {
        stack<int> st;
        vector<int>result;
        if(root == NULL) return result;
        st.push(root);
        while(!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if(node->right) st.push_back(node->right);
            if(node->left) st.push_back(node->left);
        }
    }
};
//中序遍历
class Solution {
public:
    vector<int>inorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int>result;
        if(root == NULL) return result;
        st.push(root);
        while(!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if(node->right) st.push_back(node->right);
            if(node->left) st.push_back(node->left);
        }
    }
};
//后续遍历
class Solution {
public:
    vector<int>postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int>result;
        TreeNode* cur = root;
        while(cur != NULL || !st.empty()) {
            if(cur != NULL) { //指针来访问节点，访问到最底层
                st.push(cur); //将访问的节点放入栈
                cur = cur->left;  //左
            } 
            else {
                cur = st.top(); 
                //从栈里弹出的数据，就是要处理的数据
                st.pop();
                result.push_back(cur->val); //中
                cur = cur->right;           //右
            } 
        }
 	   return result;   
    }
};
```

#### 4、单调栈

通常是一维数组，要寻找任一个元素的右边或左边第一个比自我大或小的元素的位置，此时就想到用单调栈。

1、单调栈中存放的元素是什么？

只要存放元素的下标，如果要使用对应的元素，直接用T[I]获取

2、单调栈的元素是递增呢还是递减呢？

顺序为栈头到栈底的顺序，

3、使用单调栈主要有三个判断条件。

- 当前遍历的元素T[i]小于栈顶元素T[st.top()]的情况
- 当前遍历的元素T[i]等于栈顶元素T[st.top()]的情况
- 当前遍历的元素T[i]大于栈顶元素T[st.top()]的情况

