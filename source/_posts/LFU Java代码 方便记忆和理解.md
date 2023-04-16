---
title: LFU Java代码 方便记忆和理解
tags: 缓存 数据结构 算法
categories: 四大件之数据结构和算法
abbrlink: 7c930e4b
date: 2022-03-07 17:25:43
---

<!--more-->

<p>方便记忆和理解的LFU Java代码</p>

<p id="main-toc"><strong>目录</strong></p>

<p id="LFU%E7%AE%80%E4%BB%8B-toc" style="margin-left:0px;"><a href="#LFU%E7%AE%80%E4%BB%8B">LFU简介</a></p>

<p id="%C2%A0put%E9%80%BB%E8%BE%91-toc" style="margin-left:40px;"><a href="#%C2%A0put%E9%80%BB%E8%BE%91"> put逻辑</a></p>

<p id="%E4%BB%A3%E7%A0%81-toc" style="margin-left:0px;"><a href="#%E4%BB%A3%E7%A0%81">代码（结合了两份代码的优点）</a></p>

<hr id="hr-toc" /><p></p>

<h1 id="LFU%E7%AE%80%E4%BB%8B">LFU简介</h1>

<p>LFU 算法相当于是淘汰访问频次最低的数据，如果访问频次最低的数据有多条，需要淘汰最旧的数据。</p>

<p>最不经常使用（LFU） Least Frequently Used</p>

<p><img alt="" height="532" src="https://img-blog.csdnimg.cn/af3ac31145d1482f97c94402668b9ea7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_18,color_FFFFFF,t_70,g_se,x_16" width="608" /></p>

<h2 id="%C2%A0put%E9%80%BB%E8%BE%91"> put逻辑</h2>

<p>参考 labuladong</p>

<p><img alt="" height="760" src="https://img-blog.csdnimg.cn/17533e91bd59492d907f59022e15b977.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16" width="900" /></p>

<p> </p>

<h1 id="%E4%BB%A3%E7%A0%81">代码（结合了两份代码的优点）</h1>

<p><a data-link-desc="labuladong 出品，必属精品！" data-link-icon="https://res.wx.qq.com/a/wx_fed/assets/res/NTI4MWU5.ico" data-link-title="算法题就像搭乐高：手把手带你拆解 LFU 算法" href="https://mp.weixin.qq.com/s?__biz=MzAxODQxMDM0Mw==&amp;mid=2247486545&amp;idx=1&amp;sn=315ebfafa82c0dd3bcd9197eb270a7b6&amp;scene=21#wechat_redirect" title="算法题就像搭乐高：手把手带你拆解 LFU 算法">算法题就像搭乐高：手把手带你拆解 LFU 算法</a></p>

<p><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.7/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1L8" data-link-title="力扣" href="https://leetcode-cn.com/problems/lfu-cache/solution/lfuhuan-cun-by-leetcode-solution/" title="力扣">力扣</a></p>

<p>结合了两份代码的优点：</p>

<p><strong>labuladong代码好理解，不过用了3个hash表，有些繁琐。</strong></p>

<p><strong>力扣官方给出的题解用了Node将频率作为缓存的一个属性，不过代码有冗余。</strong></p>

<pre>
<code class="language-java">class LFUCache {
    int minfreq, capacity;// o1得到最小频率 方便快速删除
    Map&lt;Integer, Node&gt; keyTable; // 键 + 值，以及对应的频率
    Map&lt;Integer, LinkedList&lt;Node&gt;&gt; freqTable;
    // 频次 + 节点链表 通过频率可以得到该频率下的所有缓存
    // 如果只有一个缓存  那么删除即可
    // 如果有多个 从缓存中找到 最近最久未使用的 删除
    public LFUCache(int capacity) {
        this.minfreq = 0;
        this.capacity = capacity;
        keyTable = new HashMap&lt;&gt;();;
        freqTable = new HashMap&lt;&gt;();
    }

    public int get(int key) {
        if (capacity == 0 || !keyTable.containsKey(key)) {
            return -1;
        }
        Node node = keyTable.get(key);
        increaseFreq(key);
        return node.val;
    }

    private void increaseFreq(int key) {
        Node node = keyTable.get(key);
        int val = node.val, freq = node.freq;
        freqTable.get(freq).remove(node);//从频率为 freq 的map中 清除 当前节点
        // 如果当前链表为空，我们需要在哈希表中删除，且更新minFreq
        if (freqTable.get(freq).size() == 0) {
            freqTable.remove(freq);
            if (minfreq == freq) {// 如果当前被get 的节点是 minfreq 对应的唯一节点 那么minfreq需要++
                minfreq += 1;
            }
        }
        // 插入到 freq + 1 中
        LinkedList&lt;Node&gt; list = freqTable.getOrDefault(freq + 1, new LinkedList&lt;&gt;());
        Node tmpNode = new Node(key, val, freq + 1);
        list.offerFirst(tmpNode);// 头插
        freqTable.put(freq + 1, list);
        keyTable.put(key, tmpNode);// 仍然要更新keyTable 因为原节点的频率变了
    }

    public void put(int key, int value) {
        if (capacity == 0) {
            return;
        }
        if(keyTable.containsKey(key)){
            // 与 get 操作基本一致，除了需要更新缓存的值
            Node node = keyTable.get(key);
            node.val = value;// 更新值
            keyTable.put(key,node);
            increaseFreq(key);
            return;
        }
        // 缓存里没有  如果缓存已满，需要先进行删除操作  再新建node添加
        if (keyTable.size() == capacity) {
            // 通过 minFreq 拿到 freqTable[minFreq] 链表的末尾节点
            Node node = freqTable.get(minfreq).peekLast();
            keyTable.remove(node.key);
            freqTable.get(minfreq).pollLast();
            if (freqTable.get(minfreq).size() == 0) {
                freqTable.remove(minfreq);
            }// 频率为minfreq的链表没有信息了  就把这个链表删掉
        }
        // 拿到 频率为1的链表  如果没有就新建一个
        LinkedList&lt;Node&gt; list = freqTable.getOrDefault(1, new LinkedList&lt;Node&gt;());
        list.offerFirst(new Node(key, value, 1));// 头插
        freqTable.put(1, list);// 更新频率为1的 频率hashmap
        keyTable.put(key, freqTable.get(1).peekFirst());
        minfreq = 1;
    }
    class Node {
        int key, val, freq;
        Node(int key, int val, int freq) {
            this.key = key;
            this.val = val;
            this.freq = freq;
        }
    }
}</code></pre>

<p></p>
