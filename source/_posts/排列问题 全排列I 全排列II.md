---
title: 排列问题 全排列I 全排列II
tags: 深度优先 java 算法
categories: 四大件之数据结构和算法 回溯问题
abbrlink: 866dc3d7
date: 2022-03-02 21:58:49
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

<h1><span style="color:#333333;"><strong>46.</strong></span><span style="color:#333333;"><strong>全排列</strong></span></h1>

<p><img alt="" height="573" src="https://img-blog.csdnimg.cn/c3bd58fc6e5c4413b8acd83dd17d8021.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="807" /></p>

<p>要用used 数组标定本层是否选过某个数字了，那么在下一次就跳过它。</p>

<pre>
<code class="language-java">class Solution {
        List&lt;List&lt;Integer&gt;&gt; res = new ArrayList&lt;&gt;();
        public List&lt;List&lt;Integer&gt;&gt; permute(int[] nums) {
            int len = nums.length;
            // 使用一个动态数组保存所有可能的全排列
            if (len == 0) {
                return res;
            }
            boolean[] used = new boolean[len];
            Deque&lt;Integer&gt; path = new ArrayDeque&lt;&gt;(len);
            dfs(nums, len, 0, path, used);
            return res;
        }

        private void dfs(int[] nums, int len, int depth,
                         Deque&lt;Integer&gt; path, boolean[] used) {
            if (depth == len) {
                res.add(new ArrayList&lt;&gt;(path));
                return;
            }
            for (int i = 0; i &lt; len; i++) {
                if (!used[i]) {
                    path.addLast(nums[i]);
                    used[i] = true;

                    // System.out.println("  递归之前 =&gt; " + path);
                    dfs(nums, len, depth + 1, path, used);

                    used[i] = false;
                    path.removeLast();
                    // System.out.println("递归之后 =&gt; " + path);
                }
            }
        }

    }</code></pre>

<h1> <span style="color:#333333;"><strong>47.</strong></span><span style="color:#333333;"><strong>全排列</strong></span><span style="color:#333333;"><strong> II</strong></span></h1>

<p><img alt="" height="375" src="https://img-blog.csdnimg.cn/9c72cc0d257e4e5b866eec41e34eab51.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="927" /></p>

<p></p>

<pre>
<code class="language-java">class Solution {
    boolean[] vis;
    List&lt;List&lt;Integer&gt;&gt; ans = new ArrayList&lt;List&lt;Integer&gt;&gt;();
    List&lt;Integer&gt; perm = new ArrayList&lt;Integer&gt;();

    public List&lt;List&lt;Integer&gt;&gt; permuteUnique(int[] nums) {
        vis = new boolean[nums.length];
        Arrays.sort(nums);
        backtrack(nums, 0);
        return ans;
    }
    public void backtrack(int[] nums, int idx) {
        if (idx == nums.length) {
            ans.add(new ArrayList&lt;Integer&gt;(perm));
            return;
        }
        for (int i = 0; i &lt; nums.length; ++i) {
                                
            if (vis[i] || (i &gt; 0  &amp;&amp;  nums[i] == nums[i - 1]  &amp;&amp;  !vis[i - 1])   ) {
                //  nums[2]  nums[1] 相邻两个一样 i不是初始值  前一个索引没被锁住（说明已经选过了）
                continue; // i=0的时候不会阻拦
            }

            perm.add(nums[i]);
            vis[i] = true;
            backtrack(nums, idx + 1);
            vis[i] = false;
            perm.remove(idx);
        }
    }

}</code></pre>

<p></p>

<p></p>

<p></p>

<p></p>
