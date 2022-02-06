# 算法
## 二叉排序树（Binary Search Tree)
### 知识
1. 左小于根节点
2. 右大于根节点
3. 中序遍历有序  
### 题目
#### 230 BST中的第k小的元素 
1. solution 1 : recursion + inOrder ,count number while number == k retrun;
2. solution 2 :  iteration + inOrder,others same  
```
public int kthSmallest(TreeNode root, int k) {
        Deque<TreeNode> que = new LinkedList<>();
        TreeNode p = root;
        while(p!=null || !que.isEmpty()){
            if(p!=null){
                que.offerLast(p);
                p = p.left;
            }else{
                p = que.pollLast();
                number++;
                if(number == k){
                    ans = p.val;
                    break;
                }
                p = p.right;
            }
        }
        return ans;
    }
```
3. solution 3 :如果你需要频繁地查找第k小的值，你将如何优化算法?  
记录下以每个结点为根结点的子树的结点数  
既可以将以每个结点为根结点的子树的结点数存储在结点中，也可以将其记录在哈希表中  
number(left)  k-1 ?  
< 右子树  
\> 左子树  
= 根节点
```
class myBST{
    TreeNode root;
    Map<TreeNode,Integer> sumMap;

    public myBST(TreeNode root){
        this.root = root;
        sumMap = new HashMap<>();
        preProcess(root);
    }
   
   
   
    public int getK(TreeNode node,int k){
        if(node == null) return 0;
        int leftnum = sumMap.getOrDefault(node.left,0);
        if(leftnum == k-1) return node.val;
        else if(leftnum > k-1) return getK(node.left,k);
        else return getK(node.right,k-leftnum-1);
    }
   
   
   
    private int preProcess(TreeNode node){
        if(node == null) return 0;
        int sum = 1+preProcess(node.left)+preProcess(node.right);
        sumMap.put(node,sum);
        return sum;
    }
}
```
4. 若取消排序树这个性质，则需遍历整个树，并加以排序
### 538. 把二叉搜索树转换为累加树
1. 使用普通递归 反中序遍历求解  
2. 改写递归为迭代，时间复杂度等于递归，可以借此复习一下 前 中 遍历的迭代iteration写法 以及后序遍历的写法
```
 public TreeNode convertBST(TreeNode root) {
        TreeNode p;
        int total = 0;
        Deque<TreeNode> stack = new LinkedList<>();

        p = root;
        while(p!=null || !stack.isEmpty()){
            if(p!=null){
                stack.offerLast(p);
                p = p.right;
            }else{
                p = stack.pollLast();
                p.val = p.val + total;
                total = p.val;
                p = p.left;
            }
        }

        return root;
    }
```
4. （Morris）遍历（动态线索树）：一种巧妙的方法可以在线性时间内，只占用常数空间来实现中序遍历  
### 450 701 700 98 关于 *二叉搜索树* 的操作：插入，删除，查找，验证
插入和查找较为简单
1. 插入都是插到空结点上，二分即可
2. 查找递归二分即可
3. 关于删除，存在用什么值来代替当前节点的问题，一般用前继和后继  
[450 题解答案，递归和迭代](https://leetcode.com/problems/delete-node-in-a-bst/discuss/1591176/Simple-Solution-w-Images-and-Detailed-Explanation-or-Iterative-and-Recursive-Approach)  
