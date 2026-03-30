### 哈希算法
#### 1 基本概念
哈希是将任意长度的数据通过某种算法转换成一个固定长度的值。
 - 哈希函数：将输入数据映射到一个固定大小的输出的数学函数。对任意输入$x$，哈希函数$H(x)$会输出固定长度的值。
 - 哈希值：哈希函数的输出，用于查找、验证、加密等快速比较。
#### 2 算法应用
##### 2.1 查找匹配：两数之和
给定一个整数数组和一个目标值，找出数组中两个数之和等于目标值的索引。


```python
def twoSum(nums, target):
    # 创建一个Hash Map保存出现过的值
    hash_map = {}

    for i in range(len(nums)):
        # 计算需要找到的数字
        complement = target - nums[i]
        # 查询数字是否出现过
        if complement in hash_map:
            return [hash_map[complement], i]

        hash_map[nums[i]] = i
    return []

solution = twoSum()
print(solution.twoSum([2, 7, 11, 15], 9))
```

    [0, 1]
    

##### 2.2 去重


```python
def removeDuplicates(nums):
    hash_set = set()
    index = 0

    for num in nums:
        if num not in hash_set:
            hash_set.add(num)
            nums[index] = num
            index += 1
    return index

nums = [1, 1, 2, 2, 3, 4]
print(removeDuplicates(nums))
```

    4
    

##### 2.3 计数和频率统计：出现频率最高的元素


```python
def majorityElement(nums):
    hash_map = {}
    for i in range(len(nums)):
        if nums[i] not in hash_map:
            hash_map[nums[i]] = 1
        else:
            hash_map[nums[i]] += 1
    majorityElement, majorityCount = 0, 0
    for key, value in hash_map.items():
        if value > majorityCount:
            majorityElement, majorityCount = key, value
    return majorityElement, majorityCount
print(majorityElement([1, 1, 2, 2, 3, 4]))
```

    (1, 2)
    

##### 2.4 最长无重复子串 <font color=red>LeetCode No.3</font>
Hash算法与动态规划（双指针）的结合


```python
def longestSubstring(s):
    charSet = set()
    j = 0
    maxLength = 0

    for i in range(len(s)):
        while s[i] in charSet:
            charSet.remove(s[j])
            j += 1
        charSet.add(s[i])
        maxLength = max(maxLength, i - j + 1)
    return maxLength

print(longestSubstring("pwwkew"))
```

    3
    

##### 2.5 字母异位词分组 <font color=red>LeetCode No.49</font>


```python
def groupAnagrams(strs):
    hashMap = {}

    for s in strs:
        key = "".join(sorted(s))
        if key not in hashMap:
            hashMap[key] = []
        hashMap[key].append(s)
    return list(hashMap.values())

print(groupAnagrams(["eat", "tea", "tan", "ate", "nat", "bat"]))
```

    [['eat', 'tea', 'ate'], ['tan', 'nat'], ['bat']]
    

##### 2.6 最长连续序列 <font color=red>LeetCode No.128</font>
1. 测试样例：`nums = [100, 4, 200, 1, 3, 4, 2]`
2. 去重：`hashSet = set(nums)` -> `hashSet = [100, 4, 200, 1, 3, 2]`
3. 遍历步骤：
 100: 99 not in -> 101 not in, longestStreak = 1, continue;

 4: 3 in, longestStreak = 1, continue;

 200: 199 not in -> 201 not in, longestStreak = 1, continue;

 1: 0 not in -> 2 in -> 3 in -> 4 in, longestStreak = 4, continue;

 3: 2 in, longestStreak = 4, continue;

 2: 1 in, longestStreak = 4, continue;




```python
def longestConsecutive(nums):
    hashSet = set()
    # 去重：等价于hashSet = set(nums)
    for num in nums:
        if num not in hashSet:
            hashSet.add(num)

    currentStreak = 0
    longestStreak = 0

    for num in hashSet:
        if num-1 not in hashSet:
            currentNum = num
            currentStreak = 1

            while currentNum + 1 in hashSet:
                currentNum += 1
                currentStreak += 1

            longestStreak = max(longestStreak, currentStreak)

    return longestStreak


print(longestConsecutive([0,3,7,2,5,8,4,6,0,1]))
```

    9
    

##### 2.7 爬楼梯问题
方法一：使用Hash表

本质上是Fibnacci数列：$F(n)=F(n-1)+F(n-2)$

或：

$F(n) = \sum ^n _{i=n/2}C(i, n-i) = \sum ^n _{i=n/2}\frac{i!}{(n-i)!(2i-n)}, $

$F(n) = \sum ^n _{i=(n+1)/2}C(i, n-i) = \sum ^n _{i=(n+1)/2}\frac{i!}{(n-i)!(2i-n)}$

时间复杂度：O(n)，空间复杂度：O(n)


```python
def climbStairs(n):
    hashMap = {}
    hashMap[0] = 1
    hashMap[1] = 1

    #动态规划：爬到第i级的方法数 = 爬到第i-1级 + 爬到第i-2级
    for i in range(2, n+1):
        hashMap[i] = hashMap[i-1] + hashMap[i-2]
    return hashMap[n]

print(climbStairs(5))
```

    8
    

更优解，空间复杂度：O(1)


```python
def climbStairs(n):
    if n <= 2:
        return n
    first = 1
    second = 2
    for i in range(3, n+1):
        current = first + second
        first = second
        second = current
    return second
print(climbStairs(5))
```

    8
    

##### 2.8 判断字谜
给定两个字符串s1和s2，判断s2是否包含s1的任意一个排列作为连续子串


```python
Map = {'a':1, 'b':2, 'c':3, 'd':4}
s = 'abc'
Map[s[1]] -= 1
print(Map)
for value in Map.values():
    print(value)
```

    {'a': 1, 'b': 1, 'c': 3, 'd': 4}
    1
    1
    3
    4
    
