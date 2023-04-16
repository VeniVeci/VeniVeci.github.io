---
title: Java算法代码模板整理 通用篇
tags: java 算法 堆排序 比较器
categories: Java基础知识 四大件之数据结构和算法
abbrlink: fe5b3a1b
date: 2022-02-15 09:57:15
---

<!--more-->

<p id="main-toc"><strong>目录</strong></p>

<p id="-toc" style="margin-left:0px;"></p>

<p id="%E5%B8%B8%E8%A7%84%E4%BB%A3%E7%A0%81%E6%A8%A1%E6%9D%BF-toc" style="margin-left:0px;"><a href="#%E5%B8%B8%E8%A7%84%E4%BB%A3%E7%A0%81%E6%A8%A1%E6%9D%BF">常规代码模板</a></p>

<p id="%E6%A0%B8%E5%BF%83%E4%BB%A3%E7%A0%81%E6%A8%A1%E5%BC%8F-toc" style="margin-left:40px;"><a href="#%E6%A0%B8%E5%BF%83%E4%BB%A3%E7%A0%81%E6%A8%A1%E5%BC%8F">核心代码模式</a></p>

<p id="ACM%E6%A8%A1%E5%BC%8F-toc" style="margin-left:40px;"><a href="#ACM%E6%A8%A1%E5%BC%8F">ACM模式</a></p>

<p id="map%20set%20%E9%81%8D%E5%8E%86-toc" style="margin-left:40px;"><a href="#map%20set%20%E9%81%8D%E5%8E%86">map set 遍历</a></p>

<p id="%E8%87%AA%E5%AE%9A%E4%B9%89%E6%AF%94%E8%BE%83%E5%99%A8-toc" style="margin-left:40px;"><a href="#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%AF%94%E8%BE%83%E5%99%A8">自定义比较器</a></p>

<p id="%E5%BB%BA%E7%AB%8B%E5%A4%A7%E8%B7%9F%E5%A0%86%E3%80%81%E5%B0%8F%E6%A0%B9%E5%A0%86-toc" style="margin-left:40px;"><a href="#%E5%BB%BA%E7%AB%8B%E5%A4%A7%E8%B7%9F%E5%A0%86%E3%80%81%E5%B0%8F%E6%A0%B9%E5%A0%86">建立大跟堆、小根堆</a></p>

<p id="-toc" style="margin-left:0px;"></p>

<hr id="hr-toc" /><h1 id="%E5%B8%B8%E8%A7%84%E4%BB%A3%E7%A0%81%E6%A8%A1%E6%9D%BF">常规代码模板</h1>

<h2 id="%E6%A0%B8%E5%BF%83%E4%BB%A3%E7%A0%81%E6%A8%A1%E5%BC%8F">核心代码模式</h2>

<p>函数名，输入参数，输出参数类型都已经写好，只需要写函数体即可。</p>

<p><img alt="" height="414" src="https://img-blog.csdnimg.cn/1a621b6ca2e143f6baa841949803822d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_18,color_FFFFFF,t_70,g_se,x_16" width="649" /></p>

<h2 id="ACM%E6%A8%A1%E5%BC%8F">ACM模式</h2>

<p>啥都没有，需要自己写</p>

<p><img alt="" height="297" src="https://img-blog.csdnimg.cn/7ce53eeaa08848aeb0d6f658446e5211.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAU2Vla19fdHJ1dGg=,size_17,color_FFFFFF,t_70,g_se,x_16" width="619" /></p>

<p> 这里提供一个模板，主要是读入示例，自己构建出输入参数，那么就可以转化为核心代码模式了，方便过渡。</p>

<p>读入大致是分为两类，数字和字符串。</p>

<pre>
<code class="language-java">import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc=new Scanner(System.in);// 获取控制台对象
        int T = sc.nextInt();//如果一个示例有多组数据 T就是组数
        while(T-- &gt;0){// 循环读入T组数据，处理后输出每组数据的结果
            int n = sc.nextInt();// 该示例中 需要先读取一个 n 表示接下来n个数是x数组
            int[] nums = new int[n];
            String[] strs = new String[n];
            for(int i = 0; i &lt; n; ++i){
                nums[i] = sc.nextInt();
            }
            for(int i = 0; i &lt; n; ++i){
                strs[i] = sc.nextLine();// nextLine()读取字符串
            }
            System.out.println(getResult(nums,strs));
        }
    }

    private static int getResult(int[] nums, String[] strs) {
        //核心代码模式
        // write your code here
        return -1;
    }

}
</code></pre>

<h2 id="map%20set%20%E9%81%8D%E5%8E%86">Map Set 遍历</h2>

<pre>
<code class="language-java">/*
* 测试List遍历的方式
* */
public class TestArrayList {
    public static void main(String[] args) {
        List&lt;String&gt; list = new ArrayList&lt;&gt;();
        list.add("aa");
        list.add("bb");
        list.add("cc");

        //方式一，for循环遍历,通过get()方法
        for (int i = 0; i &lt; list.size(); i++) {
            System.out.println(list.get(i));
        }


        //方式四,增强for循环  foreach
        for (String str : list) {
            System.out.println(str);
        }

        //方式五，转化数组
      　　　  String[] a = new String[3];
        String[] aa = list.toArray(a);
        for (int i = 0; i &lt; aa.length; i++) {
            System.out.println(aa[i]);
        }
    }
}
————————————————
版权声明：本文为CSDN博主「yuyinghe0612」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/yuyinghe0612/article/details/80549443</code></pre>

<pre>
<code class="language-java">public class TestHashMap {
    public static void main(String[] args) {

        Map&lt;String, String&gt; map = new HashMap&lt;&gt;();
        map.put("a", "2000");
        map.put("b", "3000");

        //方式一、在for-each循环中使用entrySet来遍历
        Set&lt;Map.Entry&lt;String, String&gt;&gt; entrySet = map.entrySet();
        for (Map.Entry&lt;String, String&gt; entry : entrySet) {
            String key = entry.getKey();
            String value = entry.getValue();
            System.out.println("key=" +key+"  value="+value);
        }

        //方式二、在for-each循环中通过键找值遍历
        for (String key : map.keySet()) {
            System.out.println("key=" + key+"  value="+map.get(key));
        }

        //只打印键值keys
        for (String key : map.keySet()) {
            System.out.println("key=" + key);
        }
        //只打印values
        for (String value : map.values()) {
            System.out.println("value=" + value);
        }


    }
}
————————————————
版权声明：本文为CSDN博主「yuyinghe0612」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/yuyinghe0612/article/details/80549443</code></pre>

<p>map不能用增强for循环。</p>

<p></p>

<h2 id="%E8%87%AA%E5%AE%9A%E4%B9%89%E6%AF%94%E8%BE%83%E5%99%A8">自定义比较器</h2>

<p>算法题中会出现可以使用库函数sort 的情景，不过往往不是简单的sort，在java中</p>

<p>sort默认是升序，如何实现降序呢？ 如果现在是对nums(int[2]) 中的第0列数据升序，在此基础上第1列数据降序，又怎么做呢？<br />
 </p>

<pre>
<code class="language-java">import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;

/**
 * @Description
 * @create 2022/2/14 - 21:27
 */
public class compareDemo {
    public static void main(String[] args) {
        int[][] nums = new int[3][2];
        nums[0][0] = 12;
        nums[0][1] = 0;
        nums[1][0] = 15;
        nums[1][1] = 50;
        nums[2][0] = 15;
        nums[2][1] = 90;
        for (int[] num : nums) {
            System.out.println(num[0] + " " + num[1]);
        }
        System.out.println("=========排序后==========");
        Arrays.sort(nums,new Comparator&lt;int[]&gt;(){
            @Override
            public int compare(int[] o1, int[] o2) {
                if(o1[0]!=o2[0]){
                    return o1[0] - o2[0];// 升序
                }else{
                    return o2[1] - o1[1];// 降序
                }
            }
        });
        for (int[] num : nums) {
            System.out.println(num[0] + " " + num[1]);
        }

        Integer[] nums1 = new Integer[]{5,7,8,2,3};
        Arrays.sort(nums1);// 升序
        for (int i : nums1) {
            System.out.println(i);
        }
        Arrays.sort(nums1, Collections.reverseOrder());// 降序
        for (int i : nums1) {
            System.out.println(i);
        }
        Arrays.sort(nums1,new Comparator&lt;Integer&gt;(){
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1; // 降序
            }
        });
        for (int i : nums1) {
            System.out.println(i);
        }
    }
}
</code></pre>

<p>sort 自定义比较器不能对 int[] 等基本数据类型的数组进行排序，只适用于object[]。</p>

<p>所以对于int[] 要不然转化为引用数据类型的数组，再用库函数，要不然自己写快排之类的算法。</p>

<p></p>

<h2 id="%E5%BB%BA%E7%AB%8B%E5%A4%A7%E8%B7%9F%E5%A0%86%E3%80%81%E5%B0%8F%E6%A0%B9%E5%A0%86">建立大跟堆、小根堆</h2>

<pre>
<code class="language-java">import java.util.Comparator;
import java.util.PriorityQueue;

/**
 * @Description
 * @create 2022/2/14 - 21:58
 */
public class heapDemo {
    public static void main(String[] args) {

        PriorityQueue&lt;Integer&gt; pq = new PriorityQueue&lt;&gt;(new Comparator&lt;Integer&gt;() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2;//小根堆
            }
        });
        pq.add(1); pq.add(5); pq.add(10);
        PriorityQueue&lt;Integer&gt; pq2 = new PriorityQueue&lt;&gt;(new Comparator&lt;Integer&gt;() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;//大根堆
            }
        });
        pq2.add(1); pq2.add(5); pq2.add(10);

        System.out.println(pq.peek());
        System.out.println(pq2.peek());

        int[][] nums = new int[3][2];
        nums[0][0] = 12;
        nums[0][1] = 0;
        nums[1][0] = 15;
        nums[1][1] = 50;
        nums[2][0] = 15;
        nums[2][1] = 90;
        for (int[] num : nums) {
            System.out.println(num[0] + " " + num[1]);
        }
        PriorityQueue&lt;int[]&gt; pqArr = new PriorityQueue&lt;&gt;(new Comparator&lt;int[]&gt;() {
            @Override
            public int compare(int[] o1, int[] o2) {
                if(o1[0]!=o2[0]){
                    return o1[0] - o2[0];// 升序
                }else{
                    return o2[1] - o1[1];// 降序
                }
            }
        });
        pqArr.add(nums[0]);
        pqArr.add(nums[1]);
        pqArr.add(nums[2]);
        while(!pqArr.isEmpty()) {
            System.out.println(pqArr.peek()[0] + " " + pqArr.poll()[1]);
        }
    }
}
</code></pre>

<p></p>

<h1 style="margin-left:.0001pt;text-align:justify;"><a data-link-icon="https://csdnimg.cn/release/blog_editor_html/release2.0.5/ckeditor/plugins/CsdnLink/icons/icon-default.png?t=M1FB" data-link-title="Java中List, Integer[], int[]的相互转换" href="https://www.cnblogs.com/cat520/p/10299879.html" title="Java中List, Integer[], int[]的相互转换">Java中List, Integer[], int[]的相互转换</a></h1>

<p></p>
