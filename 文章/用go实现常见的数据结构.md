# 1 golang常见数据结构实现
## 1.1 链表

举单链表的例子，双向链表同理只是多了pre指针。

定义单链表结构：

```go
type LinkNode struct {
	Data     int64
	NextNode *LinkNode
}
```

构造链表及打印链表：

```go
func main() {

	node := new(LinkNode)
	node.Data = 1

	node1 := new(LinkNode)
	node1.Data = 2
	node.NextNode = node1 // node1 链接到 node 节点上

	node2 := new(LinkNode)
	node2.Data = 3
	node1.NextNode = node2 // node2 链接到 node1 节点上

	// 顺序打印。把原链表头结点赋值到新的NowNode上
	// 这样仍然保留了原链表头结点node不变
	nowNode := node
	for nowNode != nil {
		fmt.Println(nowNode.Data)
		// 获取下一个节点。链表向下滑动
		nowNode = nowNode.NextNode
	}
}
```

## 1.2 可变数组

可变数组在各种语言中都非常常用，在golang中，可变数组语言本身已经实现，就是我们的切片slice。

## 1.3 栈和队列

### 1.3.1 原生切片实现栈和队列

栈：先进后出，后进先出，类似弹夹

队列：先进先出

golang中，实现并发不安全的栈和队列，非常简单，我们直接使用原生切片即可。

#### 1.3.1.1 切片原生栈实现

```go
func main() {
	// 用切片制作一个栈
	var stack []int
	// 元素1 入栈
	stack = append(stack, 1, 5, 7, 2)
	// 栈取出最近添加的数据。例如[1,5,7,2] ，len = 4
	x := stack[len(stack)-1] // 2
	// 切掉最近添加的数据，上一步和这一步模仿栈的pop。
	stack = stack[:len(stack)-1] // [1,5,7]
	fmt.Printf("%d", x)
}
```
#### 1.3.1.2 切片原生队列实现

```go
func main() {

	// 用切片模仿队列
	var queue []int
	// 进队列
	queue = append(queue, 1, 5, 7, 2)
	// 队头弹出，再把队头切掉，模仿队列的poll操作
	cur := queue[0]
	queue = queue[1:]

	fmt.Printf("%d", cur)
}
```

### 1.3.2 并发安全的栈和队列

#### 1.3.2.1 切片实现并发安全的栈
* 并发安全的栈

```go
// 数组栈，后进先出
type Mystack struct {
    array []string   // 底层切片
    size  int        // 栈的元素数量
    lock  sync.Mutex // 为了并发安全使用的锁
}
```

* 入栈

```go
// 入栈
func (stack *Mytack) Push(v string) {
    stack.lock.Lock()
    defer stack.lock.Unlock()

    // 放入切片中，后进的元素放在数组最后面
    stack.array = append(stack.array, v)

    // 栈中元素数量+1
    stack.size = stack.size + 1
}
```
* 出栈

1、如果切片偏移量向前移动 stack.array[0 : stack.size-1]，表明最后的元素已经不属于该数组了，数组变相的缩容了。此时，切片被缩容的部分并不会被回收，仍然占用着空间，所以空间复杂度较高，但操作的时间复杂度为：O(1)。

2、如果我们创建新的数组 newArray，然后把老数组的元素复制到新数组，就不会占用多余的空间，但移动次数过多，时间复杂度为：O(n)。

```go
func (stack *Mystack) Pop() string {
    stack.lock.Lock()
    defer stack.lock.Unlock()

    // 栈中元素已空
    if stack.size == 0 {
        panic("empty")
    }

    // 栈顶元素
    v := stack.array[stack.size-1]

    // 切片收缩，但可能占用空间越来越大
    //stack.array = stack.array[0 : stack.size-1]

    // 创建新的数组，空间占用不会越来越大，但可能移动元素次数过多
    newArray := make([]string, stack.size-1, stack.size-1)
    for i := 0; i < stack.size-1; i++ {
        newArray[i] = stack.array[i]
    }
    stack.array = newArray

    // 栈中元素数量-1
    stack.size = stack.size - 1
    return v
}
```

*  获取栈顶元素

```go
// 获取栈顶元素
func (stack *Mystack) Peek() string {
    // 栈中元素已空
    if stack.size == 0 {
        panic("empty")
    }

    // 栈顶元素值
    v := stack.array[stack.size-1]
    return v
}
```

* 获取栈大小和判定是否为空

```go
// 栈大小
func (stack *Mystack) Size() int {
    return stack.size
}

// 栈是否为空
func (stack *Mystack) IsEmpty() bool {
    return stack.size == 0
}
```

#### 1.3.2.2 切片实现并发安全的队列

* 队列结构

```go
// 数组队列，先进先出
type Myqueue struct {
    array []string   // 底层切片
    size  int        // 队列的元素数量
    lock  sync.Mutex // 为了并发安全使用的锁
}
```

* 入队

```go
// 入队
func (queue *Myqueue) Add(v string) {
    queue.lock.Lock()
    defer queue.lock.Unlock()

    // 放入切片中，后进的元素放在数组最后面
    queue.array = append(queue.array, v)

    // 队中元素数量+1
    queue.size = queue.size + 1
}
```

* 出队

1、原地挪位，依次补位 queue.array[i-1] = queue.array[i]，然后数组缩容：queue.array = queue.array[0 : queue.size-1]，但是这样切片缩容的那部分内存空间不会释放。

2、创建新的数组，将老数组中除第一个元素以外的元素移动到新数组。

```go
// 出队
func (queue *Myqueue) Remove() string {
    queue.lock.Lock()
    defer queue.lock.Unlock()

    // 队中元素已空
    if queue.size == 0 {
        panic("empty")
    }

    // 队列最前面元素
    v := queue.array[0]

    /*    直接原位移动，但缩容后继的空间不会被释放
        for i := 1; i < queue.size; i++ {
            // 从第一位开始进行数据移动
            queue.array[i-1] = queue.array[i]
        }
        // 原数组缩容
        queue.array = queue.array[0 : queue.size-1]
    */

    // 创建新的数组，移动次数过多
    newArray := make([]string, queue.size-1, queue.size-1)
    for i := 1; i < queue.size; i++ {
        // 从老数组的第一位开始进行数据移动
        newArray[i-1] = queue.array[i]
    }
    queue.array = newArray

    // 队中元素数量-1
    queue.size = queue.size - 1
    return v
}
```

## 1.4 字典Map和集合Set

### 1.4.1 Map

字典也是程序语言经常使用的结构，golang中的字典是其自身实现的map结构。具体操作可以查看语言api

并发安全的map，可以定义结构，结构中有一个map成员和一个锁变量成员，参考并发安全的栈和队列的实现。go语言也实现了一个并发安全的map,具体参考sync.map的api

### 1.4.2 Set
我们可以借助map的特性，实现一个Set结构。

* Set结构

map的值我们不适用，定义为空的结构体struct{}

```go
// 集合结构体
type Set struct {
    m            map[int]struct{} // 用字典来实现，因为字段键不能重复
    len          int          // 集合的大小
    sync.RWMutex              // 锁，实现并发安全
}
```

* 初始化Set

```go
// 新建一个空集合
func NewSet(cap int64) *Set {
    temp := make(map[int]struct{}, cap)
    return &Set{
        m: temp,
    }
}
```

* 往set中添加一个元素

```go
// 增加一个元素
func (s *Set) Add(item int) {
    s.Lock()
    defer s.Unlock()
    s.m[item] = struct{}{} // 实际往字典添加这个键
    s.len = len(s.m)       // 重新计算元素数量
}
```
* 删除一个元素

```go
// 移除一个元素
func (s *Set) Remove(item int) {
    s.Lock()
    s.Unlock()

    // 集合没元素直接返回
    if s.len == 0 {
        return
    }

    delete(s.m, item) // 实际从字典删除这个键
    s.len = len(s.m)  // 重新计算元素数量
}
```
* 查看元素是否在集合set中

```go
// 查看是否存在元素
func (s *Set) Has(item int) bool {
    s.RLock()
    defer s.RUnlock()
    _, ok := s.m[item]
    return ok
}
```
* 查看集合大小

```go
// 查看集合大小
func (s *Set) Len() int {
    return s.len
}
```
* 查看集合是否为空

```go
// 集合是够为空
func (s *Set) IsEmpty() bool {
    if s.Len() == 0 {
        return true
    }
    return false
}
```
* 清除集合所有元素

```go
// 清除集合所有元素
func (s *Set) Clear() {
    s.Lock()
    defer s.Unlock()
    s.m = map[int]struct{}{} // 字典重新赋值
    s.len = 0                // 大小归零
}
```
* 将集合转化为切片

```go
func (s *Set) List() []int {
    s.RLock()
    defer s.RUnlock()
    list := make([]int, 0, s.len)
    for item := range s.m {
        list = append(list, item)
    }
    return list
}
```

## 1.5 二叉树
二叉树：每个节点最多只有两个儿子节点的树。

满二叉树：叶子节点与叶子节点之间的高度差为 0 的二叉树，即整棵树是满的，树呈满三角形结构。在国外的定义，非叶子节点儿子都是满的树就是满二叉树。我们以国内为准。

完全二叉树：完全二叉树是由满二叉树而引出来的，设二叉树的深度为 k，除第 k 层外，其他各层的节点数都达到最大值，且第 k 层所有的节点都连续集中在最左边。

* 二叉树结构定义

```go
// 二叉树
type TreeNode struct {
    Data  string    // 节点用来存放数据
    Left  *TreeNode // 左子树
    Right *TreeNode // 右字树
}
```

* 树的遍历

1、先序遍历：先访问根节点，再访问左子树，最后访问右子树。

2、后序遍历：先访问左子树，再访问右子树，最后访问根节点。

3、中序遍历：先访问左子树，再访问根节点，最后访问右子树。

4、层次遍历：每一层从左到右访问每一个节点。

```go
// 先序遍历
func PreOrder(tree *TreeNode) {
    if tree == nil {
        return
    }

    // 先打印根节点
    fmt.Print(tree.Data, " ")
    // 再打印左子树
    PreOrder(tree.Left)
    // 再打印右字树
    PreOrder(tree.Right)
}

// 中序遍历
func MidOrder(tree *TreeNode) {
    if tree == nil {
        return
    }

    // 先打印左子树
    MidOrder(tree.Left)
    // 再打印根节点
    fmt.Print(tree.Data, " ")
    // 再打印右字树
    MidOrder(tree.Right)
}

// 后序遍历
func PostOrder(tree *TreeNode) {
    if tree == nil {
        return
    }

    // 先打印左子树
    MidOrder(tree.Left)
    // 再打印右字树
    MidOrder(tree.Right)
    // 再打印根节点
    fmt.Print(tree.Data, " ")
}
```

按层遍历：

```go
func Level(head *TreeNode) {

	if head == nil {
		return
	}

	// 用切片模仿队列
	var queue []*TreeNode
	queue = append(queue, head)

	for len(queue) != 0 {
		// 队头弹出，再把队头切掉，模仿队列的poll操作
		cur := queue[0]
		queue = queue[1:]

		fmt.Printf("%d", (*cur).Data)

		// 当前节点有左孩子，加入左孩子进队列
		if cur.Left != nil {
			queue = append(queue, cur.Left)
		}

		// 当前节点有右孩子，加入右孩子进队列
		if cur.Right != nil {
			queue = append(queue, cur.Right)
		}
	}

}
```
