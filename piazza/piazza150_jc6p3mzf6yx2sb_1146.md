---
layout: page
title: searchPath CPS Trace
permalink: /piazza/jc6p3mzf6yx2sb_1146/
---

## Note

**Yet Another CPS Trace**

As promised in lab, here's a draft CPS evaluation trace of a slightly more complicated function.

I'll fix it up a bit when I finish my labs and midterm, so by around 5pm? Perhaps 9.

*Fixed up as of 3.30 pm.*

Some things I'd like to mention:

- you might think that explicitly defining these s1 s2 k1 k2 ... is very tedious, but there is usually a very nice pattern to them that helps you avoid mistakes
	- s1 = (fn res => s (LEFT::res))
	- s2 = (fn res => s1 (LEFT::res))
	- k1 = (fn () => searchPath p R1 (fn res => s (RIGHT::res)) k)
	- k2 = (fn () => searchPath p Empty (fn res => s1 (RIGHT::res)) k1)
	- you seeing a pattern?
- tracing is still very painful. Unless we ask you to trace it, you may want to consider appealing to your intuition.
- that said, tracing can support and complement your intuition
	- for example, my intuition says you call the failure continuation every time you bounce right
	- and if you know how many layers in you went, you can use this predictable pattern to extract the continuation at any node directly
	- but everyone has different intuition and there's no one right way of thinking about it
- technically I should show the expansion of p 1, p 2, p 3 but I think you all can do that

The main thing is to keep track of your changing continuations.

```
datatype direction = LEFT | RIGHT
(* searchPath : ('a -> bool) -> 'a tree -> (direction list -> 'b) -> (unit -> 'b) -> 'b *)
fun searchPath p Empty s k = k ()
  | searchPath p (Node(L,x,R)) s k = if p x then s [] else
          searchPath p L (fn res => s(LEFT::res)) (fn () => searchPath p R (fn res => s(RIGHT::res)) k)


(*
    Let T be the following tree
            1
        2       3
*)

searchPath (fn x => x = 3) (Node(Node(Empty, 2, Empty), 1, Node(Empty, 3, Empty))) SOME (fn () => NONE)

==

    letting
        p = (fn x => x = 3)
        T = (Node(Node(Empty, 2, Empty), 1, Node(Empty, 3, Empty)))
        s = SOME
        k = (fn () => NONE)

    searchPath p T s k

==
    letting
        from T = (Node(Node(Empty, 2, Empty), 1, Node(Empty, 3, Empty)))
            L1 = Node(Empty, 2, Empty)
            x = 1
            R1 = Node(Empty, 3, Empty)

    if p 1 then s [] else
    searchPath p L1 (fn res => s (LEFT::res)) (fn () => searchPath p R1 (fn res => s (RIGHT::res)) k)
==
    if false then s [] else
    searchPath p L1 (fn res => s (LEFT::res)) (fn () => searchPath p R1 (fn res => s (RIGHT::res)) k)
==
    searchPath p L1 (fn res => s (LEFT::res)) (fn () => searchPath p R1 (fn res => s (RIGHT::res)) k)
==
    searchPath p (Node(Empty, 2, Empty)) (fn res => s(LEFT::res)) (fn () => searchPath p R1 (fn res => s(RIGHT::res)) k)
==
    letting
        s1 = (fn res => s(LEFT::res))
        k1 = (fn () => searchPath p R1 (fn res => s (RIGHT::res)) k)

    searchPath p (Node(Empty, 2, Empty)) s1 k1
==
    if p 2 then s1 [] else
    searchPath p Empty (fn res => s1 (LEFT::res)) (fn () => searchPath p Empty (fn res => s1 (RIGHT::res)) k1)
==
    if false then s1 [] else
    searchPath p Empty (fn res => s1 (LEFT::res)) (fn () => searchPath p Empty (fn res => s1(RIGHT::res)) k1)
==
    searchPath p Empty (fn res => s1 (LEFT::res)) (fn () => searchPath p Empty (fn res => s1(RIGHT::res)) k1)
==
    letting
        s2 = (fn res => s1 (LEFT::res))
        k2 = (fn () => searchPath p Empty (fn res => s1 (RIGHT::res)) k1)

    searchPath p Empty s2 k2
==
    k2 ()
==
    (fn () => searchPath p Empty (fn res => s1 (RIGHT::res)) k1) ()
==
    searchPath p Empty (fn res => s1 (RIGHT::res)) k1
==
    k1 ()
==
    (fn () => searchPath p R1 (fn res => s (RIGHT::res)) k) ()
==
    searchPath p R1 (fn res => s (RIGHT::res)) k
==
    letting
        s3 = (fn res => s (RIGHT::res))

    searchPath p (Node(Empty, 3, Empty)) s3 k
==
    if p 3 then s3 [] else
    searchPath p Empty (fn res => s3 (LEFT::res)) (fn () => searchPath p Empty (fn res => s3 (RIGHT::res)) k)
==
    if true then s3 [] else
    searchPath p Empty (fn res => s3 (LEFT::res)) (fn () => searchPath p Empty (fn res => s3 (RIGHT::res)) k)
==
    s3 []
==
    (fn res => s (RIGHT::res)) []
==
    s (RIGHT::[])
==
    s [RIGHT]
==
    SOME [RIGHT]
```
