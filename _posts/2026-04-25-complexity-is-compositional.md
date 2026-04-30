---
layout: post
title: Complexity is compositional (sort of)
---

It's generally understood that time complexity is not compositional, i.e., knowing the complexity of two functions $$ f $$ and $$ g $$ (let's call them $$ E f $$ and $$ E g $$, respectively), not much can be said about the complexity of their composition $$ E (f \cdot g) $$.

Let's formalize $$ E $$ (E for efficiency, btw).


Consider a program represented by a mathematical function $$ f $$ that takes as input some _sizeable_ input, and returns some output.

$$ A \overset{f}{\longrightarrow} B$$


Since complexity is a measure of how a function's resource usage relates to the size of its input, we need the following two functions:

$$ A \overset{\tau f}{\longrightarrow} \mathbb{N}_0 $$

$$ A \overset{size}{\longrightarrow} \mathbb{N}_0 $$

A time function that takes an input of the same type as $$f$$'s inputs and returns the time (in computational steps) it takes to compute the result. And a self-explanatory $$size$$ function.

Efficiency is thus given by composing $$\tau f$$ with $$ size^\circ $$ (the inverse of $$size$$).

$$\mathbb{N}_0 \xrightarrow{\tau f \cdot size^\circ }  \mathbb{N}_0 $$

Lastly knowing that (just take me at my word)

$$ \tau (f \cdot g) = (\tau f) \cdot g + \tau g $$

We have all the necessary pieces to address the claim in the title. 

$$
E (f \cdot g) = \tau (f \cdot g) \cdot size^\circ \\ 
E (f \cdot g) = ((\tau f) \cdot g + \tau g ) \cdot size^\circ\\
E (f \cdot g) \subseteq (\tau f) \cdot g \cdot size^\circ + (\tau g) \cdot size^\circ $$

here comes the important step, adding $$ size^\circ \cdot size $$ on the right side, which we can do, because, $$ id \subseteq  size^\circ \cdot size $$

$$
E (f \cdot g) \subseteq (\tau f) \cdot size^\circ \cdot size  \cdot g \cdot size^\circ + (\tau g) \cdot size^\circ \\ 
E (f \cdot g) \subseteq (E f)  \cdot size  \cdot g \cdot size^\circ + E g \\ 
$$

Now we finally have an equation for $$ E f\cdot g$$ with mostly $$E$$'s on the right, the important thing, however, is $$ size  \cdot g \cdot size^\circ $$. This essentially reads "what $$g$$ does to size". We can call it " $$ g $$ abstraced by size" and write it as $$ g^{\#size}$$, finally we get this beautiful expression.

$$ 
E (f \cdot g) \subseteq (E f) \cdot g^{\#size} + E g
$$

Since the left side is contained in the right side, it's also true that the worst case of the left side is at most as bad as the worst case on the right side, which means we can Big-O this whole thing.

$$ 
\begin{aligned}
E (f \cdot g) & = \mathcal{O} ((E f) \cdot g^{\#size} + E g) \\
E (f \cdot g) & = \mathcal{O} ((E f) \cdot g^{\#size}) + \mathcal{O} (E g) \\
E (f \cdot g) & = \mathcal{O} \ E f \cdot \mathcal{O} g^{\#size} + \mathcal{O} \ E g \\
E (f \cdot g) & = \mathcal{O} \ E f \cdot \mathcal{O} g^{\#size} \sqcup \mathcal{O} \ E g
\end{aligned}
$$

This all means that composition is at most, as complex as the first function, or the second function when we also consider a kind of "size complexity".

So, if you relax, and if you squint, complexity is compositional.