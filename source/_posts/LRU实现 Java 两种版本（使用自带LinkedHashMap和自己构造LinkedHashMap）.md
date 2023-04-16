---
title: LRU实现 Java 两种版本（使用自带LinkedHashMap和自己构造LinkedHashMap）
tags: 网络 缓存
categories: 四大件之数据结构和算法
abbrlink: c08d4a99
date: 2022-03-07 16:21:53
---

<!--more-->



# LRU简介
LRU 算法就是一种缓存淘汰策略

计算机的缓存容量有限，如果缓存满了就要删除一些内容，给新内容腾位置。但问题是，删除哪些内容呢？我们肯定希望删掉哪些没什么用的缓存，而把有用的数据继续留在缓存里，方便之后继续使用。那么，什么样的数据，我们判定为「有用的」的数据呢？

LRU 的全称是 Least Recently Used，也就是说我们认为最近使用过的数据应该是是「有用的」，很久都没用过的数据应该是无用的，内存满了就优先删那些很久没用过的数据。

当然还有其他缓存淘汰策略，比如不要按访问的时序来淘汰，而是按访问频率（LFU 策略）来淘汰等等，各有应用场景。


# 缓存是什么？ 有什么应用场景？
[缓存介绍以及应用场景](https://blog.csdn.net/lht337636295/article/details/106162273)

##  什么是缓存，缓存的作用是什么？
（1）缓存是数据交互的缓冲区域，简称cache，当某一个硬件想要读取数据是，会首选从缓存中获取数据，有则直接执行，或者返回，如果没有，去内存中获取。缓存的数据比内存的数据快很多。所以缓存的作用就是让硬件更快速的运行
缓存基本上都是RAM，即断电即掉的非永久性存储，所以一般使用完后后，会将数据写入内存中去。
高速缓存猪主要是用来协调CPU和朱村之间的存取速度的差异而设计的。一般CPU工作速度高，而存储工作速度低，为了解决这个问题，通常使用高速缓存。一般高速缓存的速度在于CPU和内存之间。
## 缓存的作用
缓存在不同场景下作用不一样
操作系统磁盘缓存  ————》减少磁盘机械操作
数据库缓存             ————》减少对数据库的查询
Web服务器缓存     ————》减少对应用服务器的请求。
客户端浏览器缓存 ————》减少对网站的访问。

## 常见的缓存有哪些？如何做到数据库与缓存中的数据一致。
由于不同系统的数据访问模式不同，同一种缓存策略很难在不同的场景下取得满意的性能。缓存的策略有
1）基于访问时间：LRU，此类算法按照访问时间来组织缓存队列。决定替换对象。
2）基于访问频率：LFU/LRU2/2Q,
3)  基于时间和访问频次：FBR,LRUF,ALRUF,此类算法有一个可调或自适应参数，通过该参数的调节缓存策略在两个之间取得一个平衡
4）基于访问模式：有些应用有着明确的数据访问特点，进而产生与其相适应的缓存策略。


# 代码
## 使用Java 自带LinkedHashMap
[力扣：源于 LinkedHashMap源码](https://leetcode-cn.com/problems/lru-cache/solution/yuan-yu-linkedhashmapyuan-ma-by-jeromememory/)

```java
class LRUCache extends LinkedHashMap<Integer, Integer> {
    private int capacity;

    public LRUCache(int capacity) {
        super(capacity, 0.75F, true);
        this.capacity = capacity;
    }
    public int get(int key) {
        return super.getOrDefault(key, -1);
    }
    // 这个可不写
    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity;
    }
}
```
为什么要重写removeEldestEntry函数呢？
参看源码的注释：
![在这里插入图片描述](https://img-blog.csdnimg.cn/b4c5e8d0d5c34c72ba100b7dbbbfcc4a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAdHJpZ2dlcjMzMw==,size_20,color_FFFFFF,t_70,g_se,x_16)
## 自己构造 LinkedHashMap
```java
class LRUCache {
    class DLinkedNode{
        int key;
        int value;
        DLinkedNode prev;
        DLinkedNode next;
        public DLinkedNode() {};//一定要有无参构造方法  否则head tail无法初始化
        public DLinkedNode(int key,int value){
            this.key = key;
            this.value = value;
        }
    }
    private HashMap<Integer,DLinkedNode> cache = new HashMap<>();// 存放缓存的信息 缓存的结构是键值对
    private DLinkedNode head;
    private DLinkedNode tail;
    private int capacity;
    private int size;

    public LRUCache(int capacity) {
        //初始化 建立 head和tail的关联
        this.capacity = capacity;
        size = 0;
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        DLinkedNode node = cache.get(key);
        if(node == null){
            return -1;
        }
        moveToHead(node);
        return node.value;
    }
    public void put(int key,int value) {
        DLinkedNode node = cache.get(key);
        if(node==null){
            DLinkedNode newhead = new DLinkedNode(key,value);
            cache.put(key,newhead);
            addToHead(newhead);
            size++;
            if(size>capacity){// 核心代码 
                DLinkedNode res = removeTail();// 删除缓存时需要同步到cache中
                cache.remove(res.key);
                size--;
            }
        }else{
            node.value = value;
            moveToHead(node);
        }
    }

    public void addToHead(DLinkedNode node){// 添加到头部
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }
    private void moveToHead(DLinkedNode node) {// 移动到头部 = 删除该节点并添加该节点到头部
        // 删除前驱后继关系  和头结点及其next建立联系
        removeNode(node);
        addToHead(node);
    }
    private void removeNode(DLinkedNode node) {//移除某一节点
        node.next.prev = node.prev;
        node.prev.next = node.next;
    }

    public DLinkedNode removeTail(){// 移除最近最少使用的缓存 也就是尾结点
        DLinkedNode res = tail.prev;
        removeNode(res);
        return res;
    }
}
```



