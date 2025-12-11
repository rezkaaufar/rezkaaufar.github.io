---
layout: post
title: A year on foundational programming (doing leetcode)
description: One year of leetcode submission. Check out my leetcode page
date: '2021-04-08'
tags: []
---


![Image](/assets/img/leetcode.png)
<em>One year of leetcode submission. Check out <a href="https://leetcode.com/rezka/" target="_blank" rel="noopener noreferrer">my leetcode page</a></em>



<hr>

I had never really understood why competitive programming type of problem (I will call it CP for the rest of the post) is important. I first encountered these questions during my high school, when I first started learning how to code and was preparing for national programming competition. During my bachelor study, I re-encountered CP in the lectures. Despite my reoccurring exposure to CP, I never paid attention to it. I always thought that it was a waste of time because the real-world programming doesn’t require you to create recursive program with memoization or even worse, reversing a LinkedList. It is true though that a typical software engineer wouldn’t spend their days solving these kinds of problem, as all the underlying necessary algorithms have been abstracted away by libraries. But unfortunately, these type of questions are everywhere on the initial screening for tech recruitment.



I got into CP self-training not long after I decided to quit my <a href="https://rezkaaufar.github.io/blog/2021/leaving_phd/" target="blank">quit my PhD</a>. I had huge difficulties transitioning from academia to industry back then (I will write another blog post related to my struggle in this matter). In short, I realized that I was so lacking in foundational programming that I decided to spend time to catch up and grind on these topics. To be honest, I’m glad that I found the fact that my understanding was shaky the hard way, since it became a huge fuel for me to keep going for a year.



Despite the pros and cons about whether CP is a good way to assess candidate’s skills in a tech interview, my views on CP has changed after spending a year regularly solving problems on leetcode. I now see CP and foundational programming as beneficial to some extent. CP has broadened my perspective on the importance of time and space complexity. It becomes more crucial if you’re working with large data. In my case, I started to notice the importance especially when trying to make machine learning model works at scale. The difference on how you code your way to the solution can mean a huge difference in running time and how much money is spent on the compute power. A lot of people probably jumped on looking for the best library and tool to use to alleviate and optimize their problems. But knowing the fact that these libraries and tools are mostly built upon foundational programming certainly help to force us to think in first principle. How code differences can lead to gap in running time essentially boils down to how the operations are being processed underneath.



Some of my AHA moment on how CP helped me to think in first principle on solutions and techniques (happened on a job or during self-study):


<ul>
<li>When I was in my PhD program, I worked on creating an A* parser. We used a priority queue to store and select the best scoring atomic semantic component to be merged to form a parse tree. I didn’t understood why we used a priority queue at that time. The aha moment was when I re-studied heap data structure, in which priority queue was built on. It allows O(1) extraction time and O(log n) self-restructuring during insert and delete.</li>
<li>When I studied the <a href="https://arxiv.org/pdf/2009.06732.pdf" target="_blank" rel="noopener noreferrer">efficient transformers model</a>, I stumbled upon <a href="https://ai.googleblog.com/2020/10/rethinking-attention-with-performers.html" target="_blank" rel="noopener noreferrer">performers model</a>. Performers approximate the quadratic calculation by using kernel methods to decompose the attention matrix. What’s cool is that for the unidirectional (causal) transformers where one can’t attend to future tokens, it uses prefix-sum mechanism to compute the attention on the fly. (tbh I still have shaky understanding in performers).</li>
<li>I used approximate nearest neighbor in a recommendation system. I now know that approximate nearest neighbor is essentially a <a href="https://www.slideshare.net/erikbern/approximate-nearest-neighbor-methods-and-vector-models-nyc-ml-meetup" target="_blank" rel="noopener noreferrer">binary search</a> under the hood.</li>
<li>I now know that indexing a DB leads to a faster join time because it essentially sorts the index column so that when one join the complexity becomes O(n log m) instead of O(nm) (since it uses binary search). It can even be faster if the DB has hash index O(1).</li>
<li>Working on feature engineering for machine learning model, I used bisect python library to derive a feature that is conditioned on another columns (my problem is similar to <a href="https://stackoverflow.com/questions/45092267/spark-window-function-referencing-different-columns-for-range" target="_blank" rel="noopener noreferrer">this</a>). The solution that I can use with the built-in function (both in pandas and pyspark) led me to memory problems so I have to precompute it. Hence binary search came into rescue and it saved me tons of time.</li>
<li>I coded a recursive function to traverse a taxonomy upward. I did this on my job!</li>
</ul>

To end this post, I am grateful that I went down this leetcode rabbit hole. Knowing foundational programming has been transformative for me. It makes me a better coder (self-proclaimed). It has also became a habit of mine to at least work on a problem once a week. I intend to keep this up, so fingers crossed for me!



<hr>

<blockquote>
We do not grow absolutely, chronologically. We grow sometimes in one dimension, and not in another, unevenly. We grow partially. We are relative. 
—Anais Nin
</blockquote>
