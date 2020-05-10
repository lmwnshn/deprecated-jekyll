---
layout: page
title: Motivating Red-Black Trees
permalink: /piazza/jklix7i05zj1mw_994/
---

## Note

**Motivating Red-Black Trees**

[This post is outside the scope of 150, read only if interested]

Trees are nice because they partition our elements into different spaces naturally, e.g. smaller things on left, bigger things on right, and you have a binary search tree that lets you find things in O(log N) time.

But the partitioning can be pretty stupid, you can have a tree that looks like a "right spine" where it only has right children. e.g. 1-2-3-4-5, and now you basically have a list. Where's your O(log N) now?

So self-balancing trees are nice. Most people will probably learn about AVL trees and red-black trees, and decide to completely abandon the latter. That's justifiable, because AVL trees make sense! Keep the heights kinda balanced, rotate around once or twice, and you're good. No fussing around with all the {red, black} x {child, uncle, grandparent} conditions.

But let's think about AVL trees. Why do we limit ourself to just one key and two children at every node? You could pack waaaay more data and divide your input into N spaces instead of 2 spaces at every step if you had multiple children... eventually, you'll come up with a 2-3-4 tree, which allows for multiple keys and children: a 2-node allows 1 key and 2 children, a 3-node allows 2 keys and 3 children, a 4-node allows 3 keys and 4 children.

Observation: red nodes, black nodes, and the links between them are more data than a simple AVL height per node. Harder observation: you can encode a 2-3-4 tree as a red-black tree. Very hard observation: the implementation becomes much simpler if you make it left leaning. Like 5x smaller in Java.

So, key takeaway: red-black trees in some sense encode more data than an AVL tree. Your trees will be much more efficient at storing data if you use them properly.

If you haven't clicked away yet, these [slides](https://www.cs.princeton.edu/~rs/talks/LLRB/RedBlack.pdf) are great and explain the above observations in detail. They're from a talk by Sedgewick, who wrote one of the popular implementation-heavy Algorithms textbooks and who also teaches algorithms courses on Coursera.

[In practice, if you really care about packing things into trees, you probably want a B+Tree. Those are really compact - 3 levels store 2.4 million entries aka 140ish MB, 4 levels store 312 million entries aka 18ish GB if I did my math right.  They also have sibling pointers among leaf nodes, which lets you iterate over a tree very speedily. These are widely used in actual existing databases, and are among the first serious data structure you'd implement if you were writing one. There's much more to trees than simple AVL!]
