# 作业1
给你一个 非空 整数数组 nums ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

如果用哈希表的方式，虽然不符合题目对**常量额外空间**的要求（因为哈希表需要额外空间），但思路非常直观，可以按以下步骤实现：

---
## 实现思路

### **哈希表实现思路（不考虑空间限制）**

#### **1. 统计频率**
- 创建一个哈希表（如 Python 的 `dict` 或 Java 的 `HashMap`），键是数组中的数字，值是该数字出现的次数。
- 遍历数组 `nums`，对每个数字 `num`，更新哈希表：
  - 如果 `num` 不在哈希表中，添加 `num: 1`；
  - 如果 `num` 已存在，将计数加 1（即 `count[num] += 1`）。

#### **2. 查找唯一元素**
- 再次遍历哈希表，找到计数为 1 的键，即为答案。

---

### **示例**
以 `nums = [4, 1, 2, 1, 2]` 为例：

1. **统计频率**：
   - 遍历数组：
     - 4: 出现 1 次 → `{4: 1}`
     - 1: 出现 1 次 → `{4: 1, 1: 1}`
     - 2: 出现 1 次 → `{4: 1, 1: 1, 2: 1}`
     - 1: 出现 2 次 → `{4: 1, 1: 2, 2: 1}`
     - 2: 出现 2 次 → `{4: 1, 1: 2, 2: 2}`
   - 最终哈希表：`{4: 1, 1: 2, 2: 2}`

2. **查找唯一元素**：
   - 遍历哈希表，发现 `4` 的计数是 1，返回 `4`。

---

### **复杂度**
- **时间复杂度**：O(n)（遍历数组一次 + 遍历哈希表一次）。
- **空间复杂度**：O(n)（最坏情况下哈希表存储 n/2 + 1 个键值对）。

---

### **代码框架（伪代码）**
```python
def find_unique(nums):
    count = {}
    for num in nums:
        count[num] = count.get(num, 0) + 1
    for num, freq in count.items():
        if freq == 1:
            return num
```

---

### **总结**
哈希表方法简单直接，但需要额外空间。如果**放宽空间限制**，这是最优解之一；但题目要求**常量空间**，因此需改用**异或操作**（O(1) 空间）。


# 作业2
考察：数字操作、条件判断
题目：判断一个整数是否是回文数 

### 思路：判断整数是否是回文数（不转成字符串）

**核心思想**：  
将整数的后半部分数字**反转**，然后与前半部分比较。如果相等，则是回文数。

---

#### **步骤分解**：
1. **处理特殊情况**：
   - 负数：直接返回 `false`（负号 `-` 不是数字，无法对称）。
   - 末尾是 0 的非零数：直接返回 `false`（如 10、100，反转后变成 01、001，不对称）。

2. **反转后半部分数字**：
   - 初始化 `reversed = 0`（存储反转的后半部分）。
   - 循环终止条件：当 `x <= reversed` 时，说明已经反转了一半的数字。
     - 每次循环：
       - 取出 `x` 的最后一位：`last_digit = x % 10`。
       - 将 `last_digit` 拼接到 `reversed`：`reversed = reversed * 10 + last_digit`。
       - 去掉 `x` 的最后一位：`x = x // 10`。

3. **比较前后两半**：
   - 偶数位：直接比较 `x == reversed`（如 1221 → 反转后为 12 和 12）。
   - 奇数位：去掉 `reversed` 的最后一位再比较（如 12321 → 反转后为 12 和 123，去掉 3 后比较 12 和 12）。

---

#### **示例验证**：
- **输入 121**：
  - 初始 `x=121`, `reversed=0`。
  - 第1次：`last_digit=1`, `reversed=1`, `x=12`。
  - 第2次：`last_digit=2`, `reversed=12`, `x=1`。
  - 终止（`x=1 <= reversed=12`）。此时 `x=1`, `reversed=12`（奇数位），去掉 `reversed` 的最后一位得 `1`，比较 `1 == 1`，返回 `true`。

- **输入 10**：
  - 末尾是 0，直接返回 `false`。

- **输入 -121**：
  - 负数，直接返回 `false`。


# 作业3
题目：给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效
### 思路：用栈解决有效的括号匹配  
核心思想：**遇到左括号就压栈，遇到右括号就与栈顶匹配并弹出栈顶**。如果最终栈为空，则括号有效。

---

#### **步骤分解**  
1. **初始化一个空栈**。  
2. **遍历字符串**：
   - **左括号**（`(`、`[`、`{`）→ **压栈**。  
   - **右括号**（`)`、`]`、`}`）→ **检查栈顶**：
     - 栈为空 → 无效（右括号多余）。  
     - 栈顶不匹配 → 无效（顺序错误）。  
     - 栈顶匹配 → **弹出栈顶**（成功配对）。  
3. **最终检查栈是否为空**：
   - 空栈 → 所有括号匹配完成，有效。  
   - 非空 → 左括号多余，无效。

---

#### **示例验证**  
- **输入 `"()[]{}"`**：
  - `'('` → 压栈 → 栈：`['(']`  
  - `')'` → 匹配 `'('` → 弹出 → 栈：`[]`  
  - `'['` → 压栈 → 栈：`['[']`  
  - `']'` → 匹配 `'['` → 弹出 → 栈：`[]`  
  - `'{'` → 压栈 → 栈：`['{']`  
  - `'}'` → 匹配 `'{'` → 弹出 → 栈：`[]`  
  - **栈为空** → 有效。

- **输入 `"([)]"`**：
  - `'('` → 压栈 → `['(']`  
  - `'['` → 压栈 → `['(', '[']`  
  - `')'` → 栈顶是 `'['` → 不匹配 → **无效**。

---

#### **边界情况**  
- 空字符串 → 有效（无括号）。  
- 仅左括号 → 无效（栈非空）。  
- 仅右括号 → 无效（栈空时遇到右括号）。

---

### **伪代码**
```text
function isValid(s):
    stack = empty list
    mapping = {')': '(', ']': '[', '}': '{'}

    for char in s:
        if char in mapping.values():  # 左括号
            push char to stack
        else if char in mapping.keys():  # 右括号
            if stack is empty or pop() != mapping[char]:
                return false
        else:
            return false  # 非法字符（题目保证无此情况）

    return stack is empty
```

---

### **复杂度**  
- **时间**：O(n)（遍历一次字符串）。  
- **空间**：O(n)（最坏情况下栈存所有左括号）。
---

#### **关键点**：
- **不转成字符串**：全程用数学运算（取余 `%` 和整除 `//`）。
- **O(1) 空间**：仅用两个变量 `x` 和 `reversed`。
- **O(log n) 时间**：数字的位数是 log₁₀(n) 级别。


# 作业4
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

 

示例 1：

输入：strs = ["flower","flow","flight"]
输出："fl"
示例 2：

输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
### 思路（先讲思路，后给代码）

1. **特殊情况**  
   - 空数组 `[]` → 直接返回 `""`  
   - 只有一个字符串 → 该字符串就是公共前缀  

2. **纵向扫描（按列比较）**  
   - 把第一个字符串 `strs[0]` 当作“基准前缀”。  
   - 从第 2 个字符串开始，逐个检查每个字符是否与基准前缀同一位置的字符相同。  
   - 一旦发现某一位不匹配，立即把基准前缀截断到这一位之前。  
   - 过程中如果基准前缀被截成空串，可提前结束。  

3. **复杂度**  
   - 时间：O(S) —— S 是所有字符总数；最坏情况下每个字符都被比较一次。  
   - 空间：O(1) —— 只用到常数个额外变量。

---

### Go 代码

```go
package main

import "fmt"

func longestCommonPrefix(strs []string) string {
	if len(strs) == 0 {
		return ""
	}
	// 取第一个字符串作为初始前缀
	prefix := strs[0]

	// 从第二个字符串开始比较
	for i := 1; i < len(strs); i++ {
		s := strs[i]
		// 把 prefix 截短到与 s 相同长度的最小前缀
		j := 0
		for j < len(prefix) && j < len(s) && prefix[j] == s[j] {
			j++
		}
		prefix = prefix[:j]
		if prefix == "" {
			break
		}
	}
	return prefix
}

func main() {
	fmt.Println(longestCommonPrefix([]string{"flower", "flow", "flight"})) // "fl"
	fmt.Println(longestCommonPrefix([]string{"dog", "racecar", "car"}))    // ""
}
```

# 作业5
给定一个表示 大整数 的整数数组 digits，其中 digits[i] 是整数的第 i 位数字。这些数字按从左到右，从最高位到最低位排列。这个大整数不包含任何前导 0。

将大整数加 1，并返回结果的数字数组。

 

示例 1：

输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
加 1 后得到 123 + 1 = 124。
因此，结果应该是 [1,2,4]。
示例 2：

输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。
加 1 后得到 4321 + 1 = 4322。
因此，结果应该是 [4,3,2,2]。
示例 3：

输入：digits = [9]
输出：[1,0]
解释：输入数组表示数字 9。
加 1 得到了 9 + 1 = 10。
因此，结果应该是 [1,0]。

### 思路（先讲思路，后给代码）

1. **从右往左逐位加 1**  
   - 把数组当成大整数的各位数字，最右边是个位。  
   - 从最低位（末尾）开始 `+1`。  
   - 若当前位加 1 后小于 10，直接返回即可。  
   - 若等于 10，则该位变成 0，并向前一位继续进位（循环处理）。  

2. **处理最高位进位**  
   - 如果循环结束仍有进位（例如 `[9,9] → [1,0,0]`），需要在数组最前面插入一个 `1`。

3. **复杂度**  
   - 时间：O(n)，最坏遍历整个数组一次。  
   - 空间：O(1) 额外空间（若允许返回新切片，则最坏 O(n) 当需要扩容时）。

---

### Go 代码

```go
package main

import "fmt"

func plusOne(digits []int) []int {
	n := len(digits)
	// 从最低位开始处理
	for i := n - 1; i >= 0; i-- {
		if digits[i] < 9 { // 无进位，直接加1返回
			digits[i]++
			return digits
		}
		// 当前位是9，加1变0，继续向前进位
		digits[i] = 0
	}
	// 所有位都是9，需要扩容
	return append([]int{1}, digits...)
}

func main() {
	fmt.Println(plusOne([]int{1, 2, 3})) // [1 2 4]
	fmt.Println(plusOne([]int{4, 3, 2, 1})) // [4 3 2 2]
	fmt.Println(plusOne([]int{9})) // [1 0]
	fmt.Println(plusOne([]int{9, 9})) // [1 0 0]
}
```


# 作业6
给你一个 非严格递增排列 的数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。元素的 相对顺序 应该保持 一致 。然后返回 nums 中唯一元素的个数。

考虑 nums 的唯一元素的数量为 k ，你需要做以下事情确保你的题解可以被通过：

更改数组 nums ，使 nums 的前 k 个元素包含唯一元素，并按照它们最初在 nums 中出现的顺序排列。nums 的其余元素与 nums 的大小不重要。
返回 k 。
判题标准:

系统会用下面的代码来测试你的题解:

int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
如果所有断言都通过，那么您的题解将被 通过。

 

示例 1：

输入：nums = [1,1,2]
输出：2, nums = [1,2,_]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。
示例 2：

输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4]
解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。

思路（先讲思路，后给代码）

1. 数组已经**非严格递增**，重复元素必然相邻。  
2. **双指针**  
   - `slow`：指向当前**已去重区间的末尾**（初始 0）。  
   - `fast`：遍历整个数组（从 1 开始）。  
3. **比较与复制**  
   - 若 `nums[fast] != nums[slow]`，说明遇到新的唯一值：  
     - `slow++` 先移动一位，再把 `nums[fast]` 复制到 `nums[slow]`。  
   - 否则跳过重复值。  
4. 结束时 `slow + 1` 就是唯一元素个数 `k`，且前 `k` 个位置已按要求排好。  

时间复杂度：O(n)  
空间复杂度：O(1)（原地修改）

Go 代码
```go
package main

import "fmt"

func removeDuplicates(nums []int) int {
	if len(nums) == 0 {
		return 0
	}
	slow := 0 // 已去重区间的末尾
	for fast := 1; fast < len(nums); fast++ {
		if nums[fast] != nums[slow] {
			slow++
			nums[slow] = nums[fast]
		}
	}
	return slow + 1
}

func main() {
	nums1 := []int{1, 1, 2}
	k1 := removeDuplicates(nums1)
	fmt.Println(k1, nums1[:k1]) // 2 [1 2]

	nums2 := []int{0, 0, 1, 1, 1, 2, 2, 3, 3, 4}
	k2 := removeDuplicates(nums2)
	fmt.Println(k2, nums2[:k2]) // 5 [0 1 2 3 4]
}
```

# 作业7
以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间 。

 

示例 1：

输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
示例 2：

输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。
思路（先讲思路，后给代码）

1. 先**按区间左端点升序排序**；这样所有可能重叠的区间都会变成相邻。  
2. **扫描合并**  
   - 用 `merged [][]int` 保存结果。  
   - 依次取每个区间 `[curStart, curEnd]`：  
     - 若结果为空，或当前区间与 `merged` 最后一个区间**不重叠**（`curStart > lastEnd`），则直接追加。  
     - 否则**合并**：把最后一个区间的右端点更新为 `max(lastEnd, curEnd)`。  
3. 返回 `merged`。

时间复杂度  
- 排序：O(n log n)  
- 合并：O(n)  
整体：O(n log n)

空间复杂度  
- 排序可能用 O(log n) 递归栈，合并结果 O(n)。

Go 代码
```go
package main

import (
	"fmt"
	"sort"
)

func merge(intervals [][]int) [][]int {
	if len(intervals) == 0 {
		return nil
	}

	// 1. 按区间左端点排序
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][0] < intervals[j][0]
	})

	merged := [][]int{}
	for _, cur := range intervals {
		curStart, curEnd := cur[0], cur[1]
		if len(merged) == 0 || curStart > merged[len(merged)-1][1] {
			// 不重叠，直接加入
			merged = append(merged, []int{curStart, curEnd})
		} else {
			// 重叠，合并右端点
			last := merged[len(merged)-1]
			last[1] = max(last[1], curEnd)
		}
	}
	return merged
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	fmt.Println(merge([][]int{{1, 3}, {2, 6}, {8, 10}, {15, 18}}))
	// [[1 6] [8 10] [15 18]]

	fmt.Println(merge([][]int{{1, 4}, {4, 5}}))
	// [[1 5]]
}
```

# 作业8
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。

你可以按任意顺序返回答案。

 

示例 1：

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
示例 2：

输入：nums = [3,2,4], target = 6
输出：[1,2]
示例 3：

输入：nums = [3,3], target = 6
输出：[0,1]
思路（先讲思路，后给代码）

1. **一遍哈希表（最优）**  
   - 遍历数组时，把每个元素及其下标存入哈希表 `map[num]index`。  
   - 对于当前元素 `x`，只需检查 `target - x` 是否已在哈希表中：  
     - 若存在，直接返回这对下标。  
     - 若不存在，把当前 `(x, index)` 存进哈希表继续往后扫。  
   - 时间复杂度 O(n)，空间复杂度 O(n)。  

2. **边界**  
   - 题目保证**唯一答案**，无需处理多解或重复元素。  

Go 代码（一遍哈希）
```go
package main

import "fmt"

func twoSum(nums []int, target int) []int {
	m := make(map[int]int) // value -> index
	for i, v := range nums {
		if j, ok := m[target-v]; ok {
			return []int{j, i}
		}
		m[v] = i
	}
	return nil // 题目保证有解，不会走到这里
}

func main() {
	fmt.Println(twoSum([]int{2, 7, 11, 15}, 9)) // [0 1]
	fmt.Println(twoSum([]int{3, 2, 4}, 6))      // [1 2]
	fmt.Println(twoSum([]int{3, 3}, 6))         // [0 1]
}
```
