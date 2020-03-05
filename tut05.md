# tutorial 5
sorry, eye wasnt too good today. the problems i went through in class should be quite simple to follow through. so here is one other question that i havent covered,

## PS5, prefix search
so most of you can do a normal trie word search, but what about prefix? how then will you deal with such things? well, intuitively, we can do it recursively.

okay, there are a few characters to take care of. dot, star, plus and question.

the base of it first, we are running in a recursive manner - our function will look something like these `f(List<String> resultList, String remainingQuery, TrieNode curr)`

### dot operator
Instead of explaining, it will be much easier to show, assume the dot operator, we will be passing the same `resultList` to be appended. Assume we run query `b.d`, and these will be the list of functions that will be called. If you meet a dot you effectively call every single of the next alphabet (assume we only got 4 alphabets - a, b, c, d).

```
- f(result, "b.d", Node(""))
    - f(result, ".d", Node("b")) 
        - f(result, "d", Node("ba"))
            f(result, "", Node("bad"))
        - f(result, "d", Node("bb"))
            f(result, "", Node("bbd"))
        - f(result, "d", Node("bc"))
            f(result, "", Node("bcd"))
        - f(result, "d", Node("bd"))
            f(result, "", Node("bdd"))
```

cool aint, make sense right, each dot operator will run a for loop through every single child node, and the code will look like so

```
// NOTE: assume TrieNode curr
// NOTE: string2 = string with first alphabet removed

for(int i=0; i<26; i++) {
    f(result, string2, Node(curr.arr[i]));
}
// NOTE: recursive func will always return the right result
```

### star operator
the star operator means the last character can be repeated zero or more times. Intuitively, this would mean going down the node with the node curr value, if it is a "o", we would want to make more "o", ideally, it should work something like these.

```
- f(result, "ho*pa", Node(""))
    - f(result, "o*pa", Node("h"))
        - f(result, "pa", Node("h")) // second char is a *
            - f(result, "a", Node("hp"))
                - f(result, "", Node("hpa"))
        - f(result, "*pa", Node("ho"))
            - f(result, "*pa", Node("ho"))
                - f(result, "a", Node("hop"))
                    - f(result, "", Node("hopa"))
                - f(result, "*pa", Node("hoo"))
                    - f(result, "a", Node("hoop"))
                        - f(result, "", Node("hoopa"))
                    - f(result, "*pa", Node("hooo"))
                        - f(result, "a", Node("hooop"))
                            - f(result, "", Node("hooopa"))
                // and the recursion goes on until you cannot find any more oooo
```

From the trace, you should be able to work out what you need to do for the function when you meet a star operator.

### The other operators
The rest should be fairly similar, just figure out what kind of trace you are expected, and it will be easy to see what your function should be like.

For combined operators, it is kinda similar to the star operator, you will have to keep the operators in the search string as you recursively traverse down every single possible combination.

### runtime
It wouldnt be too hard to work out the runtime given the code, give it a try.
