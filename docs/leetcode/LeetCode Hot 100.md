# LeetCode Hot 100

## 刷题攻略

灵神（python）在B站

宫水三叶（java）在leetcode

代码随想录（B站）

## 题库

### 哈希（3）

#### 哈希表理论基础

哈希表（Hash table），国内也有一些数据结构书籍翻译为散列表。哈希表是根据关键码的值而直接进行访问的数据结构。直白来讲其实数组就是一张哈希表。哈希表中关键码就是数组的索引下标，然后通过下标直接访问数组中的元素。

![](assets/哈希表/哈希表1.png)

**一般哈希表都是用来快速判断一个元素是否出现集合里。**

例如要查询一个名字是否在这所学校里。要暴力枚举的话时间复杂度是O(n)，但如果使用哈希表的话， 只需要O(1)就可以做到。我们只需要初始化把这所学校里学生的名字都存在哈希表里，在查询的时候通过索引直接就可以知道这位同学在不在这所学校里了。

将学生姓名映射到哈希表上就涉及到了**hash function ，也就是哈希函数**。

![](assets/哈希表/哈希函数.png)

#### 常见的哈希结构

在C++中，常见的哈希数据结构有 **数组、Set、Map** 这三种，底层原理和时间复杂度会有略微的区别，但整体都是基于哈希表/红黑树实现，对应的时间复杂度为O(1) / O(log n)。不同语言提供的内置哈希结构会有所差异，比如Go语言常用的Hash结构为 **Map** 。如果要详细了解不同语言（如C++、Java、Go...）所提供的哈希数据结构细节，可以分别查看对应的编程语言学习模块，进行详细了解。

#### Hot 100 例题

##### 两数之和

错误解法：暴力解决，但是会超时

```go
func twoSum(nums []int, target int) []int {
    // 直接两个for循环暴力解，时间复杂度O(n^2)
	for i := 0; i < len(nums); i++ {
        for j := i + 1; j < len(nums); j++ {
            a := nums[i]
            b := nums[j]
            sum := a + b
            fmt.Println(sum)
            if sum == target {
                return []int{i, j}
            }
        }
	}
    return []int{}
}
```

标准正确解法：使用Hash Map

```go
func twoSum(nums []int, target int) []int {
    // 初始化一个空的 hash map
    hash := make(map[int] int)
    for i, num := range nums {
        sub := target - num
        // 核心逻辑，判断hash map中是否已经存了一个数字，能够满足 该数字 + num = target
        // 若满足，则直接返回两个数字对应的数组下标
        if j, is_exists := hash[sub]; is_exists {
            return []int{j, i}
        }
        // 不满足，则继续向hash map中添加num
        hash[num] = i
    }
    return nil
}
```



### 双指针（4）

#### Hot 100 例题

##### 移动零

```go
func moveZeroes(nums []int)  {
    // 双指针实现（非交换）：不使用额外数组，直接在原数组上进行修改
    i := 0
    for j := 0; j < len(nums); j++ {
        // 寻找非0元素，i指针代表遍历过的非0元素个数，j指针代表遍历到了元素的哪个位置
        if nums[j] != 0 {
            nums[i] = nums[j]
            i++
        }
    }
    
    // 将剩余数组空间全部填0
    for i < len(nums) {
        nums[i] = 0
        i++
    }
}
```

```go
func moveZeroes(nums []int)  {
    // 双指针实现（0元素和非0元素进行交换）：不使用额外数组，直接在原数组上进行修改
    i := 0
    for i < len(nums) {
        if nums[i] == 0 {
            // 找右边第一个不为0的元素
            j := i + 1
            for j < len(nums) && nums[j] == 0 {
                j++
            }
            // 右边全是0，直接结束
            if j == len(nums) {
                return
            }
            temp := nums[j]
            nums[j] = nums[i]
            nums[i] = temp
        }
        i++
    }
}
```

### 滑动窗口（2）

### 子串（3）

### 普通数组（5）

### 矩阵（4）

### 链表（14）

#### 链表理论基础

#### 常见的链表结构

#### Hot 100 例题

##### 相交链表

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    // 比较基础的做法：通过两个链表的长度，寻找相交结点
    pA, pB := headA, headB
    lenA, lenB := 0, 0

    // 首先计算两个链表的表长
    for pA != nil {
        pA = pA.Next
        lenA++
    }
    for pB != nil {
        pB = pB.Next
        lenB++
    }

    // 比较两个链表的长度，哪个更长，哪个先走
    var step int
    var fast, slow *ListNode
    if lenA > lenB {
        step = lenA - lenB
        fast, slow = headA, headB
    } else {
        step = lenB - lenA
        fast, slow = headB, headA
    }

    // 更长的链表先走step步
    for i := 0; i < step; i++ {
        fast = fast.Next
    }

    // 此时，两个指针同时走，有相交结点就返回，无相交结点返回nil
    for fast != slow {
        fast = fast.Next
        slow = slow.Next
    }

    return fast
}
```

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    // 比较神奇的做法，不用考虑两个链表的长度
    pA, pB := headA, headB

    // 本质是让两个链表走相同的长度（lenA + lenB），如果有相交，那么走过相同的距离，一定会返回相交结点
    // 否则返回nil
    for pA != pB {
        if pA != nil {
            pA = pA.Next
        } else {
            pA = headB
        }

        if pB != nil {
            pB = pB.Next
        } else {
            pB = headA
        }
    }

    return pA
}
```

##### 反转链表

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseList(head *ListNode) *ListNode {
    // 指针法实现链表翻转，要理解清楚pre，cur和next指针的含义
    var pre *ListNode
    cur := head

    for cur != nil {
        next := cur.Next  // 保存后继
        cur.Next = pre  // 反转指针，前驱变后继
        pre = cur  // pre前移
        cur = next  // cur前移
    }

    return pre
}
```

### 二叉树（15）

#### Hot 100 例题

##### 二叉树的中序遍历

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func inorderTraversal(root *TreeNode) []int {
    // 中序遍历，左根右
    if root == nil {
        return []int {}
    }

    // 使用递归方法实现（代码很简单，但是还是要好好思考一下递归调用的过程）
    res := []int{}
    res = append(res, inorderTraversal(root.Left)...)
    res = append(res, root.Val)
    res = append(res, inorderTraversal(root.Right)...)

    return res
}
```

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func inorderTraversal(root *TreeNode) []int {
    // 非递归方法实现（借助栈），代码更复杂，仔细体会（可以画一个满二叉树，然后一步步推）
    res := []int{}
    stack := []*TreeNode{}
    p := root

    // 只要指针p不为nil或者栈stack里面还有结点，就不断进行中序遍历
    for p != nil || len(stack) > 0 {
        // 先走到最左
        for p != nil {
            stack = append(stack, p)
            p = p.Left
        }

        // 出栈
        n := len(stack) - 1
        p = stack[n]
        stack = stack[:n]

        res = append(res, p.Val)

        // 转向右子树（即使右子树为nil，但是只要栈里面元素还没有被弹空，这个最外层就还会继续走下去）
        p = p.Right
    }

    return res
}
```

### 图论（4）

### 回溯（8）

### 二分查找（6）

#### Hot 100 例题

##### 搜索插入位置（二分查找）

```go
func searchInsert(nums []int, target int) int {
    // 二分查找，基于排序后的数组
    left, right := 0 ,len(nums) - 1
    for left <= right {
        // mid
        mid := (left + right) / 2
        if nums[mid] == target {
            return mid
        } else if nums[mid] > target {
            // target落在左半区间
            right = mid - 1 
        } else {
            // target落在右半区间
            left = mid + 1
        }
    }
    return left
}
```

### 栈（5）

### 堆（3）

### 贪心算法（4）

### 动态规划（10）

### 多维动态规划（5）

### 技巧（5）



