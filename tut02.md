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
// NOTE: For simplicity, we are gonna assume N is in the form 2^c
Queue<Obj> original, extra;

int count = 1;
while (count < N) {
    // QNS: how many rounds do we need?
    for (int r=0; r<N/(2*count); r++) {
        for (int i=0; i<count; i++) {
            extra.push(original.pop());
        }

        // NOTE: !!! LOOK HERE MERGE SORT !!!
        for (int i=0; i<count*2; i++) {
            if (extra.head() < original.head()) {
                original.push(extra.pop());
            } else {
                original.push(original.pop());
            }
        }
    }

    count *= 2;
} 
```

So roughly to explain the code: We do merge sort, but bottom up. Trace the program, and you will find out how it works.

On the first round, you push one other element to the extra queue. Now you compare the two head values, and put the smaller one in first. **TADA** now you have a small portion (2 items) of the queue sorted. (right at the end of the original queue)

When everything is done for the first round, we do the same thing, but this time `count==2`. Ask yourself? what does this mean?

Everytime now, we are pushing 2 values into the extra queue, and from the previous step, we are sure that they are in the right order. So after we compare them and push them back into the original queue the mergesort way, we can be sure of one thing. The elements at the end of the original queue will be sorted (4 items by 4 items).

And here, I am sure you can see how the keyword mergesort comes in. If you don't, it just means you are not trying hard enough. TRY HARDER.

But yeah, this solution is pretty big brain. And just like what I have mentioned in class, think about what is required for you to ultilize such a sorting method (mergesort).

## spies qns (just the proof)
Okay, so in class, I have mentioned how this question can be simplified into another parallel question. In class, I see a few nodding heads, but I am sure you are just doing it for show :(

So here is how it is done, the analysis.

So we know that `log(1024c17)/log(2) = 122` approximately. What does this mean? In class, we covered the intuition, basically what this means is that you can generate `2^122` possible combinations of spies.

And how do we decide a mission? We can clearly do so in O(1) time. Remember? union of all the members in each suspicious set?

So what does that mean, it means that now we have `2^122` and we want to find the right one, how do we do that in the least steps?

First, we send the first half to the mission(`2^121` students), this will tell us if the spy set is in the first or second half. Then you send the suspected `2^120` sets. So on and so forth. Do you see how this is binary search? BIG BRAIN

**KEY TAKEAWAY** we might be evil and set hard question, but we are reasonable people, there is always so way of simplifying the problems we give you into things that we have taught you, or "casually mention" in class.

## end
