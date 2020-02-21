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

## problem 4 - finger search on AVL tree
Okay finger search, what does it actually means. Finger search algorithm can be used on any data struction, and should work in this way. Given an input value `x`, search and return value `y`.

So how do you do this using a AVL tree? The nice thing about the AVL tree is that it is searchable, **how does this help**? First, here are a list of possible ideas 

- go all the way up to the root and come back down.
- searching all possible ancestor

So what if we combine both strategy? It will be abit hard to explain in words, so I will let the code do the work. But before that, it is crucial to understand the idea of **"lowest left branch ancestor"**.

nothing speaks as much as code, looks at the `llba` line and trace the tree yourself. This will bring you up to the node where the node last branches left. **Why is this important?**

The properly of binary search tree allows us to exploit all these comparision to determine which tree to search.

One of the other TA has so kindly worked out the detail and put all the proof in the code. Go through them and **convince yourself** why those proofs are correct. Now go draw some trees and test them out.

```
fingersearch(x, y) {
    // NOTE: search(x, y) is normal tree traversal
    if (x.val == y) return x

    if (x < y) {
        llba = x; while (llba.parent < llba) llba = llba.parent

        // NOTE: 2 cases
        // y could be in x.right
        // y could be in an lba (or its right subtree)
        // y CANNOT be in any rba (or their left subtrees)
        if (llba == null) return search(x.right, y);
        if (llba == y) return llba;

        // x.val < y < llba.val
        // Any lba of llba (and its right subtree) >= llba.val > y
        // Any rba of llba is rba of x, which we know doesn't work
        // Therefore y must be in x.right
        if (llba.val > y) return search(x.right, y);

        return search(llba, y);
    } else {
        lrba = x; while (lrba.parent > lrba) lrba = lrba.parent

        // NOTE: 2 cases
        // y could be in x.left
        // y could be in an rba (or its left subtree)
        // y CANNOT be in any lba (or their right subtrees)
        if (lrba == null) return search(x.left, y);
        if (lrba == y) return y;

        // lrba.val < y < x.val
        // Any rba of lrba (and its left subtree) <= lrba.val < y
        // Any lba of lrba is an lba of x, which we know doesn't work
        // Therefore y must be in x.left
        if (lrba.val < y): return search(x.left, y)

        // y < lrba.val < x.left < x.val
        // Therefore continue searching at lrba
        return search(lrba, y)
    }

}
```

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
