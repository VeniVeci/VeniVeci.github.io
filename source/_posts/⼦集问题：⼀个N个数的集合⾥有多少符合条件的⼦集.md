---
title: ⼦集问题：⼀个N个数的集合⾥有多少符合条件的⼦集
tags: leetcode 算法 dfs
categories: 四大件之数据结构和算法 回溯问题
abbrlink: c7295414
date: 2022-03-02 21:36:17
---

<!--more-->

<p><span><a href="https://blog.csdn.net/weixin_40757930/article/details/123035194" title="回溯问题其他文章（组合，分割，子集，排列）">回溯问题其他文章（组合，分割，子集，排列）</a></span></p>

<h1>给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。</h1>

<p></p>

<p>k==n，只有一个结果。因为组合不考虑顺序。</p>

<p></p>

<p>如果是返回结果有多少种，利用组合公式即可。</p>

<p></p>

<p></p>

<p><img alt="" height="584" src="https://img-blog.csdnimg.cn/f62a15d1221f4b48beaa4d2b48749408.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1200" /></p>

<p></p>

<p></p>

<pre>
<code class="language-java">class Solution {

        List&lt;List&lt;Integer&gt;&gt; res = new ArrayList&lt;&gt;();
        public List&lt;List&lt;Integer&gt;&gt; combine(int n, int k) {
            if (k &lt;= 0 || n &lt; k) {
                return res;
            }
            Deque&lt;Integer&gt; path = new ArrayDeque&lt;&gt;();
            dfs(n, k, 1, path);
            return res;
        }

        private void dfs(int n, int k, int begin, Deque&lt;Integer&gt; path) {
            // 递归终止条件是：path 的长度等于 k
            if (path.size() == k) {
                res.add(new ArrayList&lt;&gt;(path));
                return;
            }
            // 遍历可能的搜索起点
//            for (int i = begin; i &lt;= n; i++) {
                // 剪枝后   代码时间16ms -&gt; 1ms
//                剪枝代码
            for (int i = begin; i &lt;=  n - (k - path.size()) + 1; i++) {
                // 向路径变量里添加一个数
                path.addLast(i);
                // 下一轮搜索，设置的搜索起点要加 1，因为组合数理不允许出现重复的元素
                dfs(n, k, i + 1, path);
                // 重点理解这里：深度优先遍历有回头的过程，因此递归之前做了什么，递归之后需要做相同操作的逆向操作
                path.removeLast();
            }
        }

    }</code></pre>

<blockquote>
<p>(k - path.size())  代表还有几位数就凑够k位数了 用来剪枝。</p>
</blockquote>

<h1>找出所有相加之和为 n 的 k 个数的组合。组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。</h1>

<p>本题就是在[1,2,3,4,5,6,7,8,9]这个集合中找到和为n的k个数的组合。因为有和的限制，所以加一个sum判断，因为是正整数 所以可以有两处剪枝的地方。</p>

<p></p>

<pre>
<code class="language-java">class Solution {

        List&lt;List&lt;Integer&gt;&gt; res = new ArrayList&lt;&gt;();
        public List&lt;List&lt;Integer&gt;&gt; combinationSum3(int k, int n) {
            if (n &lt;= 0 || n &gt; 45) {
                return res;
            }
            Deque&lt;Integer&gt; path = new ArrayDeque&lt;&gt;();
            dfs(n, k, 0, 1, path);
            return res;
        }
        private void dfs(int target, int k, int sum, int begin, Deque&lt;Integer&gt; path) {
            if(sum &gt; target) return; // 剪枝
            if (path.size() == k) {
                if(sum == target){
                    res.add(new ArrayList&lt;&gt;(path));
                    return;
                }
            }
            for (int i = begin; i &lt;=  9 - (k - path.size()) + 1; i++) {// 剪枝
                path.addLast(i);
                sum += i;
                dfs(target, k, sum, i + 1, path);
                sum -= i;
                path.removeLast();
            }
        }

    }</code></pre>

<h1> 电话号码</h1>

<p><img alt="" height="660" src="https://img-blog.csdnimg.cn/44bb2c5a6b0e4c7da258fd9127a06694.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1125" /></p>

<p> 这道题需要收集树的所有子节点的值，无法剪枝。正常回溯即可。</p>

<pre>
<code class="language-java">class Solution {
        List&lt;String&gt; res = new ArrayList&lt;&gt;();
        String[] strs = {"","","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
        public List&lt;String&gt; letterCombinations(String digits) {
            int n = digits.length();
            if(n==0) return res;
            StringBuilder s = new StringBuilder();
            dfs(digits,0,n,s);
            return res;
        }

        public void dfs(String digits,int index,int n,StringBuilder s){
            if(index==n){
                res.add(s.toString());
                return;
            }
            int num = digits.charAt(index) - '0';
            String str = strs[num];//'abc'
            for (int i = 0; i &lt; str.length(); i++) {
                s.append(str.substring(i,i+1));
                dfs(digits,index+1,n,s);
                s.replace(s.length()-1,s.length(),"");
            }

        }
    }</code></pre>

<p></p>

<p>给定⼀个⽆重复元素的数组 candidates 和⼀个⽬标数 target ，找出 candidates 中所有可以使数字和为</p>

<p>target 的组合。</p>

<h1>candidates 中的数字可以⽆限制重复被选取</h1>

<p><img alt="" height="450" src="https://img-blog.csdnimg.cn/bba50eec00db437dbd49d0285618e4ac.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="1200" /></p>

<p></p>

<p></p>

<pre>
<code class="language-java">class Solution {
    public List&lt;List&lt;Integer&gt;&gt; combinationSum(int[] candidates, int target) {
        int len = candidates.length;
        List&lt;List&lt;Integer&gt;&gt; res = new ArrayList&lt;&gt;();
        if (len == 0) {
            return res;
        }
        // 排序是剪枝的前提
        Arrays.sort(candidates);
        Deque&lt;Integer&gt; path = new ArrayDeque&lt;&gt;();
        dfs(candidates, 0, len, target, path, res);
        return res;
    }

    private void dfs(int[] candidates, int begin, int len, int target, Deque&lt;Integer&gt; path, List&lt;List&lt;Integer&gt;&gt; res) {
        // 由于进入更深层的时候，小于 0 的部分被剪枝，因此递归终止条件值只判断等于 0 的情况
        if (target == 0) {
            res.add(new ArrayList&lt;&gt;(path));
            return;
        }

        for (int i = begin; i &lt; len; i++) {
            // 重点理解这里剪枝，前提是候选数组已经有序，
            if (target - candidates[i] &lt; 0) {
                break;// 当前这一支全都pass
            }
            path.addLast(candidates[i]);// path存放当前已经选择的队列
            dfs(candidates, i, len, target - candidates[i], path, res);
            path.removeLast();
        }
    }


}</code></pre>

<p></p>

<h1>candidates中的每个数字在每个组合中只能使用一次。</h1>

<p><img alt="" height="279" src="https://img-blog.csdnimg.cn/8f3792c1b6c84ae8a1300c28fde26968.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_18,color_FFFFFF,t_70,g_se,x_16" width="602" /></p>

<p></p>

<pre>
<code class="language-java">class Solution {
    public List&lt;List&lt;Integer&gt;&gt; combinationSum2(int[] candidates, int target) {
        List&lt;List&lt;Integer&gt;&gt; res = new ArrayList&lt;&gt;();
        if (candidates.length == 0) {
            return res;
        }
        // 排序是剪枝的前提
        Arrays.sort(candidates);
        Deque&lt;Integer&gt; path = new ArrayDeque&lt;&gt;();
        dfs(candidates, 0, target, path, res);
        return res;
    }

    private void dfs(int[] candidates, int begin, int target, Deque&lt;Integer&gt; path, List&lt;List&lt;Integer&gt;&gt; res) {
        // 由于进入更深层的时候，小于 0 的部分被剪枝，因此递归终止条件值只判断等于 0 的情况
        if (target == 0) {
            res.add(new ArrayList&lt;&gt;(path));
            return;
        }
        for (int i = begin; i &lt; candidates.length; i++) {
            // 重点理解这里剪枝，前提是候选数组已经有序，
            if (target - candidates[i] &lt; 0) {
                break;// 当前这一支全都pass
            }
            if(i&gt;begin &amp;&amp; candidates[i] == candidates[i-1] ){
                continue;//这样就不会有重复的组合 
            } // i&gt;begin 不能省略，因为 1 1 2 这种情况同一个路径上 第一个数字不需要判断
            path.addLast(candidates[i]);// path存放当前已经选择的队列
            dfs(candidates, i+1, target - candidates[i], path, res);
            path.removeLast();
        }
    }
}</code></pre>

<p></p>

<p> </p>

<h1><span style="color:#333333;"><strong>第</strong></span><span style="color:#333333;"><strong>78</strong></span><span style="color:#333333;"><strong>题</strong></span><span style="color:#333333;"><strong>. </strong></span><span style="color:#333333;"><strong>⼦集</strong></span></h1>

<p></p>

<p><img alt="" height="661" src="https://img-blog.csdnimg.cn/ae3b110c4a9c4d279fd5ed06d164afc1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="940" /></p>

<p></p>

<pre>
<code class="language-java">
class Solution {
        List&lt;Integer&gt; t = new ArrayList&lt;Integer&gt;();
        List&lt;List&lt;Integer&gt;&gt; ans = new ArrayList&lt;List&lt;Integer&gt;&gt;();
        //[[1,2,3],[1,2, ],[1,3, ],[1, , ],[2,3, ],[2, , ],[3, , ],[]]
        public List&lt;List&lt;Integer&gt;&gt; subsets(int[] nums) {
            dfs(0, nums);
            return ans;
        }

        public void dfs(int startIdx, int[] nums) {
            ans.add(new ArrayList&lt;&gt;(t));
            if(startIdx &gt;= nums.length)return;
            for (int i = startIdx; i &lt; nums.length; i++) {
                t.add(nums[i]);
                dfs(i+1, nums);
                t.remove(t.size() - 1);
            }
        }
}</code></pre>

<p></p>

<h1>90.子集II</h1>

<p><img alt="" height="396" src="https://img-blog.csdnimg.cn/deedef13014d4a4e949d31f81bb0e8ff.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_18,color_FFFFFF,t_70,g_se,x_16" width="606" /></p>

<p></p>

<pre>
<code class="language-java">class Solution {
   List&lt;List&lt;Integer&gt;&gt; result = new ArrayList&lt;&gt;();// 存放符合条件结果的集合
   LinkedList&lt;Integer&gt; path = new LinkedList&lt;&gt;();// 用来存放符合条件结果
    public List&lt;List&lt;Integer&gt;&gt; subsetsWithDup(int[] nums) {
        if (nums.length == 0){
            result.add(path);
            return result;
        }
        Arrays.sort(nums);
        subsetsWithDupHelper(nums, 0);
        return result;
    }
    
    private void subsetsWithDupHelper(int[] nums, int startIndex){
        result.add(new ArrayList&lt;&gt;(path));
        if (startIndex &gt;= nums.length){
            return;
        }
        for (int i = startIndex; i &lt; nums.length; i++){
            if (i &gt; startIndex &amp;&amp; nums[i] == nums[i - 1]){
                continue;
            }
            path.add(nums[i]);
            subsetsWithDupHelper(nums, i + 1);
            path.removeLast();
        }
    }
}
</code></pre>

<p></p>

<h1><span style="color:#333333;"><strong>491.</strong></span><span style="color:#333333;"><strong>递增⼦序列</strong></span></h1>

<p><img alt="" height="433" src="https://img-blog.csdnimg.cn/c246a3de63ce461b88e183c70deb4e47.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_18,color_FFFFFF,t_70,g_se,x_16" width="613" /></p>

<p><strong>  4 6 7 8 7 9</strong></p>

<pre>
<code class="language-java">class Solution {
    private List&lt;Integer&gt; path = new ArrayList&lt;&gt;();
    private List&lt;List&lt;Integer&gt;&gt; res = new ArrayList&lt;&gt;();
    public List&lt;List&lt;Integer&gt;&gt; findSubsequences(int[] nums) {
        backtracking(nums,0);
        return res;
    }

    private void backtracking (int[] nums, int start) {
        if (path.size() &gt; 1) {
            res.add(new ArrayList&lt;&gt;(path));
        }

        int[] used = new int[201];
        for (int i = start; i &lt; nums.length; i++) {
            if (!path.isEmpty() &amp;&amp; nums[i] &lt; path.get(path.size() - 1) ||
                    (used[nums[i] + 100] == 1)) continue;
            used[nums[i] + 100] = 1;
            path.add(nums[i]);
            backtracking(nums, i + 1);
            path.remove(path.size() - 1);
        }
    }
}

</code></pre>

<p></p>
