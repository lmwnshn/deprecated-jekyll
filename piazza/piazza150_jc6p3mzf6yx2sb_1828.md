---
layout: page
title: Mutual Recursion
permalink: /piazza/jc6p3mzf6yx2sb_1828/
---

## Commentary

I personally have issues with my answer to this question, but it appears to have helped people, so well...

## Question

**Stream.tabulate**

Hello,

I'm having a little difficulty reasoning about Stream.tabulate

```
fun tabulate f = delay (fn () => tabulate' f)
  and tabulate' f = Cons(f(0), tabulate (fn i => f(i+1)))
```

Why does it feel like it is f(0) f(1) f(1) f(1).... instead of f(0), f(1), f(2), f(3)....?
I think I can understand using natural language saying that every time tabulate get called the i is already increased by one, but I guess I just don't know how to step through the code?

## Answer

Good morning!

I like to think of our mutually recursive functions as a ping pong ball bouncing around, though I'm not sure that's very relevant here.

But it's kind of like CPS, where we are modifying the function f each time. So all the tabulate' calls are using different f's.

Let's step through the tabulate!

If you call tabulate f, your first element is from tabulate' f, which is just f(0), like you said.

What will the rest of your elements be?

It'll be the result of tabulate (different f) where this (different f) is some sneaky looking monkey that will add one to whatever number you give it.

So in particular, when tabulate (different f) is called,
it will end up getting f(1).

So far, I think, you are ok until here.

Now let's think about what you get for the REST of the stream.

This time, it'll be the result of tabulate (yet another f), where (yet another f) is some cat that adds one, before passing it to the monkey that adds one.

You're wrapping the wrapped function! It might look something like this,

```
(fn j => (fn i => f (i+1)) (j+1)) (* functions are burritos *)
```

(when working recursively, I personally find it helpful to think of functions as creatures doing things instead of just saying function all the time - sorry if it doesn't help or made it more confusing)

In particular, two things that I think may be helpful to highlight:

I think you accept that we can wrap the f function with `(fn i => f (i+1))`. Well, nothing is stopping you from wrapping this wrapper function! So it's like `(fn j => (fn i => f (i+1)) (j+1))`

The Cons cells are really just "one element, and something that will give me the rest of the elements"
So in particular, each cell only cares about itself and its neighbor. Sure, you can get from the first cell to the 2190821903213th cell, but the first cell is only "directly connected to" the second cell.
So there's no reason that the first cell gets to tell everyone that they have to do f(1), f(1), ... and so on. It's not the same function and not the same i. You only get to tell the person next to you what you want.

If you're still not entirely sure, try explaining both your interpretations in a followup! We can look at it and figure out which part to talk about.
