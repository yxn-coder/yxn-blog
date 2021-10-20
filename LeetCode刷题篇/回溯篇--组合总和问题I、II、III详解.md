## 1、附上题目链接

[39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

[40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

[216. 组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/)


## 2、代码关键点详解


### 2.1 组合总和问题

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new ArrayList<>();

    Arrays.sort(candidates);//排序

    backtrack(candidates, target, 0, 0, res, path);
    return res;
}

public void backtrack(int[] candidates, int target, int sum, int begin, List<List<Integer>> res, List<Integer> path) {
	// base case
    if (sum == target) {
        res.add(new ArrayList<>(path));
        return;
    }
    if (sum > target) return;

	// 回溯
	// i = begin 保证解集不能包含重复的组合
    for (int i = begin; i < candidates.length; i++) { 
		// 剪枝，前提是候选数组已经有序
        if(candidates[i] > target) break; 
        
        sum += candidates[i];
        path.add(candidates[i]);
        backtrack(candidates, target, sum, i, res, path); // i 保证 candidates 中的数字可以无限制重复被选取
        path.remove(path.size() - 1);
        sum -= candidates[i];
    }
}
```
**关键点1：剪枝**

 - `Arrays.sort(canList itemdidates);//排序` 排序后，`if(candidates[i] > target) break; // 剪枝，前提是候选数组已经有序`可以直接将大于目标值的数组元素进行剪枝

**关键点2：解集不能包含重复的组合**

 - 添加搜索起点，`i = begin` 保证每次都从begin之后的元素开始搜索，不会向前搜索，保证不会出现重复集合的情况

**关键点3：candidates 中的数字可以无限制重复被选取**

 - `backtrack(candidates, target, sum, i, res, path);` 每次回溯时，搜索起点都应从 i 开始，保证了candidates 中的数字可以无限制重复被选取。注意：如果从 i + 1开始的话，那么就是保证元素不可以被重复被选取了

### 2.2、组合总和 II 问题

```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        Arrays.sort(candidates);//排序
        backtrack(candidates, target, 0, 0, res, path);
        return res;
    }

    public void backtrack(int[] candidates, int target, int sum, int begin, List<List<Integer>> res, List<Integer> path) {
    	// base case
        if (sum == target) {
            res.add(new ArrayList<>(path));
            return;
        }
        if (sum > target) return;

		// 回溯
		// i = begin 保证解集不能包含重复的组合
        for (int i = begin; i < candidates.length; i++) { 
            // 剪枝，前提是候选数组已经有序
            if (candidates[i] > target) break;
            if (i > begin && candidates[i] == candidates[i - 1]) continue; // 避免相同的情况筛选两次！！！！！
            
            sum += candidates[i];
            path.add(candidates[i]);
            backtrack(candidates, target, sum, i + 1, res, path); // i + 1 保证candidates 中的每个数字在每个组合中只能使用一次
            path.remove(path.size() - 1);
            sum -= candidates[i];
        }
    }
}
```
**关键点1：剪枝**

 - 和上一题相同。略

**关键点2：解集不能包含重复的组合**

 - 和上一题相同。略

**关键点3：candidates 中的每个数字在每个组合中只能使用一次。**

 - `backtrack(candidates, target, sum, i + 1, res, path);` 每次回溯时，搜索起点都应从 i  + 1开始，保证了candidates 中的每个数字在每个组合中只能使用一次。***注意该点和上一题不同***

 **关键点4：去重（重要）**

 - 由于 candidates 数组中元素是重复的，但是又需要解集中不能出现重复集合，此题的关键点在于如何去重。一般情况下，我们采用判断的方式，当发现相同值时，跳过该元素`if (i > begin && candidates[i] == candidates[i - 1]) continue; // 避免相同的情况筛选两次！！！！！`

### 2.3、组合总和 III 问题

```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        backtrack(k, n, 0, 1, res, path);
        return res;
    }

    public void backtrack(int k, int target, int sum, int begin, List<List<Integer>> res, List<Integer> path) {
    	// base case
        if (sum == target && path.size() == k) {
            res.add(new ArrayList<>(path));
            return;
        }
        if (sum > target) return;
        if (path.size() > k) return;

		// 回溯
		// i = begin 保证每种组合中不存在重复的数字
        for (int i = begin; i <= 9; i++) { 
            if (i > target) break; // 剪枝

            sum += i;
            path.add(i);
            backtrack(k, target, sum, i + 1, res, path); // i + 1 保证每种组合中不存在重复的数字
            path.remove(path.size()- 1);
            sum -= i;
        }
    }
}
```
**关键点1：剪枝**

 - 和上一题相同。略

**关键点2：解集不能包含重复的组合**

 - 和上一题相同。略

**关键点3：每种组合中不存在重复的数字**

 - `backtrack(k, target, sum, i + 1, res, path);` 每次回溯时，搜索起点都应从 i  + 1开始，保证了每种组合中不存在重复的数字。***和上一题思路相同***

 **关键点4：停止条件**

 - 由于此题有集合存在长度的限制，所以停止条件增加一个，就是当长度不满足的时候`if (path.size() > k) return;`，直接返回。