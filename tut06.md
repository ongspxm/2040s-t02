# tutorial 6 
Okay just some parts that I left as homework exercises, **do it yourself first**, its not too hard. After that then try to read through see if whatever I said makes sense, or maybe there is some error, just check that you understand what it is all about.

## part 1c, the minhash one
here, we want to prove that the hash function fulfills the jaccard property. In class, we went through the steps of analyzing `h(A)` and `h(B)` separately. It might seem a bit messy, but here is the writeup, read this, and try to figure it out, just stare and think about it, it will kinda help.

To figure out the probability, we see how `h(A^B)` compares with the hash function's result on the set separately. Namely, `P(h(A)=h(B))` can be rewritten as `P(h(A)=h(A^B) and h(B)=h(A^B))`

This analysis can be applied to a generic form of hash function where hashes are applied to sets. But there is a property of the minhash function that allows us to extend the analysis further.

**specific ordering on all values** Let U be the set of all the values in A and B. The property `h(A)=h(A^B) and h(B)=h(A^B)` holds when `h(U) =   h(A^B)`.

Why is this so? Intuitively, when `x=h(U)` is not in `A^B`, but `x` in `A`,  that means that `h(A)=h(U)` and `h(B)!=h(U)`. This is the same case for when `x` is in `B` as well. Therefore, we can analyse the hash function as following.

Considering `|U| = |A v B|`, there are `|U|` ways of picking the min of the set, and of these ways, there are exactly `|A ^ B|` ways to pick the min such that `h(U)=h(A^B)`. Therefore, the final probability becomes `P(h(A) = h(B)) = P(h(AvB) = h(A^B)) = |A^B| / |A v B|`, which is the jagard measure.

## problem 2, cuckoo hash
Once you all have figured out how the cuckoo hash thing works, here is how it is applied to the question. Basically you want to use the array itself as a hashtable. Repeatedly kicking out values until you have eventually placed all the values in the right place.

And what are we guaranteed here? If there is a missing item, we are guaranteed that there will be an `i` where `arr[i] != i` which will therefore be the missing value.
