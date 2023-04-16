---
title: 求最大公约数 C++
abbrlink: d4be24bd
date: 2019-03-20 15:17:08
tags:
categories:
---

<!--more-->

<p>三种方法：</p>

<p>求此数有三种思想，一是定义法求解，二是相减求等法，三是辗转相除法。具体算法如下。</p>

<p>定义法</p>

<pre class="has">
<code class="language-cpp">#include&lt;bits/stdc++.h&gt;
using namespace std;
int main()
{
	int n,m;
	cin&gt;&gt;n&gt;&gt;m;
	int x = n&lt;m?n:m;
	int i;
	for(i=x;i&gt;=1;i--)
	{
		if(n%i==0&amp;&amp;m%i==0)break;
	}
	cout&lt;&lt;i&lt;&lt;endl;
	return 0;
}</code></pre>

<p>相减求等法:   更相减损术</p>

<pre class="has">
<code class="language-cpp">#include&lt;bits/stdc++.h&gt;
using namespace std;
int main()
{
	int n,m;
	cin&gt;&gt;n&gt;&gt;m;
	while(n!=m)
	{
		if(n&gt;m)n=n-m;
		else m=m-n;
	}
	cout&lt;&lt;m&lt;&lt;endl;
	return 0;
}</code></pre>

<p>辗转相除法:</p>

<pre class="has">
<code class="language-cpp">#include&lt;bits/stdc++.h&gt;
using namespace std;
int main()
{
	int n,m;
	cin&gt;&gt;n&gt;&gt;m;
	int a;
	while((a=n%m)!=0)
	{
		n=m;
		m=a;
	}
	cout&lt;&lt;m&lt;&lt;endl;
	return 0;
}</code></pre>

<p> </p>
