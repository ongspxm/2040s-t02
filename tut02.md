# tut 2
## k = log(n) question
So, first up, we made a boo boo in the answer key, it has been changed and the amended one will be updated. Below outlines the correct solution:

```
We want only one level of recursion:

1) choose k = n/log(n) pivots. Partition into n/log(n) buckets.

2) run selectionsort on each bucket separately.

(No recursive quicksort.)

Total cost:
1) O(n) to sort pivots (because n/logn pivots)
2) O(n log n) to place elements in buckets.
3) O(n /log(n))*O(log^2n) to do all the selection sorts.
Total: O(n log n)

Write cost:
O(n) to sort pivots (since n/logn pivots)
O(n) to bucket
O(n/log n)*O(log n) to do selection sorts.
Total: O(n)
```

This is quite the big brain solution here, and the purpose of choosing `k` is to make sure that we can keep without both bounds.

Selection sort in itself will guarantee `O(n)` writes. But it does a `O(n^2)` on total operations. The hard part comes in keeping the total read to `n log(n)`.

In picking the right value for `k`, we reduce the amount of reads in the selection sort stage. To magically pull this value out of thin air, you will have to ~~sell your soul to the devil~~ think very hard.

Figure out the math magic, it will be fun.

## sort using 2 queues
So i talked about the pop item out put it back in kind of idea to implement selection sort. Think about it what do we need from the queue to implement this?

I propose the following properties:
1) All items uniquely identifiable
2) All items unique, comparison wise (i.e. order exist)
3) (check that this is all you require)

Woohoo, thats all, we dun even need to know the size of the queue, how many items inside and all. I am not saying that this is the most efficient solution, but this solution requires quite little assumptions, which is cool to explore.

The steps is as follows. I always believe in the C^3 principle: **Code Can Clarify** and of course **Code Cannot Confuse**

```
Queue<Obj> existing, final;

Obj min = null;
while (!existing.isEmpty()) {
    Obj curr = existing.pop();

    if (curr == min) {
        final.push(curr); 
        min = null;
        continue;
    }
    
    if (min == null || curr < min) {
        min = curr;
    }
    existing.push(curr);
}
```

Code instead speak a thousand words. The main idea is that, we do not necessary need to know the size of the original queue. 

Since we keep pushing stuff back, if we see that we see the `min` once again, we can be sure that it is the minimum, no questions. (Rmb: uniquely identifiable)

Then the question we always ask ourselves, **can we do better**?

## sort using 2 queues (big brain method)
The previous solution is slow, like really really slow. It is quite cool, but that is not fast enough for us. So lets ask ourselves, how then? How do we sort a list using 2 queues, efficiently? One word: MERGESORT

Okay it struck me that I just covered the naive method above without going in depth into this big brain solution. No wonder I ended class so early today...

Let's look at some code first before I go explaining what I am trying to do here.

```

```

## spies qns (just the proof)
## minimum of queue
