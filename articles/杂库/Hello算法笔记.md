
<a name="sGNsU"></a>
### 数据结构
> 可以根据逻辑结构和物理结构来区分

- 按照逻辑结构    

      1. 线性数据结构： 数组、链表、栈、队列、哈希表<br />      2.非线形数据结构：树、图、堆、哈希表

- 按照物理结构（在计算机内存中的存储方式）

1. 连续空间存储<br />      2.离散空间存储
<a name="NUUzq"></a>
### 数组
> 将相同类型元素存储在连续内存空间的数据结构，为什么数组的索引从0开始？因为索引本质上表示的是内存地址的偏移量，首个地址的偏移量是0，所以索引自然为0

<a name="s6gTx"></a>
#### 优点：

- 快速查找，直接访问对应索引元素
<a name="X2eMF"></a>
#### 缺点：

- 插入和删除的效率比较低（插入的时候需要后挪，删除的时候需要前移，时间复杂度O(N））
<a name="PdRhl"></a>
### 链表
> 一种线形数据结构，由一个个节点连接。节点中包含包含：当前节点的值value和指向下一个节点的地址next

尾节点的next字段值为null
<a name="HaeSH"></a>
#### 链表类型

1. 单向链表
2. 环形链表
3. 双向链表
<a name="Jrcaz"></a>
#### 优点：

- 插入和删除的效率高，只需要改变原来一个节点的指针
<a name="hy8Zy"></a>
#### 缺点：

- 访问节点的效率低，不能直接访问想要的节点，只能通过前置节点才能知道下一个节点
- 内存占用多，因为一个节点的内容除了包含值还有下个节点的饮用地址

<a name="nBfiD"></a>
### 栈
> 先进后出，叠在一起的盘子，要拿最低下的盘子，需要把上面的盘子一个个拿走

` javascript`可以使用数组模拟栈操作，也可以使用链表

<a name="AUBhb"></a>
### 队列
> 先进先出

`javascript`可以使用数组模拟栈操作，也可以使用链表
<a name="em1bC"></a>
#### 队列类型

- 双向队列，两端都可以添加和删除元素

<a name="AszfC"></a>
### 哈希表(HashMap)
> 哈希表通过键值之前的映射，实现高效的元素查找。时间复杂度0(1),在`js`中可以使用Map类型来实现
> 避免hash冲突可以是用Symbol来指定唯一key


<a name="JxaVt"></a>
### 二叉树
> 二叉树是一种非线性数据结构，表示祖先与后代之间的派生关系，体现一分为二的分治逻辑。类似于链表，二叉树也是节点关联。节点包含一个值，两个指针，左指针和右指针

```javascript
/* 初始化二叉树 */
// 初始化结点
let n1 = new TreeNode(1),
    n2 = new TreeNode(2),
    n3 = new TreeNode(3),
    n4 = new TreeNode(4),
    n5 = new TreeNode(5);
// 构建引用指向（即指针）
n1.left = n2;
n1.right = n3;
n2.left = n4;
n2.right = n5;
```
<a name="HS9MN"></a>
#### 完美二叉树
所有的节点都被填满，所有节点的度为2<br />![perfect_binary_tree.png](https://cdn.nlark.com/yuque/0/2023/png/297368/1673599957981-dd5ebabd-dc47-4354-9585-9bb7a7a0987f.png#averageHue=%23fcfcfc&clientId=u0140c98f-87b6-4&crop=0&crop=0&crop=1&crop=1&from=ui&height=421&id=uc32e59ad&margin=%5Bobject%20Object%5D&name=perfect_binary_tree.png&originHeight=720&originWidth=1280&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65603&status=done&style=none&taskId=ufbc03126-fd63-48cd-9467-678d1542e3b&title=&width=748)
<a name="oiGUv"></a>
#### 完全二叉树
只有最底层的节点没有被填满，且最底层节点尽量靠左填充<br />![complete_binary_tree.png](https://cdn.nlark.com/yuque/0/2023/png/297368/1673599986964-bee60a94-8066-425b-b4a7-5b27faa77e35.png#averageHue=%23fcfcfc&clientId=u0140c98f-87b6-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=u9ff4532f&margin=%5Bobject%20Object%5D&name=complete_binary_tree.png&originHeight=720&originWidth=1280&originalType=binary&ratio=1&rotation=0&showTitle=false&size=56261&status=done&style=none&taskId=u016e14ce-9b65-4cd5-9d31-168accc43e8&title=)
<a name="ihv7j"></a>
#### 完满二叉树
除了叶结点之外，其余所有的节点都有两个子节点<br />![full_binary_tree.png](https://cdn.nlark.com/yuque/0/2023/png/297368/1673600134537-41e2ee79-8d19-41e3-a6ec-11a7886ffd6d.png#averageHue=%23fdfdfd&clientId=u0140c98f-87b6-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=u5e1654b3&margin=%5Bobject%20Object%5D&name=full_binary_tree.png&originHeight=720&originWidth=1280&originalType=binary&ratio=1&rotation=0&showTitle=false&size=50414&status=done&style=none&taskId=u72b2150c-d4e4-41f8-9858-5f5444558c4&title=)
<a name="q4ptx"></a>
#### 平衡二叉树
任意节点的左子树和右子树的高度之差绝对值 ![](https://cdn.nlark.com/yuque/__latex/0fc7b821521f1860f7e5cd7b8d1de90a.svg#card=math&code=%E2%89%A4%201&id=UpfLg)<br />![balanced_binary_tree.png](https://cdn.nlark.com/yuque/0/2023/png/297368/1673600405243-c786c52d-c6ee-4f45-bb70-76bbc42ff9ae.png#averageHue=%23fbfbfb&clientId=u0140c98f-87b6-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=ue20bb64d&margin=%5Bobject%20Object%5D&name=balanced_binary_tree.png&originHeight=720&originWidth=1280&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63985&status=done&style=none&taskId=u2baf4d36-2183-4326-9791-503870f2f83&title=)
<a name="HEuSg"></a>
#### 层序遍历（广度优先）
从上到下，从左到右

<a name="bciuI"></a>
#### 前序/中序/后序（深度优先）
其体现着一种：先走到头，再回头继续的遍历方式

- 前序访问： // 访问优先级：根结点 -> 左子树 -> 右子树
- 中序访问： // 访问优先级：左子树 -> 根结点 -> 右子树 
- 后序访问： // 访问优先级：左子树 -> 右子树 -> 根结点

![binary_tree_dfs.png](https://cdn.nlark.com/yuque/0/2023/png/297368/1673600801905-58a2a830-92bc-4a60-ab44-d070cd98e9bd.png#averageHue=%23fcfbfa&clientId=u0140c98f-87b6-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=u36dffa4a&margin=%5Bobject%20Object%5D&name=binary_tree_dfs.png&originHeight=720&originWidth=1280&originalType=binary&ratio=1&rotation=0&showTitle=false&size=174973&status=done&style=none&taskId=ub495513e-404d-4aae-afc6-d7f0f187cfc&title=)

<a name="uDyFB"></a>
#### 二叉搜索树
二叉搜索树需要满足：

1. 对于根节点，左子树中所有的节点的值 < 根节点的值 < 右子树所有的节点值
2. 任意节点的左子树和右子树也是满足条件1，即也是二叉搜索树

对于以上，可以想到先排序，然后二分

<a name="ebjek"></a>
#### AVL树
> AVL 树的独特之处在于「旋转 Rotation」的操作，其可 **在不影响二叉树中序遍历序列的前提下，使失衡结点重新恢复平衡**。换言之，旋转操作既可以使树保持为「二叉搜索树」，也可以使树重新恢复为「平衡二叉树」。


<a name="Rq5nS"></a>
#### 堆
一颗限定条件下的「完全二叉树」。<br />根据成立条件：

- Max Heap 大顶堆： 任意节点的值 ≥ 其子节点的值
- Min Heap 小顶堆： 任意节点的值 ≤ 其子节点的值


<a name="CY4t2"></a>
### 查找算法
<a name="ubcVi"></a>
#### 线形查找
遍历数据结构+判断是否命中<br />时间复杂度： O(n)<br />空间复杂度： O(1)
<a name="KRheu"></a>
##### 优点：

- 线形查找通用性极佳：没有跳跃访问的元素，数组和链表都可以
<a name="GJJbQ"></a>
##### 缺点：

- 线形查找的时间复杂度太高，n足够大时，查找销量低
<a name="sdhK5"></a>
#### 二分查找
利用数据的有序性，通过每轮缩小一半的搜索区间来查找<br />使用二分法的两个前置条件：

- 数据必须有序性
- 仅适用于数组，因为在循环的过程中需要跳跃
> 如果为了满足数据有序性，去做数据的排序，是得不偿失。排序算法的时间复杂度一般为O(nlogn)

<a name="eQymW"></a>
##### 优点：

- 时间复杂度低
- 不需要额外的空间
<a name="iIN6s"></a>
##### 缺点：

- 仅适用于有序数据
- 仅适用于数组
- 在小数据量情况下，线形查找的性能更好
<a name="hy6tC"></a>
#### hash查找
借助一个哈希数据结构，根据键值对的映射，实现O(1)的查找，以空间换时间<br />在`js`中使用Map类型
<a name="aBAb6"></a>
##### 优点：

- 查找、插入、删除操作的平均时间复杂度都为 O(1) 
<a name="OTUOw"></a>
##### 缺点：

- 需要使用额外空间
- 建立hash需要时间，不适合高频增删改，低频查找
- 数据量小，线形查找更快
<a name="GSzTv"></a>
#### 
<a name="Qd7nu"></a>
### 排序算法
<a name="BVKIr"></a>
#### 冒泡排序
```javascript
function bubbleSort(arrs) {
  for (let i = nums.length; i > 0; i--) {
    for (let j = 0; j < i; j++) {
      if (nums[j] > nums[j+1]) {
        const tmp = nums[j];
        nums[j] = nums[j+1]
        nums[j+1] = tmp
      }
    }
  }
}
```
<a name="gojTO"></a>
#### 插入排序
```javascript
function insertSort(nums) {
  for (let i = 1; i < nums.length; i++) {
    let tmp = nums[i]
    let j = i - 1
    while(j >= 0 && nums[i] < nums[j]) {
      nums[i] = nums[j]
      j--
    }
    nums[j+1] = tmp
  }
}
```

<a name="gckur"></a>
#### 快速排序
```javascript
// 先计算一次分割点，然后递归
function partition(nums, left, right) {
  // 取第一个元素为基准值
  // i 从左往右找到一个 大于基准值的
  // j 从右往左找到一个 小于基准值的 
  let i = left
  let j = right
  const base = nums[i]
  while(i < j) {
    while(i < j && nums[j] > nums[left]) {
      j--
    }
    while(i < j && nums[i] <= nums[left]) {
      i++
    }
    swap(nums, i, j)
  }
  // i,j重叠
  swap(nums, i, left)
  return i
}
function swap(nums, i, j) {
  const tmp = nums[i] 
  nums[i] = nums[j]
  nums[j] = tmp
}
// 
function quickSort(nums, left, right) {
  if (left >= right) {
    return
  }
  const pivot = partition(nums, left, right)
  quickSort(nums, left, pivot - 1)
  quickSort(nums, pivot + 1, right)
}
```
