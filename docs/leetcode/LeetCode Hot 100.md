# LeetCode Hot 100

## 刷题攻略

灵神（python）在B站

宫水三叶（java）在leetcode

代码随想录（B站）

## 高频考点

1. [最长连续序列](#⭐⭐⭐⭐⭐ 最长连续序列  ✅😅) ✅✅
2. [盛最多水的容器](#⭐⭐⭐⭐ 盛最多水的容器  ✅😅) ✅✅
3. [三数之和](# ⭐⭐⭐⭐⭐ 三数之和  ✅😅) ✅✅
4. [接雨水](# ⭐⭐⭐⭐⭐⭐ 接雨水  ✅😅)✅
5. [无重复字符的最长子串](# ⭐⭐⭐⭐⭐ 无重复字符的最长子串  ✅😅) ✅✅
6. [滑动窗口最大值](# ⭐⭐⭐⭐⭐⭐⭐滑动窗口最大值✅😅) ✅✅
7. [最小覆盖子串](# ⭐⭐⭐⭐⭐⭐⭐最小覆盖子串✅😅) ✅
8. [合并区间](# ⭐⭐⭐⭐⭐合并区间✅😅) ✅
9. [搜索二维矩阵2](# ⭐⭐⭐⭐⭐ 搜索二维矩阵2  ✅😀) ✅
10. [反转链表](# ⭐⭐⭐反转链表 ✅😅) ✅✅
11. 两两交换链表中的节点
12. k个一组翻转链表
13. [二叉树的中序遍历](# ⭐⭐⭐二叉树的中序遍历 ✅😅) ✅✅
14. [二叉树的最大深度](# ⭐⭐二叉树的最大深度 ✅😅) ✅✅
15. 二叉树的层序遍历
16. 二叉树的右视图
17. 路径总和3
18. 二叉树的最近公共祖先
19. [岛屿数量](# ⭐⭐ ⭐ ⭐ ⭐  岛屿数量✅😅) ✅✅
20. [全排列](# ⭐⭐ ⭐ ⭐ ⭐  全排列 ✅😅) ✅✅
21. 子集
22. 组合总和
23. 搜索插入位置
24. [打家劫舍](# ⭐⭐⭐⭐⭐打家劫舍 ✅😅) ✅✅
25. 零钱兑换
26. 最长递增子序列
27. 最小路径和
28. 最长公共子序列
29. 下一个排列





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

##### ⭐ 两数之和  ✅😅

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

##### ⭐⭐⭐⭐⭐ 最长连续序列  ✅😅

```go
func longestConsecutive(nums []int) int {
    if len(nums) == 0 {
        return 0
    }

    // 创建hash集合，同时去重
    hash := make(map[int]bool)
    for _, num := range nums {
        hash[num] = true
    }

    maxLensStreak := 0

    for num := range hash {
        // 只从可能的起点开始检查
        if !hash[num-1] {
            currentNum := num
            currentLensStreak := 1

            // 向上查找连续的数字
            for hash[currentNum+1] {
                currentNum++
                currentLensStreak++
            }

            // 更新最长序列长度
            if currentLensStreak > maxLensStreak {
                maxLensStreak = currentLensStreak
            }
        }
    }

    return maxLensStreak
}
```



### 双指针（4）

#### Hot 100 例题

##### ⭐ 移动零  ✅😅

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

##### ⭐⭐⭐⭐ 盛最多水的容器  ✅😅

```go
func maxArea(height []int) int {
    left, right := 0, len(height) - 1
    maxArea := 0

    for left < right {
        // 计算当前水量
        width := right - left
        h := height[left]
        if height[right] < h {
            h = height[right]
        }

        area := width * h
        if area > maxArea {
            maxArea = area
        }

        // 移动较短的那条线
        if height[left] < height[right] {
            left++
        } else {
            right--
        }
    }

    return maxArea
}
```

##### ⭐⭐⭐⭐⭐ 三数之和  ✅😅

```go
​```
核心思想：
	1. 先排序
	2. 固定一个数 nums[i]
	3. 用双指针在后面找 -nums[i]
关键点：去重！！！
时间复杂度：O(n²)
空间复杂度：O(1)
​```
import (
    "fmt"
    "sort"
)

func threeSum(nums []int) [][]int {
    results := [][]int{}

    sort.Ints(nums)
    n := len(nums)

    for i:= 0; i < n-2; i++ {
        // 如果排序后的第一个元素都大于0，那么后面求和不可能等于0
        if nums[i] > 0 {
            break
        }

        // 去重: 跳过相同的起始值
        if i > 0 && nums[i] == nums[i-1] {
            continue
        }

        left, right := i+1, n-1

        // 双指针
        for left < right {
            sum := nums[i] + nums[left] + nums[right]

            if sum == 0 {
                results = append(results, []int{nums[i], nums[left], nums[right]})

                // 去重: 跳过左边的重复值
                for left < right && nums[left] == nums[left+1] {
                    left++
                }

                // 去重: 跳过右边的重复值
                for left < right && nums[right] == nums[right-1] {
                    right--
                }

                left++
                right--

            } else if sum < 0 {
                left++
            } else {
                right--
            }
        }
    }

    return results
}
```

##### ⭐⭐⭐⭐⭐⭐ 接雨水  ✅😅

```go
​```
使用左右两个指针从两端向中间移动
每次移动较矮的一边，因为较矮的一边决定了能接多少水
​```
func trap(height []int) int {
    if len(height) == 0 {
        return 0
    }

    left, right := 0, len(height)-1
    leftMax, rightMax := 0, 0
    rain := 0

    for left < right {
        if height[left] < height[right] {
            if height[left] > leftMax {
                leftMax = height[left]
            } else {
                rain += leftMax - height[left]
            }
            left++
        } else {
            if height[right] > rightMax {
                rightMax = height[right]
            } else {
                rain += rightMax - height[right]
            }
            right--
        }
    }

    return rain
}
```



### 滑动窗口（2）

#### Hot 100 例题

##### ⭐⭐⭐⭐⭐ 无重复字符的最长子串  ✅😅

```go
func lengthOfLongestSubstring(s string) int {
    // 记录字符最后出现的位置
    lastseen := make(map[byte]int)
    start := 0
    maxlen := 0

    for i := 0; i < len(s); i++ {
        ch := s[i]
        
        // 如果当前位置的这个字符存在lastseen中，并且它的位置是在起点的后面，说明它出现过
        // 此时，移动窗口起点到重复字符的下一个位置
        if pos, exists := lastseen[ch]; exists && pos >= start {
            start = pos + 1
        }
		
        // 更新当前窗口长度，其实不管怎么走，i都一定是窗口的 "end"
        currentlen := i - start + 1
        if currentlen > maxlen {
            maxlen = currentlen
        }

        lastseen[ch] = i
    }
    return maxlen
}
```

##### 找到字符串中所有字母异位词 ❌😅

### 子串（3）

##### ⭐⭐⭐⭐⭐⭐⭐滑动窗口最大值✅😅

```go
​```
最优解法，时间复杂度 $O(n)$。
核心思想：维护一个单调递减的双端队列（Double End Queue）
	队首始终是当前窗口的最大值。
	队列中存储的是元素的索引，队列对应的元素值单调递减。
规则：
	队首维护：如果队首索引已不在当前窗口内（i - queue[0] >= k），则弹出队首
	队尾维护：新元素入队前，从队尾弹出所有比它小的元素索引（因为这些元素不可能成为后面窗口的最大值）
	队尾入队：将当前元素索引加入队尾
	记录结果：当窗口形成后（i >= k-1），队首对应的元素就是当前窗口的最大值
​```
func maxSlidingWindow(nums []int, k int) []int {
    n := len(nums)
    if n == 0 || k == 0 {
        return []int{}
    }
	
    // 最终返回的结果，大小为n-k+1，这个储存的才是值
    results := make([]int, n-k+1)
    // 维护一个单调递减的双端队列，并且这个双端队列存储的是元素的位置索引，而不是值
    deque := make([]int, 0)

    for i := 0; i < n; i++ {
        // 队首元素出队
        // 把队首不在当前这个窗口的元素弹出
        if len(deque) > 0 && deque[0] < i-k+1 {
            deque = deque[1:]
        }

        // 队尾元素出队
        // 遍历这个队列，把队尾所有小于当前元素的索引，全部弹出
        // 因为这些元素不可能成为后面窗口的最大值
        for len(deque) > 0 && nums[deque[len(deque)-1]] < nums[i] {
            deque = deque[:len(deque)-1]
        }

        // 当前索引入队
        deque = append(deque, i)

        // 队首元素加入results
        // 从第k-1个元素开始，窗口形成
        if i >= k-1 {
            results[i-k+1] = nums[deque[0]]
        }
    }

    return results
}
```

##### ⭐⭐⭐⭐⭐⭐⭐最小覆盖子串✅😅

```go
func minWindow(s string, t string) string {
    m, n := len(s), len(t)
    if m == 0 || n == 0 || m < n {
        return ""
    }

    // 用hash统计 t 中每个字符的需求量
    need := make(map[byte]int)
    for i := 0; i < len(t); i++ {
        need[t[i]]++
    }

    left, right := 0, 0
    // 注意，这个 minLen 一定是 len(s)+1，直接len(s)会有问题
    minStart, minLen := 0, len(s)+1
    // 还需要匹配的字符总数
    count := len(t)

    window := make(map[byte]int)

    for right < len(s) {
        // 1. 把当前ch加入到窗口
        ch := s[right]
        window[ch]++

        // 需要ch && 当前窗口内的ch数量没满足需求
        // 说明上面添加ch有效，也就是的确需要添加ch，此时count--
        if need[ch] > 0 && window[ch] <= need[ch] {
            count--
        }

        for count == 0 && left <= right {
            // 2. 更新滑动窗口起点和长度
            if right-left+1 < minLen {
                minLen = right - left + 1
                minStart = left
            }

            // 3. 把leftChar从窗口当中移除
            leftChar := s[left]
            window[leftChar]--

            // 需要leftChar && 当前窗口内的leftChar数量没满足需求
            // 但是上面又把leftChar移出去了
            // 所以需要再往右找，此时count++
            if need[leftChar] > 0 && window[leftChar] < need[leftChar] {
                count++
            }

            left++
        }

        right++
    }

    // 4. 最终判断并返回
    if minLen == len(s)+1 {
        return ""
    }

    return s[minStart : minStart+minLen]
}
```



### 普通数组（5）

#### Hot 100 例题

##### ⭐⭐⭐⭐最大子数组和✅😅

```go
// Kadane 算法（动态规划）
func maxSubArray(nums []int) int {
    if len(nums) == 1 {
        return nums[0]
    }

    currentSum := nums[0]  // 当前子数组的和
    maxSum := nums[0]  // 全局最大和

    // 从 "1" 开始，如果从 "0" 开始的话，会导致第一个元素被处理两次
    for i := 1; i < len(nums); i++ {
        // 要么从当前元素开始，要么继续累加
        currentSum = max(nums[i], currentSum + nums[i])
		// 更新全局最大和
        maxSum = max(currentSum, maxSum)
    }

    return maxSum
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

##### ⭐⭐⭐⭐⭐合并区间✅😅

```go
import (
    "sort"
)

func merge(intervals [][]int) [][]int {
    if len(intervals) == 0 {
        return [][]int{}
    }

    // 先按照区间的起点进行排序
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][0] < intervals[j][0]
    })

    results := make([][]int, 0)
    current := intervals[0]

    for i := 0; i < len(intervals); i++ {
        next := intervals[i]

        // 重叠合并，取最大的右端点
        // next[0] current[1] next[1]
        if next[0] <= current[1] {
            if current[1] < next[1] {
                current[1] = next[1]
            }
        } else {
            // 不重叠，加入结果
            results = append(results, current)
            current = next
        }
    }

    // 加入最后一个区间
    results = append(results, current)
    return results
}
```



### 矩阵（4）

#### Hot 100 例题

##### ⭐⭐⭐⭐矩阵置零✅😅

```go
func setZeroes(matrix [][]int)  {
    if len(matrix) == 0 {
        return
    }

    // 计算行数与列数
    m, n := len(matrix), len(matrix[0])

    // 技巧：第一行一定对应列数；第一列一定对应行数
    firstRowHasZeros := false
    firstColHasZeros := false

    // 检查第一行是否有0
    for i := 0; i < n; i++ {
        if matrix[0][i] == 0 {
            firstRowHasZeros = true
        }
        
    }

    // 检查第一列是否有0
    for j := 0; j < m; j++ {
        if matrix[j][0] == 0 {
            firstColHasZeros = true
        }
    }

    // 用第一行和第一列做标记
    for i:= 1; i < m; i++ {
        for j:= 1; j < n;j++ {
            if matrix[i][j] == 0 {
                matrix[i][0] = 0
                matrix[0][j] = 0
            }
        }
    }

    // 根据标记置0
    for i := 1; i < m; i++ {
        for j := 1; j < n; j++ {
            if matrix[i][0] == 0 || matrix[0][j] == 0 {
                matrix[i][j] = 0
            }
        }
    }

    // 处理第一行
    if firstRowHasZeros {
        for i := 0; i < n; i++ {
            matrix[0][i] = 0
        }
    }

    // 处理第一列
    if firstColHasZeros {
        for j := 0; j < m; j++ {
            matrix[j][0] = 0
        }
    }
}
```

##### ⭐⭐⭐⭐⭐ 搜索二维矩阵2  ✅😀

```go
​```
z字形查找-从右上角开始
	时间复杂度 O(m + n) --- 最优解
逐行二分
	时间复杂度 O(m * log n)
逐列二分
	时间复杂度 O(n * log m)
​```
func searchMatrix(matrix [][]int, target int) bool {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return false
    }

    m, n := len(matrix), len(matrix[0])
    row, col := 0, n-1

    for row < m && col >= 0 {
        value := matrix[row][col]
        if value == target {
            return true
        } else if value < target {
            // target更大，往下走
            row++
        } else {
            // target更小，往左走
            col--
        }
    }

    return false
}
```



### 链表（14）

#### 链表理论基础

#### 常见的链表结构

#### Hot 100 例题

##### ⭐⭐ 相交链表 ✅😅

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

##### ⭐⭐⭐反转链表 ✅😅

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

##### ⭐⭐⭐⭐⭐回文链表 ✅😅

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func isPalindrome(head *ListNode) bool {
    // 注意边界条件
    if head == nil || head.Next == nil {
        return true
    }

    // 使用快慢指针找到链表中点
    fast, slow := head, head
    // 这里边界条件一定要是fast != nil && fast.Next != nil
    // 不能是fast != nil && slow != nil，感觉很容易写错
    for fast != nil && fast.Next != nil {
        fast = fast.Next.Next
        slow = slow.Next
    }
    
    // 反转后半部分链表
    var prev *ListNode
    cur := slow
    for cur != nil {
        next := cur.Next  // 保存当前节点的下一个节点
        cur.Next = prev  // 反转指针
        prev = cur  // 前驱指针前进
        cur = next  // 当前指针前进
    }

    // 比较前半部分和反转后的后半部分链表
    p, q := head, prev
    for p != nil && q != nil {
        if p.Val != q.Val {
            return false
        }
        p = p.Next
        q = q.Next
    }
    return true
}
```

```go
func isPalindrome(head *ListNode) bool {
    if head == nil || head.Next == nil {
        return true
    }

    // 不用快慢指针，先计算总长度，再计算中间节点
    len := 0
    p := head
	for p != nil {
        len++
        p = p.Next
    }

    var slow *ListNode
    p = head
    // 注意奇数和偶数的区别，go默认向下取整，(len + 1) / 2 的目的是向上取整，实现奇数和偶数的统一
    // 4: 0 1 2 3
    // 3: 0 1 2
    for i := 0; i < (len + 1) / 2; i++ {
        p = p.Next
    }
	slow = p
    
    var prev *ListNode
    cur := slow
    for cur != nil {
        next := cur.Next
        cur.Next = prev
        prev = cur
        cur = next
    }

    p, q := head, prev
    for p != nil && q != nil {
        if p.Val != q.Val {
            return false
        }
        p = p.Next
        q = q.Next
    }
    return true
}
```

##### ⭐⭐⭐环形链表 ✅😅

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func hasCycle(head *ListNode) bool {
    if head == nil || head.Next == nil {
        return false
    }
	
    // 这里要注意，如果把快慢指针都初始化成head，下面if判断的时候，就直接返回true了
    // 所以这里初始化的时候错开，让快指针先走一步
    // slow, fast := head, head
    slow, fast := head, head.Next
    for fast != nil && fast.Next != nil {
        if slow == fast {
            return true
        }
        slow = slow.Next
        fast = fast.Next.Next
    }

    return false
}
```



### 二叉树（15）

#### Hot 100 例题

##### ⭐⭐⭐二叉树的中序遍历 ✅😅

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

##### ⭐⭐二叉树的最大深度 ✅😅

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func maxDepth(root *TreeNode) int {
    // 如果树为空，深度为 0。
    if root == nil {
        return 0
    }
    
    // 否则，深度 = 1 + max(左子树深度, 右子树深度)
    leftDepth := maxDepth(root.Left)
    rightDepth := maxDepth(root.Right)
    if leftDepth > rightDepth {
        return leftDepth + 1
    } else {
        return rightDepth + 1
    }
}
```

##### ⭐⭐ 翻转二叉树✅😅

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func invertTree(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }

    left := invertTree(root.Left)
    right := invertTree(root.Right)

    root.Left = right
    root.Right = left
    return root
}
```



### 图论（4）

#### Hot 100 例题

##### ⭐⭐ ⭐ ⭐ ⭐  岛屿数量✅😅

- DFS深度优先搜索

```go
func numIslands(grid [][]byte) int {
    if len(grid) == 0 {
        return 0
    }

    m, n := len(grid), len(grid[0])
    count := 0

    var dfs func(int, int)
    dfs = func(i, j int) {
        // 检测到 "水" 就返回
        if i < 0 || i >= m || j < 0 || j >= n || grid[i][j] != '1' {
            return
        }
        grid[i][j] = '0'

        dfs(i-1, j)
        dfs(i+1, j)
        dfs(i, j-1)
        dfs(i, j+1)
    }

    for i:= 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if grid[i][j] == '1' {
                count++
                dfs(i, j)
            }
        }
    }

    return count
}
```

- BFS广度优先搜索

- 并查集

- DFS + 辅助矩阵标记

### 回溯（8）

#### Hot 100 例题

##### ⭐⭐ ⭐ ⭐ ⭐  全排列 ✅😅

```text
初始状态
    nums = [1, 2, 3]
    path = []
    visited = [false, false, false]
    result = []
    
第一层
├── 选择1
│   ├── 选择2
│   │   └── 选择3 → [1,2,3]
│   └── 选择3
│       └── 选择2 → [1,3,2]
├── 选择2
│   ├── 选择1
│   │   └── 选择3 → [2,1,3]
│   └── 选择3
│       └── 选择1 → [2,3,1]
└── 选择3
    ├── 选择1
    │   └── 选择2 → [3,1,2]
    └── 选择2
        └── 选择1 → [3,2,1]
        
1. path = [] → [1] → [1,2] → [1,2,3] ✓
2. [1,2]回溯 → [1] → [1,3] → [1,3,2] ✓
3. [1]回溯 → [] → [2] → [2,1] → [2,1,3] ✓
4. [2,1]回溯 → [2] → [2,3] → [2,3,1] ✓
5. [2]回溯 → [] → [3] → [3,1] → [3,1,2] ✓
6. [3,1]回溯 → [3] → [3,2] → [3,2,1] ✓
```

```go
// 经典回溯 + 递归
func permute(nums []int) [][]int {
    var result [][]int
    var path []int
    visited := make([]bool, len(nums))

    var backtrack func()
    backtrack = func() {
        // 路径长度等于数组长度，返回一个全排列的结果
        if len(path) == len(nums) {
            temp := make([]int, len(nums))
            copy(temp, path)
            result = append(result, temp)
            return
        }

        for i := 0; i < len(nums); i++ {
            // 如果没有被访问过
            if !visited[i] {
                visited[i] = true  // 标记为已访问
                path = append(path, nums[i])  // 添加到path路径中去

                backtrack()  // 递归，进入下一层

                path = path[:len(path)-1]  // 从path路径中删除
                visited[i] = false  // 取消标记
            }
        }
    }

    backtrack()
    return result
}
```



### 二分查找（6）

#### Hot 100 例题

##### ⭐⭐ 搜索插入位置（二分查找）  ✅😀

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

##### ⭐⭐⭐ 搜索二维矩阵  ✅😀

```go
​```
二维数组一维化
时间复杂度 O(log(m×n))
​```
func searchMatrix(matrix [][]int, target int) bool {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return false
    }

    m, n := len(matrix), len(matrix[0])
    left, right := 0, m*n-1

    // 边界条件，这里是 <=
    for left <= right {
        mid := (left + right) / 2
		// 主要是坐标转换
        row := mid / n
        col := mid % n

        value := matrix[row][col]
        if value == target {
            return true
        } else if value < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }

    return false
}
```



### 栈（5）

#### Hot 100 例题

##### ⭐ ⭐有效的括号 ✅😅

```go
func isValid(s string) bool {
    stack := make([]rune, 0)

    for _, char := range(s) {
        switch char {
            case '(', '[', '{':
                stack = append(stack, char)
            case ')':
                if len(stack) == 0 || stack[len(stack) - 1] != '(' {
                    return false
                }
                stack = stack[:len(stack) - 1]
            case ']':
                if len(stack) == 0 || stack[len(stack) - 1] != '[' {
                    return false
                }
                stack = stack[:len(stack) - 1]
            case '}':
                if len(stack) == 0 || stack[len(stack) - 1] != '{' {
                    return false
                }
                stack = stack[:len(stack) - 1]
        }
    }
    
    // 最后一定要检查栈是否为空
    if len(stack) != 0 {
        return false
    }
    return true
}
```

### 堆（3）

### 贪心算法（4）

#### Hot 100 例题

##### ⭐⭐⭐ 买卖股票的最佳时机 ✅😅

```go
func maxProfit(prices []int) int {
    n := len(prices)
    if n == 1 {
        return 0
    }

    // 先更新最小价格，再更新最大利润
    maxProfit := 0
    minPrice := prices[0]

    for i := 0; i < len(prices); i++ {
        if prices[i] < minPrice {
            minPrice = prices[i]
        } else {
            // 当前价格比我的最小价格高，就计算当前利润
            profit := prices[i] - minPrice
            if profit > maxProfit {
                maxProfit = profit
            }
        }
    }

    return maxProfit
}
```

### 动态规划（10）

#### Hot 100 例题

##### ⭐ 爬楼梯 ✅😅

```go
// 斐波那契数列
// 递推公式：dp[i] = dp[i-1] + dp[i-2]
func climbStairs(n int) int {
    if n == 1 {
        return 1
    }
    if n == 2 {
        return 2
    }

    n1, n2 := 1, 2
    for i:= 3; i <= n; i++ {
        n1, n2 = n2, n2 + n1
    }
    return n2
}
```

##### ⭐⭐⭐ 杨辉三角  ✅😅

```go
func generate(numRows int) [][]int {
    if numRows == 0 {
        return [][]int{}
    }

    results := make([][]int, numRows)

    for i := 0; i < numRows; i++ {
        results[i] = make([]int, i+1)

        results[i][0] = 1
        // 注意这里的边界条件
        results[i][i] = 1

        // 也注意这里的边界条件
        for j := 1; j < i; j++ {
            results[i][j] = results[i-1][j-1] + results[i-1][j]
        }
    }

    return results
}
```

##### ⭐⭐⭐⭐⭐打家劫舍 ✅😅

```go
// 用 dp[i] 表示偷窃到第 i 个房屋时能获得的最大金额

// 对于第 i 个房屋，有两种选择：
// 1. 偷窃第 i 个房屋：那么不能偷窃第 i-1 个房屋，最大金额 = dp[i-2] + nums[i]
// 2. 不偷窃第 i 个房屋：那么最大金额 = dp[i-1]
func rob(nums []int) int {
    n := len(nums)

    if n == 1 {
        return nums[n-1]
    }
    if n == 2 {
        return max(nums[n-1], nums[n-2])
    }

    dp := make([]int, n)
    dp[0] = nums[0]
    dp[1] = max(nums[1], nums[0])

    for i := 2; i < n; i++ {
        dp[i] = max(dp[i-1], dp[i-2] + nums[i])
    }
    return dp[n-1]
}

func max(a, b int) int {
    if a > b {
        return a
    } else {
        return b
    }
}
```



### 多维动态规划（5）

### 技巧（5）

##### ⭐⭐只出现一次的数字✅😅

```go
// 对于数组 [4, 1, 2, 1, 2]
// 由于异或运算满足交换律，实际上相当于：
// 4 ^ 1 ^ 2 ^ 1 ^ 2 = 4 ^ (1 ^ 1) ^ (2 ^ 2) = 4 ^ 0 ^ 0 = 4
func singleNumber(nums []int) int {
    single := 0
    for _, num := range nums {
        // 异或运算
        single ^= num
    }
    return single
}
```

##### ⭐⭐ 多数元素 ✅😅

```go
func majorityElement(nums []int) int {
    candidate := 0  // 候选元素
    count := 0  // 计数

    for _, num := range nums {
        if count == 0 {
            candidate = num  // 选取新的候选元素
        }

        if candidate == num {
            count++
        } else {
            count--
        }
    }

    return candidate  // 题目保证存在多数元素，不需要验证
}
```































