# 基础知识

3. 数组中的重复数字
```python
def duplicate(a, duplication):
    n = a.length
    for i=0 to n-1:
        while a[i] != i:
            if a[i] == a[a[i]]:
                duplication[0] = a[i]
                return true
            else:
                swap(a, i, a[i])
    return false
def swap(a, i, j):
    temp = a[i]
    a[i] = a[j]
    a[j] = temp
```
4. 从尾到头打印链表
```python
def printListFromTailToHead(listNode):
    list = List()
    if listNode != null:
        printListFromTailToHead(listNode.next)
        list.add(listNode.val)
    return list
```
7. 重建二叉树
```python
def reConstructBinaryTree(preA, inA, preL, preR, inL):
    if preL > preR:
        return null
    root = TreeNode(preA[preL])
    size = 0
    while root.val != inA[inL+size]:
        size++
    root.left = reConstructBinaryTree(preA, inA, preL+1, preL+size, inL)
    root.right = reConstructBinaryTree(preA, inA, preL+size+1, preR, inL+size+1)
    return root
```
#### 8. 二叉树的下一个结点
```python
def getNext(node):
    if node.right != null:
        nextNode = node.right
        while(nextNode != null):
            next = next.left
        return nextNode
    else:
        while node.next != null:
            parent = node.next
            if (parent.left == node):
                return parent
            node = node.next
    return null
```
#### 9. 用两个栈实现队列
```python
inStack = Stack()
outStack = Stack()
def enqueue(item):
    inStack.push(item)
def dequeue():
    if outStack.isEmpty():
        while !inStack.isEmpty():
            outStack.push(inStack.pop())
    if inStack.isEmpty():
        throw new Exception("queue is empty)
    return outStack.pop()
```
#### 10.1 斐波那契数列
```python
def Fibonacci(n):
    if n <= 1:
        return n
    pre1 = 0, pre2 = 1, fib = pre1+pre2
    for i=2 to n:
        fib = pre1 + pre2
        pre1 = pre2
        pre2 = fib
    return fib

def Fibonacci(n):
    if n <= 1:
        return n
    return Fibonacci(n-1) + Fibonacci(n-2)
```
#### 10.2 跳台阶
```python
def JumpFloor(n):
    if n <= 2:
        return n
    pre1 = 1, pre2 = 2, result = 1
    for i=2 to n-1:
        result = pre1 + pre2
        pre1 = pre2
        pre2 = result
    return result
```
#### 10.4 变态跳台阶
```python
def JumpFloorII(n):
    dp = [n]
    
```
#### 11. 旋转数组的最小数字
```python
def minNumberInRotateArray(a):
    n = a.length
    low = 0, high = n-1
    while low < high:
        mid = (low + high) / 2
        if a[mid] <= a[high]:
            h = m
        else:
            l = m+1
    return a[l]

```
# 解决面试题的思路
### 1画图-抽象问题形象化
#### 27. 二叉树的镜像
[nowcoder](https://www.nowcoder.com/practice/564f4c26aa584921bc75623e48ca3011?tpId=13&tqId=11171&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
结构： 二叉树：前序遍历
策略： 无
思路：画图可知，二叉树的镜像与原二叉树相比，以根节点为对称轴，交换了对应位置上的元素。即，前序遍历二叉树，对于任意节点，交换其左子树和右子树。
伪代码：
```python
def mirror(root):
    if root == null: return;
    swap(root.left, root.right)
    mirror(root.left)
    mirror(root.right)
def swap(nodeA, nodeB):
    tempNode = nodeA
    nodeA = nodeB
    nodeB = tempNode
```
```java
public void Mirror(TreeNode root) {
    if (root == null) {
        return;
    }
    swap(root);
    Mirror(root.left);
    Mirror(root.right);
}
private void swap(TreeNode root) {
    TreeNode tempNode = root.left;
    root.left = root.right;
    root.right = tempNode;
}
```
#### 28. 对称二叉树
[nowcoder](https://www.nowcoder.com/practice/ff05d44dfdb04e1d83bdbdab320efbcb?tpId=13&tqId=11211&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

结构： 二叉树：遍历
策略：无
思路：这是一个是否问题，可以从是的角度考虑，也可以从否的角度考虑。画图可知，对称二叉树在同一层上，以根节点为分界面，其左子树左子节点与右子树右子节点相同，其左子树右子节点与右子树左子节点相同。所以对于整棵树，不考虑根节点，同时遍历数的两棵子树，如果出现对应位置左子树与右子树节点数不同或者值不同则非对称二叉树。
伪代码：
```python
def isSymmetrical(root):
    if root == null: return true
    return isSymmetrical(root.left, root.right)
def isSymmetrical(root1, root2):
    if (root1 == null and  root2 == null): return true
    if root1 == null or root2 == null: return false
    if root1.val != root2.val: return false
    return isSymmetrical(root1.left, root.right) and isSymmetrical(root1.right, root2.left)
```
```java
boolean isSymmetrical(TreeNode root) {
    if (root == null) {
        return true;
    }
    return isSymmetrical(root.letf, root.right);
}
boolean isSymmetrical(TreeNode root1, TreeNode root2) {
    if (root1 == null && root2 == null) {
        return true;
    }
    if (root1 == null || root2 == null) {
        return false;
    }
    if (root1.val != root2.val) {
        return false;
    }
    return isSymmetrical(root1.left, root2.right) && isSymmetrical(root1.right, root2.left);
}
```

#### 29. 顺时针打印矩阵
[nowcoder](https://www.nowcoder.com/practice/9b4c81a02cd34f76be2659fa0d54342a?tpId=13&tqId=11172&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
结构： 二维矩阵：遍历
策略： 双指针索引
思路：对于顺时针打印矩阵其路径已经很明确，即按行-列-行-列的顺序一圈一圈的向内打印。难点在于每次打印时索引的范围和索引行和列的切换。可以用四个变量动态标记行和列的边界。c1,c2表示列的左和右,r1, r2表示行的上和下。按c1:c2 - r1+1:r2 - c2-1:c1 - r2-1:r1+1索引元素值，每打印一圈后动态改变c1,c2,r1,r2。直到c1=c2,r1=r2。
伪代码：
```python
def printMatrix(matrix):
    r1 = 0, r2 = matrix.length - 1
    c1 = 0, c2 = matrix[0].length - 1
    list = List()
    while r1 <= r2 and c1 <= c2:
        for i = c1 to c2:
            list.add(matrix[r1][i])
        for i = r1+1 to r2:
            list.add(matrix[i][c2])
        if r1 < r2:
            for i = c2-1 to c1:
                list.add(matrix[r2][i])
        if c1 < c2:
            for i = r2-1 to r1+1:
                list.add(matrix[i][c1])
        c1++, c2--
        r1++, r2--
    return list
```
```java
    public ArrayList<Integer> printMatrix(int [][] matrix) {
    int r1 = 0, r2 = matrix.length - 1;
    int c1 = 0, c2 = matrix[0].length - 1;
    ArrayList<Integer> list = new ArrayList<>();
    while (r1 <= r2 && c1 <= c2) {
        for (int i = c1; i <= c2; i++) {
            list.add(matrix[r1][i]);
        }
        for (int i = r1+1; i <= r2; i++) {
            list.add(matrix[i][c2]);
        }
        // 避免重复打印同一行
        if (r1 < r2) {
            for (int i = c2-1; i >= c1; i--) {
                list.add(matrix[r2][i]);
            }
        }
        // 避免重复打印同一列
        if (c1 < c2) {
            for (int i = r2-1; i > r1; i--) {
                list.add(matrix[i][c1]);
            }
        }
        c1++; c2--;
        r1++; r2--;
    }
    return list;
}
```


### 2举例-抽象问题具体化
#### 30.包含min函数的栈
[nowcoder](https://www.nowcoder.com/practice/4c776177d2c04c2494f2555c9fcc1e49?tpId=13&tqId=11173&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
结构： 栈：入栈，出栈，获得栈顶元素
策略： 无
思路：由于push,pop,min的时间复杂度都必须为O(1),进行入栈出站时min元素应该能动态变化，以保证可以在O(1)时间内被找到。可以使用一个辅助栈，当入栈时，当前最小元素入辅助栈，当出栈时当前最小元素出栈。
伪代码：
```python
dataStack = Stack()
minStack = Stack()
def push(node):
    if minStack.isEmpty() or node < minStack.peek():  minStack.push(node)
    else:  minStack.push(minStack.peek())
def pop():
    dataStack.pop()
    minStack.pop()
def min():
    return minStack.peek()
```
```java
    Stack<Integer> dataStack = new Stack<>();
    Stack<Integer> minStack = new Stack<>();
    public void push(int node) {
        dataStack.push(node);
        if (minStack.isEmpty() || node < minStack.peek()) {
            minStack.push(node);
        } else {
            minStack.push(minStack.peek());
        }
    }
    public void pop() {
        dataStack.pop();
        minStack.pop();
    }
    public int top() {
        return dataStack.peek();
    }
    public int min() {
        return minStack.peek();
    }
```
#### 31.栈的压入与弹出序列
[nowcoder](https://www.nowcoder.com/practice/d77d11405cc7470d82554cb392585106?tpId=13&tqId=11174&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
结构： 栈：入栈，出栈，获得栈顶元素
策略： 无
思路：建立一个栈，模拟出栈入栈，遍历入栈序列，元素入栈，当栈顶元素与出栈序列元素相等时，出栈。遍历结束时，如果栈为空表示模拟成功。
伪代码：
```python
def isPopOrder(pushA, pushB):
    stack = Stack()
    popIndex = 0
    for pushIndex=0 to pushA.length:
        stack.push(pushA[pushIndex])
        while popIndex < popA.length and stack.peek() == popA[popIndex]:
            stack.pop()
            popIndex++
    return stack.isEmpty()
```

```java
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        Stack<Integer> stack = new Stack<>();
        for (int pushIndex = 0, popIndex = 0; pushIndex < pushA.length; pushIndex++) {
            stack.push(pushA[pushIndex]);
            while (popIndex < popA.length && stack.peek() == popA[popIndex]) {
                stack.pop();
                popIndex++;
            }
        }
        return stack.isEmpty();
    }
```

#### 32. 从上到下打印二叉树
[nowcoder](https://www.nowcoder.com/practice/7fe2212963db4790b57431d9ed259701?tpId=13&tqId=11175&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)
结构： 二叉树：层序遍历
策略：无
思路：
伪代码：
```python
def PrintFromTopToBottom(root):
    list = List()
    if root == null:  return list
    queue = Queue()
    queue.add(root)
    while !queue.isEmpty():
        tempNode = queue.remove()
        list.add(tempNode.val)
        if tempNode.left != null:  queue.add(tempNode.left)
        if tempNode.right != null:  queue.add(tempNode.right)
    return list
```
```java
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
        ArrayList<Integer> list = new ArrayList<>();
        if (root == null)
            return list;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            TreeNode tempNode = queue.remove();
            list.add(tempNode.val);
            if (tempNode.left != null) {
                queue.add(tempNode.left);
            }
            if (tempNode.right != null) {
                queue.add(tempNode.right);
            }
        }
        return list;
    }
```
#### 32.2 分层打印印二叉树
伪代码：
```java
def print(root):
    list = List()
    if root == null:  return list
    queue = Queue()
    queue.add(root)
    nextLevel = 0, curLevel = 1
    while !queue.isEmpty():
        alist = List()
        while curLevel != 0:
            node = queue.remove()
            curLevel--
            alist.add(node.val)
            if node.left != null:
                queue.add(node.left)
                nextLevel++
            if node.right != null:
                queue.add(node.right)
                nextLevel++
        curLevel = nextLevel
        nextLevel = 0
        list.add(alist)
    return list
```
```java
    ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer>> list = new ArrayList<>();
        if (pRoot == null) {
            return list;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(pRoot);
        int nextLevel = 0;
        int curLevel = 1;
        while (!queue.isEmpty()) {
            ArrayList<Integer> alist = new ArrayList<>();
            while (curLevel != 0) {
                TreeNode tempNode = queue.remove();
                curLevel--;
                alist.add(tempNode.val);
                if (tempNode.left != null) {
                    queue.add(tempNode.left);
                    nextLevel++;
                }
                if (tempNode.right != null) {
                    queue.add(tempNode.right);
                    nextLevel++;
                }
            }
            curLevel = nextLevel;
            nextLevel = 0;
            list.add(alist);
        }
        return list;
    }
```

