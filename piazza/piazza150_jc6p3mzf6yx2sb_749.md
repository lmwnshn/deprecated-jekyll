---
layout: page
title: fact CPS Trace
permalink: /piazza/jc6p3mzf6yx2sb_749/
---

## Note

**factCPS Trace**

Hey everyone, just for your reference, here is an evaluation trace of `factCPS 3 s`, in all it's gory detail. Please read and understand what's happening here (it'll be useful for your homework!).

Here's the implementation of `factCPS`:

```
(* factCPS : int -> (int -> 'a) -> 'a
 * REQUIRES: n>=0
 * ENSURES: factCPS n s == s(n!)
 *)
fun factCPS 0 s = s 1
  | factCPS n s = factCPS (n-1) (fn res => s(n*res))
```

and here's a trace of it:

```
factCPS 3 s
  ==> factCPS 2 (fn res => s(3*res))
  ==> factCPS 1 (fn res' => (fn res => s(3*res)) (2 * res'))
  ==> factCPS 0 (fn res'' => (fn res' => (fn res => s(3*res)) (2 * res')) (1 * res''))
  ==> (fn res'' => (fn res' => (fn res => s(3*res)) (2 * res')) (1 * res'')) (1)
  ==> (fn res' => (fn res => s(3*res)) (2 * res')) (1 * 1)
  ==> (fn res' => (fn res => s(3*res)) (2 * res')) 1
  ==> (fn res => s(3*res)) (2 * 1)
  ==> (fn res => s(3*res)) 2
  ==> s(3*2)
  ==> s(6)
 ```

Hope this helps!

## Followup

In a similar "renaming to make life easier" spirit, I usually do

```
factCPS 3 s					s0 = s
  ==> factCPS 2 (fn res => s0 (3*res))		s1 = (fn res => s0 (3*res))
  ==> factCPS 1 (fn res => s1 (2*res))		s2 = (fn res => s1 (2*res))
  ==> factCPS 0 (fn res => s2 (1*res))		s3 = (fn res => s2 (1*res))
  ==> s3 1
  ==  (fn res => s2 (1*res)) 1
  ==> s2 (1*1)
  ==> s2 1
  ==  (fn res => s1 (2*res)) 1
  ==> s1 (2*1)
  ==> s1 2
  ==  (fn res => s0 (3*res)) 2
  ==> s0 (3*2)
  ==> s0 6
  ==  s 6
 ```

if I have to trace CPS code by hand. The main advantage is that you write less, your modified continuations are easy to keep track of, and you tend to have a pattern (like my s0 s1 s2 above) to guide you.

Jacob's original trace is still the correct way of going about it, though.

19/02/27: fixed typo, thanks for bringing it up!
