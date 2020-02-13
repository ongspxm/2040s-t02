# tut3
## Q6. the pre-post order tree thingy
The big brain part about this question is the fact that it uses the order traversal to figure out whether a node is an ancestor of another. This is quite cool to think about.

In this case, we dun really need to think about all the insertion and deletion step, because it is all done for you, given a node, the instruction will tell you to either `setLeft` or `setRight`.

So how do we efficiently tell whether something is a ancestor? Does the ordered traversal give us a clue?

```
        A
        +
        |
    B<--+-->D
    +
E<--+
```

### intuition
Think about it. In a pre-order traversal, what does something being in front means? It means that it must be "reached" first before the other node. But think about it, is this necessary the case?

```
[node] [traverse(node.left)] [traverse(node.right)]
```

if a certain node is before another, but it is not the ancestor, there could only be one other case - one of the node is in `node.left` and the other is the `node.right`.

How then do we differentiate it? Think about how the post-order traversal works. 

```
[traverse(node.left)] [traverse(node.right)] [node]
```

In this case, if a node is printed after another, it should be the ancestor. All but one case - if one is in `node.left` and the other is in `node.right`.

Combining both

1) if `nodeA` is in front of `nodeB` in preorder traversal
2) if `nodeA` is after `nodeB` in postorder traversal

If all these conditions satisfy, we can be sure that `nodeA` is an ancestor of `nodeB`. So how do we combine these two order traversal together?

### the traversal scheme
If you refer to the colorful picture in your answer, you will have a scheme that is something like that:

```
S(n) traverse(n.left) traverse(n.right) E(n)
```

If you draw it all out, you will see that all the `S(x)` form the preorder traversal, and all the `E(x)` form the postorder traversal.

### updating it
So what happens when you do a `n.insertLeft(x)`? How does the traversal order change?

Take a look at the prepend on first, we know for sure that if you do a `insertLeft` the new item directly follows the original one. (Note: go try it out)

This gives us a hint, we can just insert `S(x)` directly after `S(n)`. What about the post order? How do we insert the `E()`s?

We know that `E(x)` must be before `E(n)`, that is all we care when we sre checking ancentral links. So in this case, we can simply put it anywhere before `E(n)`. Can we just put it right next to each other? Like `S(n) S(x) E(x)`? (well, think about it, can we?)

The same analysis goes for `insertRight`, we can be sure of its position in the post order, (right before thr original node). Do the same analysis and you will know how to come up with the specifications of it.

And thats all, now try to work out the runtime complexity for this method, and how to improve it.

## a-b tree
This is technically in the recitation, and not really tutorial material, but since you guys asked, i will deliver.

**caution** this is not meant to replace the recitation sheet, but to help you understand it.

I dun really like going through mechanical stuff, those follow steps do things stuff, because they are generally not very cool. So here, i am going to explain some of the intuition behind it to make easier to see why it is being done that way.

### explaination

Each node has these things:

- `val`s - each node can store more than one value, sorted, up to `b`
- `children` - if we have `n` values, we will have `n+1` children to cover all the partitions (how is this created? Wait, read on)

So to explain the intuition, its easiest to use an insertion example. Lets say `b=4`, i.e. maximum of 4 values per node.

Insertion of 3 items is easy, you just keep filling the root node. Say, we insert, `4, 5, 6, 7`. 

Now what happens when we insert 8? Too much, what happens then?

`4,5,6,7,8` - we pick the median to be the splitting value. `6` is picked. What do we do to it then? We shift it to the node above (new one created) and then create the child.

```
    6
4,5   7,8
```

### intuition
Okay cool? What is the intuition here? 

Every time we have too many values, the value will be pushed upwards into the node above? Think about it, this is actually quite different from our usual tree.

In a usual tree, our tree extends downwards, but for this, our values are being pushed upwards, how is this better? (Think)

### explaination, extended
Assume i have this tree at first.

```
    4 
2,3   5,6,7,8
```

Say I want to insert `9`, where will that go? Clearly it will go into the very last node.

```
    4 
2,3   5,6,7,8,9
```

Too much, now it will overflow and push it up 

```
   (4     7)
2,3   5,6   8,9
```

See, 2 values, 3 partitions. Okay, now proof that the value will always be one less than the number of partitions if we construct our tree this way. Big brain time :)
