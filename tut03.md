# tut3
## a-b tree
## Q6. the pre-post order tree thingy
The big brain part about this question is the fact that it uses the order traversal to figure out whether a node is an ancestor of another. This is quite cool to think about.

In this case, we dun really need to think about all the insertion and deletion step, because it is all done for you, given a node, the instruction will tell you to either `setLeft` or `setRight`.

So how do we efficiently tell whether something is a ancestor? Does the ordered traversal give us a clue?

Think about it. In a pre-order traversal, what does something being in front means? It means that it must be "reached" first before the other node. In this case, If something is before

```
        A
        +
        |
    B<--+-->D
    +
E<--+
```

