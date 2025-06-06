---
title: "APIO-Bridges15"
date: 2025-05-02
layout: default
---

{% include mathjax.html %}


link: https://codebreaker.xyz/problem/bridge_apio15

Abridged statement: lazy, and the statement is quite clear 🤡

$K=1$ case:  

Let the position of the bridge be $x$.  
If citizen $i$'s office and house are in the same zone, the distance that citizen has to travel is obviously $|S_i-T_i|$.  
Now let us consider the case where citizen $j$'s office and house are in different zones.  
Citizen $j$ would have to travel from their house at $S_j$ to the bridge at $x$, then travel from the bridge at $x$ to their office at $T_j$.  
Hence the distance traveled by citizen $i$ will be $|S_j-x|+|T_j-x|+1$ as the width of the river is equal to $1$ too.
As such the total distance traveled by all the citizens will be:  

$$\sum (|S_i-T_i|) + \sum (|S_j-x| + |T_j-x| + 1)$$ 

for each citizen $i$ (whose office and house are in the same zone) and $j$ (whose office and house are in different zones).

Now we just have to figure out how to choose $x$.  
Claim $1$: The optimal $x$ to minimise $\sum (|S_j-x| + |T_j-x| + 1)$ is the median in the set of all $S_j$ and $T_j$.  

Proof $1$: 
We first reduce the problem to:  
Given a sorted array $A$ of $n$ numbers, find a value $x$ such that $\sum |A[i] - x|$ will be minimised.  
Note: we just ignore the $+1$ here and just consider the case for $\sum (|S_j-x| + |T_j-x|)$ as the $1$ could just be added back to final answer regardless of $x$.  
Assume $y$ is the optimal value for $x$ and $y \neq A[\frac{n}{2}] $  
This implies that there will be $>\lfloor\frac{n}{2}\rfloor$ values of $A[i]$ that are either greater than $y$ or smaller than $y$.  
If there are $>\lfloor\frac{n}{2}\rfloor$ that are greater than $y$, incrementing $y$ by $1$ will lead to $\sum |A[i] - x|$ to either decrease, or remain unchanged as the sum will decrease by at most $\lfloor\frac{n}{2}\rfloor$ and increase by at least $\lceil\frac{n}{2}\rceil$.  
Similarly, if there are $>\lfloor\frac{n}{2}\rfloor$ that are smaller than $y$, decrementing $y$ by $1$ will lead to $\sum |A[i] - x|$ decreasing via the same logic.  
We can keep repeating this process until $y$ reaches the median value of $A[\frac{n}{2}]$, which is when there will no longer be $>\lfloor\frac{n}{2}\rfloor$ values of $A[i]$ that are either greater than $y$ or smaller than $y$.  
Thus we can conclude that picking $x$ to be the median value in the array is optimal.  
(This idea is just marginalist principle in economics)

Thus we will just sort the array consisting of all the value of $S_j$ and $T_j$, and set $x$ to the median value in that array.  We can then print 
$\sum (|S_i-T_i|) + \sum (|S_j-x| + |T_j-x| + 1|)$ for the final answer.  



$K=2$ case:  

Claim $2$: It is optimal that a bridge is built on citizen $j$'s house or office for some $j$  
Proof $2$:  
The idea is the same as our median argument. Placing a bridge from a building that is not a house or an office to the nearest building that is one will lead to the sum of distances traveled by all citizens $j$ to remain the same or decrease. 

Now we want to consider given 2 bridges located at positions $L$ and $R$, which bridge would citizen $j$ choose to cross in order to minimise their travel time.  
Doing some algebra, we get citizen $j$ would choose to travel via the left bridge $L$ iff $|S_j-L|+|T_j-L|<|S_j-R|+|T_j-R|$ and travel via the right bridge $R$ otherwise.  
This looks very similar to the problem we mentioned at the start of proof $2$, but we have to choose between $L$ and $R$ for values of $x$ instead.  
The optimal value $x$ to minimise $|S_j-x|+|T_j-x|$ would be the median of the 2 values $\frac{S_i+T_i}{2}$, and the closer a value $y$ is to $x$ (the smaller the value of $|x-y|$), the smaller the value of $|S_j-y|+|T_j-y|$. This can be proved with the same argument in proof $2$. 

As such we can make a crucial observation:  
Citizen $j$ will choose to use whichever bridge that is closest to $\frac{S_j+T_j}{2}$  
Thus if we sort the citizens by $S_j+T_j$, we realise there will be a prefix of people who will choose bridge $L$ and a suffix who will choose bridge $R$.  
Let $A$ denote the sorted array of citizens who need to cross a bridge, where $A_j = {(S_j,T_j)}$, and size of $A = n$ ($N$ shall denote the total number of citizens instead)
We can iterate through all the possible places $z$ to split the people into a prefix and suffix.  
Citizens in $A[1,...,z]$ shall go through bridge $L$  
Citizens in $A[z+1,...,n]$ shall go through bridge $R$


We then recall our solution for the $K=1$ case and realise that it is optimal to build the 2 bridges in the median positions in the prefix and suffix sections respectively. 
We first consider the case for all possible prefix sections.  
Let the ordered set $P_z$ denote all of the positions of buildings in $A[1,...,z]$.   
$P_z$ will thus be of the size $2z$.  
We know the cost of all citizens in the prefix traveling through $L$ is minimised when the bridge is built at the median position in $P_z, P_z[z]$.  
Since $A_j$ is a pair of values ${(S_j,T_j)}$, the sum of distances in the prefix be:  
$\sum_{j=1}^{z} (|S_j-P_z[z]| + |T_j-P_z[z]|)$ = $\sum_{j=1}^{2z}(|P_z[j]-P_z[z]|)$
Now we just have to manipulate some algebra for magic to happen. 

 $$\sum_{j=1}^{2z}(|P_z[j]-P_z[z]|) = \sum_{j=1}^{z}(|P_z[j]-P_z[z]|) + \sum_{j=z+1}^{2z}(|P_z[j]-P_z[z]|)$$

 
 Since $P_z$ is sorted, 
 $P_z[j] < P_z[z]$ for all $j < z$
 and $P_z[j] > P_z[z]$, we can eliminate the $|\,|$ signs:  

 $$\sum_{j=1}^{z}(|P_z[j]-P_z[z]|) + \sum_{j=z+1}^{2z}(|P_z[j]-P_z[z]|) = \sum_{j=1}^{z}(P_z[z]|-P_z[j]) + \sum_{j=z+1}^{2z}(P_z[j]-P_z[z])$$  
 Notice that $P_z[z]$ is both subtracted and added $z$ times in the resulting sum. Hence we can simplify the expression even more:  

 $$\sum_{j=1}^{z}(P_z[z]|-P_z[j]) + \sum_{j=z+1}^{2z}(P_z[j]-P_z[z]) = \sum_{j=z+1}^{2z}(P_z[j]) - \sum_{j=1}^{z}(P_z[j])$$  

 Looking at this expression, we can make one of our last few observations required for the question.  
 We realise $\sum_{j=1}^{z}(P_z[j])$ is the sum of all elements $\leq P_z[j]$ and $\sum_{j=z+1}^{2z}(P_z[j])$ is sum of all elements $> P_z[z]$.  
 We then realise we can maintain the 2 sums with a median heap data structure involving 2 multisets/priority_queues.  
 This allows us to maintain the sums and have an $O(\log n)$ transition as we iterate through $z$ in $A$.  
 We will thus be able to compute all $\sum_{j=z+1}^{2z}(P_z[j]) - \sum_{j=1}^{z}(P_z[j])$ in $O(n \log n)$ time.  
 For each $z$ we shall store $\sum_{j=z+1}^{2z}(P_z[j]) - \sum_{j=1}^{z}(P_z[j])$ in a prefix array $pre[]$ at index $pre[z]$.  

 We shall then repeat the same idea but in reverse order (from $n$ to $1$) to obtain all the suffix values and store the sum of distances for citizens traveling through bridge $R$ in the suffix array $suff[]$. 

The minimum sum of distances of all citizens that need to cross bridges would thus be $\min(pre[j]+suff[j+1])$ for some $j$ in $[1,n]$. (We should have already accounted for the distance of $n$ due to people traveling across the bridge).  

Thus we can have the formula for our final answer :

$$\sum (|S_i-T_i|) + n + \min(pre[j]+suff[j+1])$$

for all $i,j$ where:  
$i$ are citizens whose office and house are in the same zone and hence do not need to cross the bridge  
$j$ are citizens whose office and house are in different zones and need to cross the bridge  
$n$ is the number of citizens who have to cross the bridge to reach their house/office  

The final time complexity will be $O(N \log N)$ where $N$ is the total number of citizens. 

[Code](https://oj.uz/submission/1194321)


----------
Omg I'm finally done with this question. I spent 2 days learning how to set up latex, solving and typing this question. I hope it's worth it, the problem is quite cool. I am too lazy to format my previous blog, but ig it will be funny to look back at my progress 🤡. Idk if this will ever be useful, I hope it will but it probably helped me get better at explaining and proving ideas. Kinda funny that the key ideas of this problem can be proved with just marginalist principle 🤡.
