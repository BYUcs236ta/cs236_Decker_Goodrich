# Speeding_Up_Code

If your code is not running fast enough to pass the pass-off driver's tests, you will need to perform some code optimization to help it run more efficiently.

**Slides about generally optimizing C++ code can be found [here](https://docs.google.com/presentation/d/1PFfrHA29ME65d0KEaEmszE_XT5CecSdCzOvF2Ws0uj0/edit?usp=sharing).**  
  
Specifically for this project, here are some tips:

If your code is hitting the time limit, the problem is usually one (or more) of the following:

-   Your `Relation` class uses a vector to store its tuples, and you're manually sorting them. This is almost always done inefficiently. You should use `set<Tuple>` and implement the `bool Tuple::operator<(const Tuple& other) const` function
-   Your `Relation::join` function is implemented by doing the Cartesian product, selecting the overlapping columns, then projecting one. This is too slow. You should use the algorithm outlined in the help session slides (in nested for-loops you check tuple compatibility, and combine and add the tuples if they are).
-   In `Relation::join`, you're calculating which columns overlap  inside the nested for-loops. This is unbelievably inefficient. The columns that overlap don't change depending which pair of tuples you're looking at, and so should be calculated exactly once (so, outside the nested for-loops)
-   You're passing `Relation` objects into functions as regular objects (parameter type `Relation`), not as pointers (parameter type `Relation*`) or references (parameter type `Relation&` or `const Relation&`). If you're just passing them as plain `Relation` object, not pointers or references, then it is copying the relation every time it is passed.
-   In `Relation::join`, in the center of the nested for-loops, you're passing things into functions without them being references. For example, do you have a function that checks if two tuples are compatible? And if so, is it receiving those two tuples as references (`Tuple&` or `const Tuple&` or just plain `Tuple` objects? Just like the relations, if that is not done by reference then it is making unnecessary copies. If your two relations have ~100 tuples each, it is making ~20,000 unnecessary `Tuple` objects.
-   Your for-each loops are declared as `for(Tuple t : rel)`, but this is also copying tuples to pull them out of the relation. You could change it to const references to avoid that unnecessary copying. That looks like this: `for(const Tuple& t : rel)`. You need to use `const` here because we're iterating over a set. You cannot change the contents of a set as you iterate over it. If we're using references, that means we're using the originals (the ones actually in the set), not copies. The compiler will only let us use the originals if we take them as `const`.

If that still didn't help it, you can ask yourself the following questions:

-   Do I have unnecessary loops?
-   Am I calculating the exact same thing multiple times?