# tutorial 7 week 9
Okay this week's tutorial is a bit rush. So i couldnt go through everything fully. 

## 1 heapify in O(n)
This week's tutorial focused a lot on graph, and not much on heap. But **heaps are pretty interesting data structures**.

heapify can be **done in linear runtime** is a interesting proof and it follows as so. Take a binary heap for example, i.e one parent, two child. How many edge will that heap have? Okay lets keep that in mind first. **The number of edges matter**.

Lets say we start fixing the constraints from the most bottom later first, i.e. **from the leaf upwards**. To fix all the leafs with its corresponding parent, all of the edges represent all the connections that requires checking.

So we just need to **check each parent child relation only once**, and that is exactly the number of edges, so how many edges are there in the whole heap?

**Drawing it out and counting**, we get the number of edges to be `2 + 2^2 + 2^3 + ... + 2^(log n)`, and well, using some math, gp and all, we can get `n`. But that is the math method, is there a intuitive method to visualize this?

U can **imagine each edge as pointing to the parent**. Each node can only have one parent. And there are total of n node, therefore, **max n edges total**.

## 7c the graph job problem
Okay so there is a **few typos in this solution**.

### edge added if NOT overlap
"Now you need to find collections of nodes that are not neighbors."

One of you raised questions about this statement, so let me try explaining this fully. I kinda got confused with this statement too, I think I didnt quite explain it right in class, so... 

Lets start by **explaining the kind of intuition** first. If we draw an edge between nodes that means that it **doesn't conflict and can be considered to be part of the same collection**.

Now we want to get **a subset of nodes such that all the nodes form a complete graph**, i.e. each node has a edge with every other node, which means that that job is non overlapping with every other job in the collection.

So it becomes a problem of picking nodes. For one or two node, its pretty trivial. How then do we pick the third node? Any third node we pick must have edges to the first two nodes. Then the fourth nodes must have edges to the first 3 nodes, and so on. **Every time we add a node, we want it to form a complete graph**.

Intuitive, does this makes sense? For every new job added in, we want it to "play nice" with the other jobs in the subset. So how would we change the phrase in the answer?

"Now you need to find collections of nodes that are complete neighbors." This is what i would change it to.

But do note, the **idea of independent set doesn't apply here**. Independent set is when you want the nodes to have **NO edges with any other nodes in the set**, that applies to the other case.

### edges if job overlaps
One of you raised the idea of adding edge if job overlaps. I just said its a hard problem, but didn't really elaborate, but **here is where the independent set idea is used**.

Let's say we want to just find a possible combination, nothing about maximal, just a possible set.

What do we do first? We pick a random node, then **we "blacklist" all these neighbors**, meaning removing nodes that has edges connected to it, i.e. removing jobs that overlaps. Then **we pick another node that is not blacklisted**, then we continue the process, all the way until, there is no more "non blacklisted" to pick.

What is guaranteed with this picking? The **blacklisting process ensures that no neighbors will get picked** and therefore, any jobs that overlaps will not be picked, and hence will give a valid solution.

But how do we find the maximal? Well, **you have to try every single thing**, if there are `n` nodes that gives us `2^n` possible states, and **any optimization will will not be trivial**, therefore getting maximal independent set is np-hard, non polynomial.

Okay if this was the solution that was intended, then the typo will be "assuming edges added if jobs overlap".
