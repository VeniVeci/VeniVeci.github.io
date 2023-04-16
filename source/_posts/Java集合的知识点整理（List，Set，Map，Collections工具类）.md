---
title: Java集合的知识点整理（List，Set，Map，Collections工具类）
tags: java 数据结构 集合 map set
categories: Java基础知识
abbrlink: d15fe24e
date: 2021-02-16 17:51:09
---

<!--more-->

<p>Java集合概念和使用大全集（一些集合类list set map的介绍、使用和源码剖析），内容来自java尚硅谷（课程链接<a href="https://www.bilibili.com/video/BV1Kb411W75N">https://www.bilibili.com/video/BV1Kb411W75N</a>）</p>

<p><strong>通过对集合的学习，我们要达到：</strong></p>

<p>1.选择合适的集合类去实现数据的保存，调用其内部的相关方法。</p>

<p>2.不同的集合类底层的数据结构为何？如何实现数据的操作的：增删改查等。</p>

<p>对应于不同的数据种类，采用不同的集合类，能够针对性的实现数据的存储和使用。</p>

<p> </p>

<p>首先说到数据的存储我们都知道有数组，集合、数组都是对多个数据进行存储操作的结构，简称Java容器。</p>

<p><strong>为什么java中会出现集合类呢，为什么不用数组呢？</strong><br />
因为数组在存储多个数据方面的缺点：<br />
&gt; 一旦初始化以后，其长度就不可修改。<br />
&gt; 数组中提供的方法非常有限，对于添加、删除、插入数据等操作，非常不便，同时效率不高。<br />
&gt; 获取数组中实际元素的个数的需求，数组没有现成的属性或方法可用<br />
&gt; 数组存储数据的特点：有序、可重复。对于无序、不可重复的需求，不能满足。</p>

<hr /><h1>list</h1>

<h2><code class="language-html"><span style="color:#f33b45;"><strong>List接口框架介绍和源码分析</strong></span><br />
    |----Collection接口：单列集合，用来存储一个一个的对象<br />
          |----List接口：存储有序的、可重复的数据。  --&gt;“动态”数组,替换原有的数组（我们通常所说的数组）<br />
              |----ArrayList：作为List接口的主要实现类；线程不安全的，效率高；底层使用Object[] elementData存储<br />
              |----LinkedList：对于频繁的插入、删除操作，使用此类效率比ArrayList高(因为底层使用双向链表存储)<br />
              |----Vector：作为List接口的古老实现类；线程安全的，效率低；底层使用Object[] elementData存储<br />
  面试题：ArrayList、LinkedList、Vector三者的异同？<br />
  同：三个类都是实现了List接口，存储数据的特点相同：存储有序的、可重复的数据<br />
  不同：见上<br />
  一般在实际应用中我们会使用ArrayList，在有线程安全的需求时我们使用Vector，需要频繁的插入、删除操作时使用LinkedList。<br /><span style="color:#f33b45;">1. ArrayList的源码分析：</span><br />
   2.1 jdk 7情况下（可忽略，现在一般都是jdk8）<br />
      ArrayList list = new ArrayList();//底层创建了长度是10的Object[]数组elementData<br />
      list.add(123);//elementData[0] = new Integer(123);<br />
      ...<br />
      list.add(11);//如果此次的添加导致底层elementData数组容量不够，则扩容。<br />
      默认情况下，扩容为原来的容量的1.5倍，同时需要将原有数组中的数据复制到新的数组中。<br /><br />
      结论：建议开发中使用带参的构造器：ArrayList list = new ArrayList(int capacity)<br />
       避免中间环节扩容降低程序的效率<br />
   2.2 jdk 8中ArrayList的变化：<br />
      ArrayList list = new ArrayList();//底层Object[] elementData初始化为{}.并没有创建长度为10的数组<br />
      list.add(123);//第一次调用add()时，底层才创建了长度10的数组，并将数据123添加到elementData[0]<br />
      ...<br />
      后续的添加和扩容操作与jdk 7 无异。<br />
      小结：jdk7中的list创建类似于单例模式的饿汉式，而jdk8中的创建类似于单例的懒汉式。<br />
      在实际开发中，我们在生成arraylist之后，转而进行别的操作，这个时候jdk8中的做法<br />
      就可以节省内存，提高效率。<br /><span style="color:#f33b45;">2. LinkedList的源码分析：</span><br />
      LinkedList list = new LinkedList(); 内部声明了Node类型的first和last属性，默认值为null<br />
      list.add(123);//将123封装到Node中，创建了Node对象。</code></h2>

<h2><span style="color:#f33b45;">List接口的常用方法</span></h2>

<h3><code class="language-html">void add(int index, Object ele):在index位置插入ele元素（ele：element的缩写）<br />
boolean addAll(int index, Collection eles):从index位置开始将eles中的所有元素添加进来<br />
Object get(int index):获取指定index位置的元素<br />
int indexOf(Object obj):返回obj在集合中首次出现的位置<br />
int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置<br />
Object remove(int index):移除指定index位置的元素，并返回此元素<br />
Object set(int index, Object ele):设置指定index位置的元素为ele<br />
List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的子集</code></h3>

<h3><code class="language-html">增：add(Object obj)<br />
删：remove(int index) / remove(Object obj)<br />
改：set(int index, Object ele)<br />
查：get(int index)<br />
插：add(int index, Object ele)<br />
长度：size()<br />
遍历：① Iterator迭代器方式<br />
     ② 增强for循环<br />
     ③ 普通的循环</code></h3>

<h2>set<br /><span style="color:#f33b45;"><code class="language-html">Set接口框架介绍和源码分析</code></span><br /><code class="language-html">* |----Set接口：存储无序的、不可重复的数据。<br />
* |----HashSet：HashSet底层：数组+链表的结构。<br />
* |----LinkedSet：LinkedHashSet作为HashSet的子类，在添加数据的同时，每个数据还维护了两个引用，记录此数据前一个数据和后一个数据。优点：对于频繁的遍历操作，LinkedHashSet效率高于HashSet<br />
* |----TreeSet：1.向TreeSet中添加的数据，要求是相同类的对象。2.两种排序方式：自然排序（实现Comparable接口） 和 定制排序（Comparator）<br /><br /><span style="color:#f33b45;">无序性</span>：当调用set.add(obj)时，会先计算obj的哈希值，然后再经过映射，得到存放的索引。<br />
所以不同的obj存放的顺序不是按照数组的索引0 1 2 3这样顺序存储的。<br /><br /><span style="color:#f33b45;">不可重复性</span>：当调用set.add(obj)时，计算obj哈希值，存放时先判断存放位置是否有元素，<br />
如果没有直接存储，如果有，则调用equals()方法判断两个obj是否相等，如果相等，就不存储，如果不等，则<br />
按照七上八下的规则存储为链表。<br />
七上八下：jdk8中，新来的元素放在旧元素后面，链表第一个值指向旧元素的地址。</code></h2>

<p><strong>Set接口的常用方法和list差不多，具体看代码。</strong></p>

<h3>map</h3>

<h3><code class="language-html">|----Map:双列数据，存储key-value对的数据 ---类似于高中的函数：y = f(x)<br />
     |----HashMap:作为Map的主要实现类；线程不安全的，效率高；存储null的key和value<br />
     |----LinkedHashMap:保证在遍历map元素时，可以按照添加的顺序实现遍历。<br />
     原因：在原有的HashMap底层结构基础上，添加了一对指针，指向前一个和后一个元素。<br />
     对于频繁的遍历操作，此类执行效率高于HashMap。<br />
     |----TreeMap:保证按照添加的key-value对进行排序，实现排序遍历。此时考虑key的自然排序或定制排序<br />
     底层使用红黑树<br />
     |----Hashtable:作为古老的实现类；线程安全的，效率低；不能存储null的key和value,健壮性比较差。<br />
     |----Properties:常用来处理配置文件。key和value都是String类型<br />
      HashMap的底层：数组+链表 （jdk7及之前）<br />
      数组+链表+红黑树 （jdk 8）</code></h3>

<p><strong>Map接口的常用方法和list差不多，具体看代码。</strong></p>

<h3>代码主要是List，Set，Map，Collections工具类的使用，软件使用IDEA，JDK8。</h3>

<p>下载链接：</p>

<p><a href="https://download.csdn.net/download/weixin_40757930/15315105">https://download.csdn.net/download/weixin_40757930/15315105</a></p>
