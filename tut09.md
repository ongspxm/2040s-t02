# tutorial 9

## the uphill downhill qns

Okay so the question is, given the constraints in the questions, will the uphill path will just the same as the downhill path? Intuitively this will be the case, but how do we formalize this intuition.

This is a sketch. We do so by contradiction. The total route taken for the run can be split into 2 parts, the first part uphill then the later part downhill. This means that there will be a highest point of the route, let's call that node H.

Let the uphill path will be U be a collection of nodes, `U=[S, n1, n2, ..., H]` representing the nodes taken for the uphill path. Let the downhill part be defined in the same way `D=[H, ..., S]`. 

We claim that if `U+D` is the longest path, then `|U| = |D| = x`. 

Suppose not, we have we have `|U| < |D|`, this means that `D` is a longer path than `U`. Let `D2` be the reverse of `D` which will be a valid uphill path. Therefore `|D+D2| > |U+D|` which contradicts the fact that `U+D` is the longest path.

Therefore, if `U+D` is the longest path, then `|U| = |D|`, and in this case, D can just be the reverse of U, since that will give us the same length of the path.

### this only works because ...
- the uphill and downhill cost is the same
- it is okay to travel a path multiple times
- a lot more, go through the proof, see what is dependent for each part to hold true.
