# tutorial 4
Okay this tutorial is slightly long, i couldnt cover everything in detail. So the write up will be abit longer.

## problem 1 - kd tree find x-min analysis
Okay this one is pretty fun, so i want you to work it out for yourselves, at least the math part. 

Here is an explanation of the hint. Here is how the code looks like, same as in class.

```
function xmin(node) {
    // QNS: terminating criteria?
    
    if (node.isSplitX()) {
        return xmin(node.left);
    } else {
        return min(xmin(node.left), xmin(node.right));
    }
}
```

okay, now stare at the code, and tell me if this makes sense.

```
T(n) = T(n/2) + O(1)
T(n) = 2T(n/4) + O(1)
```

Why? Remember skip layer? Okay now the **big brain time**. Instead of using `n`, which represent number of nodes, lets use `h` height instead.

```
T(h) = T(h-1) + O(1)
T(h) = 2T(h-2) + O(1)
```

For most of you, this calculation will be **much easier**. You will get the `1 + 2 + 4 + 8 + ...` after you expand it out. Here you should use GP, given there are `h/2` terms.

Asssuming it is a balance tree. This means that you can sub back `h=log n` to get the runtime in terms of `n`. And when you do that, you can kinda simplify it into `O(n)`, how? **hint** log both sides.

## problem 2 - trie findRange analysis
Remember the swingly wiggly diagram that looks like a fan? Okay this is the math part, so listen close.

Okay the analysis goes like these. For each node that you reach, what is the maximum number of things you need to do? You need to do at most 2 fully, then the rest you need to just O(1).

Here, we are using `h = len(word) = height of tree required to traverse`

```
T(h) = 2T(h-1) + O(1)
```

Now you do the **math expansion** then you will get the GP, then you combine them, do work out the math, and you should get something similar to the one for the trie analysis - something along the lines of `O(2^h)`

## problem 5 - table sitting one
Okay this is the last question that we covered, and we talked about the criteria for the search - **"how many people ended the room after me and havent left yet"**.

Okay, i hope you have given some thought into it, and came up with something cool. Figure out what makes your solution works, and what doesnt.

Here, I am going to propose this particular solution - you want to store all the people still in the room. Assume you have an AVL, or any sort of tree for that matter. And we want to be able to tell the rank each element. 

**Why?** Every time we process a start time, we will insert the guy into the tree, everytime we process an end time, we will remove that guy from the tree. What will this give us? **"Everybody still in the room"**

So how many people end after me? How do I find out? If my tree stores the **rank of the item**, (ie which place it is in), then finding the number of people who entered after me is just **size(tree) - rank**.

Okay with this as a hint, work out the full details. Think hard, it will be fun

## problem 6 - pre order and in order unique tree
Okay, the proof starts with the definition of preorder and inorder traversal. Then you need to answer the question at each node, does this goes on the left or right.

As for the details, i will leave it out for you to work out. But here i will be providing some sort of code  for rebuilding the tree.

```
function build(preorder, inorder) {
    char data = preorder[0];

    // QNS: whats the stopping criteria here?
    
    // QNS: best runtime? Why? log(n)
    int cnt = inorder.indexOf(data);
    int N = len(preorder);
    
    Node node = new Node(data);
    node.left = build(preorder.slice(1, cnt), inorder.slice(0, cnt-1));
    node.right = build(preorder.slice(1+cnt, N), inorder.slice(1+cnt, N));
    
    return node;
}
```
