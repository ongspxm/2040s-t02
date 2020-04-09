# tutorial 10
## qns 2 - runtime of algorithm

Parts of the algorithm, assuming F(V,E)

1) Median edge of E
2) partion into 2 sets of E/2 each
3) Perform the same thing on F(V,E/2) on both sets of edges
4) Combine the edges from the two trees

Okay so some basic values first (all went through in class)
- the first two steps will just be `O(E)`, the runtime of quickselect
- Each of the trees in 3 will have the maximum number of edges `V-1`, therefore, step for will be F(V, 2V)
- This is will be run one time using the MST algo, this will give us runtime of O(VlogV)

Here is the complete analysis on the count of the number of function calls in total. On the first layer, there will be one function of `F(V,E)`, on the second layer, there will be 2 of `F(V, E/2)`.

So lets ask ourselves this question, how many nodes are there on each layer? On layer `l`, you will have `2^l` nodes, this seems simple, but how many layers will we have in total? At which level will i have the number of edges be `4V`?

```
E/(2^l) < 4V
E/4V < 2^l
log(E/4V) < l
log(E/V)-log(4) < l
```
So the number of layers will be `log(E/V)`. Okay the other thing that we need, the total nubmer of nodes (number of function calls), how do we do that? Up to layer `l`, how many nodes will there in total? If we have `log(E/V)` layers, how many nodes will that be?

```
l layers => 2^(l+1)
log(E/V) layers => 2E/V
```

so that will gives us a total of `2E/V` nodes. And all these information is all that is required to get to calculating the runtime.

So from the description of the algorithm, we can calculate the runtime using the following equations.

```
T(V,E) = O(E) + O(VlogV) + 2T(V,E/2)
T(V,E) = O(E) + O(VlogV) + 2(O(E/2) + O(VlogV) + 2T(V,E/4))

S1 = sum all of the O(E) together
S1 = O(E) + 2O(E/2) + 4O(E/4) + ...
S1 = O(E) + O(E) + O(E) + ... (total of log(E/V) layers)
S1 = O(Elog(E/V)) = O(ElogE) = O(ElogV) (since E<V^2)

S2 = sum of all the O(VlogV) (how many do we need)
S2 = (1+2+4+...)O(VlogV) total of log(E/V) layers
S2 = (2E/V)O(VlogV) = 2O(ElogV)

S1+S2 = O(ElogV) + 2O(ElogV) = O(ElogV)
```

okay... so that's all of it... you should be able to get it, if you just work thorugh it step by step... all the math we have gone through before.
