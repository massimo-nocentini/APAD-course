Exercises
=========

This chapter is an intermezzo that allows us to check and have a deeper
understanding of the concepts seen so far by means of exercises. We will
see how the code shown can be rewritten to take advantage of
battle-tested solutions and idioms that emerges from daily practice.

First of all, we import some modules

.. code:: ipython3

    import functools, operator, math, itertools, random, collections, statistics, bisect

that contains useful definitions for the code that we are going to
write. Moreover, an utility for generators,

.. code:: ipython3

    def take(iterable, n):
        yield from map(lambda p: p[1], zip(range(n), iterable))

that consumes an iterable and return a generator that will yield
:math:`n` objects at most. For the sake of clarity,

.. code:: ipython3

    taken = take(itertools.count(), 50)
    taken




.. parsed-literal::

    <generator object take at 0x7fde72786660>



is a actually generator and its content equals

.. code:: ipython3

    assert list(taken) == list(range(50))

Before starting, we initialize the random generator with a nice prime

.. code:: ipython3

    random.seed(11)

(Pythagorean) tuples
--------------------

Let

.. code:: ipython3

    def tuples(*slices):
        return itertools.product(*map(lambda s: range(s.start, s.stop), slices))

where

.. code:: ipython3

    help(itertools.product)


.. parsed-literal::

    Help on class product in module itertools:
    
    class product(builtins.object)
     |  product(*iterables, repeat=1) --> product object
     |  
     |  Cartesian product of input iterables.  Equivalent to nested for-loops.
     |  
     |  For example, product(A, B) returns the same as:  ((x,y) for x in A for y in B).
     |  The leftmost iterators are in the outermost for-loop, so the output tuples
     |  cycle in a manner similar to an odometer (with the rightmost element changing
     |  on every iteration).
     |  
     |  To compute the product of an iterable with itself, specify the number
     |  of repetitions with the optional repeat keyword argument. For example,
     |  product(A, repeat=4) means the same as product(A, A, A, A).
     |  
     |  product('ab', range(3)) --> ('a',0) ('a',1) ('a',2) ('b',0) ('b',1) ('b',2)
     |  product((0,1), (0,1), (0,1)) --> (0,0,0) (0,0,1) (0,1,0) (0,1,1) (1,0,0) ...
     |  
     |  Methods defined here:
     |  
     |  __getattribute__(self, name, /)
     |      Return getattr(self, name).
     |  
     |  __iter__(self, /)
     |      Implement iter(self).
     |  
     |  __next__(self, /)
     |      Implement next(self).
     |  
     |  __reduce__(...)
     |      Return state information for pickling.
     |  
     |  __setstate__(...)
     |      Set state information for unpickling.
     |  
     |  __sizeof__(...)
     |      Returns size in memory, in bytes.
     |  
     |  ----------------------------------------------------------------------
     |  Static methods defined here:
     |  
     |  __new__(*args, **kwargs) from builtins.type
     |      Create and return a new object.  See help(type) for accurate signature.
    


Consider the application to an empty sequence of ``slide``\ s,

.. code:: ipython3

    units = tuples()
    units




.. parsed-literal::

    <itertools.product at 0x7feb34e6fe80>



then saturate it

.. code:: ipython3

    list(units)




.. parsed-literal::

    [()]



Now, build tuples using just a ``slide`` object,

.. code:: ipython3

    singletons = tuples(slice(5, 11))
    singletons




.. parsed-literal::

    <itertools.product at 0x7feb34e66540>



then saturate it

.. code:: ipython3

    list(singletons)




.. parsed-literal::

    [(5,), (6,), (7,), (8,), (9,), (10,)]



Now, build tuples using a twin ``slide`` object,

.. code:: ipython3

    s = slice(5, 11)
    pairs = tuples(s, s)
    pairs




.. parsed-literal::

    <itertools.product at 0x7feb34e7a180>



then saturate it

.. code:: ipython3

    list(pairs)




.. parsed-literal::

    [(5, 5),
     (5, 6),
     (5, 7),
     (5, 8),
     (5, 9),
     (5, 10),
     (6, 5),
     (6, 6),
     (6, 7),
     (6, 8),
     (6, 9),
     (6, 10),
     (7, 5),
     (7, 6),
     (7, 7),
     (7, 8),
     (7, 9),
     (7, 10),
     (8, 5),
     (8, 6),
     (8, 7),
     (8, 8),
     (8, 9),
     (8, 10),
     (9, 5),
     (9, 6),
     (9, 7),
     (9, 8),
     (9, 9),
     (9, 10),
     (10, 5),
     (10, 6),
     (10, 7),
     (10, 8),
     (10, 9),
     (10, 10)]



Now, build tuples using a three different ``slide`` objects (taking into
account of splitting the returned generator),

.. code:: ipython3

    triples_a, triples_b = itertools.tee(tuples(slice(5, 11), slice(6, 13), slice(7, 14)))

where

.. code:: ipython3

    help(itertools.tee)


.. parsed-literal::

    Help on built-in function tee in module itertools:
    
    tee(iterable, n=2, /)
        Returns a tuple of n independent iterators.
    


then saturate it

.. code:: ipython3

    list(triples_a)




.. parsed-literal::

    [(5, 6, 7),
     (5, 6, 8),
     (5, 6, 9),
     (5, 6, 10),
     (5, 6, 11),
     (5, 6, 12),
     (5, 6, 13),
     (5, 7, 7),
     (5, 7, 8),
     (5, 7, 9),
     (5, 7, 10),
     (5, 7, 11),
     (5, 7, 12),
     (5, 7, 13),
     (5, 8, 7),
     (5, 8, 8),
     (5, 8, 9),
     (5, 8, 10),
     (5, 8, 11),
     (5, 8, 12),
     (5, 8, 13),
     (5, 9, 7),
     (5, 9, 8),
     (5, 9, 9),
     (5, 9, 10),
     (5, 9, 11),
     (5, 9, 12),
     (5, 9, 13),
     (5, 10, 7),
     (5, 10, 8),
     (5, 10, 9),
     (5, 10, 10),
     (5, 10, 11),
     (5, 10, 12),
     (5, 10, 13),
     (5, 11, 7),
     (5, 11, 8),
     (5, 11, 9),
     (5, 11, 10),
     (5, 11, 11),
     (5, 11, 12),
     (5, 11, 13),
     (5, 12, 7),
     (5, 12, 8),
     (5, 12, 9),
     (5, 12, 10),
     (5, 12, 11),
     (5, 12, 12),
     (5, 12, 13),
     (6, 6, 7),
     (6, 6, 8),
     (6, 6, 9),
     (6, 6, 10),
     (6, 6, 11),
     (6, 6, 12),
     (6, 6, 13),
     (6, 7, 7),
     (6, 7, 8),
     (6, 7, 9),
     (6, 7, 10),
     (6, 7, 11),
     (6, 7, 12),
     (6, 7, 13),
     (6, 8, 7),
     (6, 8, 8),
     (6, 8, 9),
     (6, 8, 10),
     (6, 8, 11),
     (6, 8, 12),
     (6, 8, 13),
     (6, 9, 7),
     (6, 9, 8),
     (6, 9, 9),
     (6, 9, 10),
     (6, 9, 11),
     (6, 9, 12),
     (6, 9, 13),
     (6, 10, 7),
     (6, 10, 8),
     (6, 10, 9),
     (6, 10, 10),
     (6, 10, 11),
     (6, 10, 12),
     (6, 10, 13),
     (6, 11, 7),
     (6, 11, 8),
     (6, 11, 9),
     (6, 11, 10),
     (6, 11, 11),
     (6, 11, 12),
     (6, 11, 13),
     (6, 12, 7),
     (6, 12, 8),
     (6, 12, 9),
     (6, 12, 10),
     (6, 12, 11),
     (6, 12, 12),
     (6, 12, 13),
     (7, 6, 7),
     (7, 6, 8),
     (7, 6, 9),
     (7, 6, 10),
     (7, 6, 11),
     (7, 6, 12),
     (7, 6, 13),
     (7, 7, 7),
     (7, 7, 8),
     (7, 7, 9),
     (7, 7, 10),
     (7, 7, 11),
     (7, 7, 12),
     (7, 7, 13),
     (7, 8, 7),
     (7, 8, 8),
     (7, 8, 9),
     (7, 8, 10),
     (7, 8, 11),
     (7, 8, 12),
     (7, 8, 13),
     (7, 9, 7),
     (7, 9, 8),
     (7, 9, 9),
     (7, 9, 10),
     (7, 9, 11),
     (7, 9, 12),
     (7, 9, 13),
     (7, 10, 7),
     (7, 10, 8),
     (7, 10, 9),
     (7, 10, 10),
     (7, 10, 11),
     (7, 10, 12),
     (7, 10, 13),
     (7, 11, 7),
     (7, 11, 8),
     (7, 11, 9),
     (7, 11, 10),
     (7, 11, 11),
     (7, 11, 12),
     (7, 11, 13),
     (7, 12, 7),
     (7, 12, 8),
     (7, 12, 9),
     (7, 12, 10),
     (7, 12, 11),
     (7, 12, 12),
     (7, 12, 13),
     (8, 6, 7),
     (8, 6, 8),
     (8, 6, 9),
     (8, 6, 10),
     (8, 6, 11),
     (8, 6, 12),
     (8, 6, 13),
     (8, 7, 7),
     (8, 7, 8),
     (8, 7, 9),
     (8, 7, 10),
     (8, 7, 11),
     (8, 7, 12),
     (8, 7, 13),
     (8, 8, 7),
     (8, 8, 8),
     (8, 8, 9),
     (8, 8, 10),
     (8, 8, 11),
     (8, 8, 12),
     (8, 8, 13),
     (8, 9, 7),
     (8, 9, 8),
     (8, 9, 9),
     (8, 9, 10),
     (8, 9, 11),
     (8, 9, 12),
     (8, 9, 13),
     (8, 10, 7),
     (8, 10, 8),
     (8, 10, 9),
     (8, 10, 10),
     (8, 10, 11),
     (8, 10, 12),
     (8, 10, 13),
     (8, 11, 7),
     (8, 11, 8),
     (8, 11, 9),
     (8, 11, 10),
     (8, 11, 11),
     (8, 11, 12),
     (8, 11, 13),
     (8, 12, 7),
     (8, 12, 8),
     (8, 12, 9),
     (8, 12, 10),
     (8, 12, 11),
     (8, 12, 12),
     (8, 12, 13),
     (9, 6, 7),
     (9, 6, 8),
     (9, 6, 9),
     (9, 6, 10),
     (9, 6, 11),
     (9, 6, 12),
     (9, 6, 13),
     (9, 7, 7),
     (9, 7, 8),
     (9, 7, 9),
     (9, 7, 10),
     (9, 7, 11),
     (9, 7, 12),
     (9, 7, 13),
     (9, 8, 7),
     (9, 8, 8),
     (9, 8, 9),
     (9, 8, 10),
     (9, 8, 11),
     (9, 8, 12),
     (9, 8, 13),
     (9, 9, 7),
     (9, 9, 8),
     (9, 9, 9),
     (9, 9, 10),
     (9, 9, 11),
     (9, 9, 12),
     (9, 9, 13),
     (9, 10, 7),
     (9, 10, 8),
     (9, 10, 9),
     (9, 10, 10),
     (9, 10, 11),
     (9, 10, 12),
     (9, 10, 13),
     (9, 11, 7),
     (9, 11, 8),
     (9, 11, 9),
     (9, 11, 10),
     (9, 11, 11),
     (9, 11, 12),
     (9, 11, 13),
     (9, 12, 7),
     (9, 12, 8),
     (9, 12, 9),
     (9, 12, 10),
     (9, 12, 11),
     (9, 12, 12),
     (9, 12, 13),
     (10, 6, 7),
     (10, 6, 8),
     (10, 6, 9),
     (10, 6, 10),
     (10, 6, 11),
     (10, 6, 12),
     (10, 6, 13),
     (10, 7, 7),
     (10, 7, 8),
     (10, 7, 9),
     (10, 7, 10),
     (10, 7, 11),
     (10, 7, 12),
     (10, 7, 13),
     (10, 8, 7),
     (10, 8, 8),
     (10, 8, 9),
     (10, 8, 10),
     (10, 8, 11),
     (10, 8, 12),
     (10, 8, 13),
     (10, 9, 7),
     (10, 9, 8),
     (10, 9, 9),
     (10, 9, 10),
     (10, 9, 11),
     (10, 9, 12),
     (10, 9, 13),
     (10, 10, 7),
     (10, 10, 8),
     (10, 10, 9),
     (10, 10, 10),
     (10, 10, 11),
     (10, 10, 12),
     (10, 10, 13),
     (10, 11, 7),
     (10, 11, 8),
     (10, 11, 9),
     (10, 11, 10),
     (10, 11, 11),
     (10, 11, 12),
     (10, 11, 13),
     (10, 12, 7),
     (10, 12, 8),
     (10, 12, 9),
     (10, 12, 10),
     (10, 12, 11),
     (10, 12, 12),
     (10, 12, 13)]



Now a corner case, but still interesting for ensuring a sound behavior,

.. code:: ipython3

    triples = tuples(slice(5, 11), slice(6, 6), slice(7, 14)) # ouch!

then saturate it

.. code:: ipython3

    list(triples) # who we have to blame?




.. parsed-literal::

    []



Finally, let

.. code:: ipython3

    def is_pythagorean(tup, n=2):
        '''A Pythagorean triple consists of three positive integers a, b, and c, such that a^2 + b^2 = c^2. 
        
        Such a triple is commonly written (a, b, c), and a well-known example is (3, 4, 5). 
        If (a, b, c) is a Pythagorean triple, then so is (ka, kb, kc) for any positive integer k. 
        
        A primitive Pythagorean triple is one in which a, b and c are coprime (that is, 
        they have no common divisor larger than 1).
        
        See also https://en.wikipedia.org/wiki/Pythagorean_triple.
        '''
        a, b, c = tup
        return (tup[0]**n + tup[1]**n == tup[2]**n) if a <= b <= c else False

in

.. code:: ipython3

    list(filter(is_pythagorean, triples_b))




.. parsed-literal::

    [(5, 12, 13), (6, 8, 10)]



and

.. code:: ipython3

    help(is_pythagorean) # just to show that writing docstrings is cool and useful.


.. parsed-literal::

    Help on function is_pythagorean in module __main__:
    
    is_pythagorean(tup, n=2)
        A Pythagorean triple consists of three positive integers a, b, and c, such that a^2 + b^2 = c^2. 
        
        Such a triple is commonly written (a, b, c), and a well-known example is (3, 4, 5). 
        If (a, b, c) is a Pythagorean triple, then so is (ka, kb, kc) for any positive integer k. 
        
        A primitive Pythagorean triple is one in which a, b and c are coprime (that is, 
        they have no common divisor larger than 1).
        
        See also https://en.wikipedia.org/wiki/Pythagorean_triple.
    


``sum_upto``
------------

Let

.. code:: ipython3

    def sum_upto(n):
        return functools.reduce(operator.add, range(n+1))

and test according to Euler’s quicker formula

.. code:: ipython3

    n = 100
    v = sum_upto(n)
    assert v == (n*(n+1)/2) == 5050

where

.. code:: ipython3

    help(functools.reduce)


.. parsed-literal::

    Help on built-in function reduce in module _functools:
    
    reduce(...)
        reduce(function, sequence[, initial]) -> value
        
        Apply a function of two arguments cumulatively to the items of a sequence,
        from left to right, so as to reduce the sequence to a single value.
        For example, reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) calculates
        ((((1+2)+3)+4)+5).  If initial is present, it is placed before the items
        of the sequence in the calculation, and serves as a default when the
        sequence is empty.
    


and

.. code:: ipython3

    help(operator.add)


.. parsed-literal::

    Help on built-in function add in module _operator:
    
    add(a, b, /)
        Same as a + b.
    


``sqrt``
--------

Let

.. code:: ipython3

    def sqrt(n):
        
        yield n
        refined = n/2
        while True:
            yield refined
            refined = (n/refined + refined)/2

to enumerate 15 approximation of the square root of 37

.. code:: ipython3

    n = 37
    list(take(sqrt(37), 15))




.. parsed-literal::

    [37,
     18.5,
     10.25,
     6.929878048780488,
     6.134538672432479,
     6.082981028300877,
     6.082762534222396,
     6.08276253029822,
     6.08276253029822,
     6.08276253029822,
     6.08276253029822,
     6.08276253029822,
     6.08276253029822,
     6.08276253029822,
     6.08276253029822]



and check with respect to

.. code:: ipython3

    math.sqrt(n)




.. parsed-literal::

    6.082762530298219



where

.. code:: ipython3

    help(math.sqrt)


.. parsed-literal::

    Help on built-in function sqrt in module math:
    
    sqrt(x, /)
        Return the square root of x.
    


:math:`\pi`
-----------

According to https://en.wikipedia.org/wiki/Leibniz_formula_for_%CF%80,
let

.. code:: ipython3

    def pi_Leibniz():
        
        d = 0
        for i, coeff in enumerate(itertools.count(1, step=2)):
            yield 4*d
            d += (-1)**i/coeff

in

.. code:: ipython3

    list(take(pi_Leibniz(), 1000))[-10:]




.. parsed-literal::

    [3.140582552837346,
     3.1426017350685425,
     3.140584589329763,
     3.1425997026798886,
     3.140586617627045,
     3.142597678461635,
     3.1405886377785612,
     3.1425956623646125,
     3.140590649833284,
     3.142593654340044]



and check against the

.. code:: ipython3

    math.pi




.. parsed-literal::

    3.141592653589793



where

.. code:: ipython3

    help(itertools.count)


.. parsed-literal::

    Help on class count in module itertools:
    
    class count(builtins.object)
     |  count(start=0, step=1)
     |  
     |  Return a count object whose .__next__() method returns consecutive values.
     |  
     |  Equivalent to:
     |      def count(firstval=0, step=1):
     |          x = firstval
     |          while 1:
     |              yield x
     |              x += step
     |  
     |  Methods defined here:
     |  
     |  __getattribute__(self, name, /)
     |      Return getattr(self, name).
     |  
     |  __iter__(self, /)
     |      Implement iter(self).
     |  
     |  __next__(self, /)
     |      Implement next(self).
     |  
     |  __reduce__(...)
     |      Return state information for pickling.
     |  
     |  __repr__(self, /)
     |      Return repr(self).
     |  
     |  ----------------------------------------------------------------------
     |  Static methods defined here:
     |  
     |  __new__(*args, **kwargs) from builtins.type
     |      Create and return a new object.  See help(type) for accurate signature.
    


The Collatz’s conjecture
------------------------

Consider the following operation on an arbitrary positive integer:

::

   If the number is even, divide it by two.
   If the number is odd, triple it and add one.

See also https://en.wikipedia.org/wiki/Collatz_conjecture. Let

.. code:: ipython3

    def collatz(n):
        
        yield n
        
        while True:
            n = 3*n + 1 if n % 2 else n // 2
            yield n

in

.. code:: ipython3

    [list(take(collatz(n), 15)) for n in range(1, 20)]




.. parsed-literal::

    [[1, 4, 2, 1, 4, 2, 1, 4, 2, 1, 4, 2, 1, 4, 2],
     [2, 1, 4, 2, 1, 4, 2, 1, 4, 2, 1, 4, 2, 1, 4],
     [3, 10, 5, 16, 8, 4, 2, 1, 4, 2, 1, 4, 2, 1, 4],
     [4, 2, 1, 4, 2, 1, 4, 2, 1, 4, 2, 1, 4, 2, 1],
     [5, 16, 8, 4, 2, 1, 4, 2, 1, 4, 2, 1, 4, 2, 1],
     [6, 3, 10, 5, 16, 8, 4, 2, 1, 4, 2, 1, 4, 2, 1],
     [7, 22, 11, 34, 17, 52, 26, 13, 40, 20, 10, 5, 16, 8, 4],
     [8, 4, 2, 1, 4, 2, 1, 4, 2, 1, 4, 2, 1, 4, 2],
     [9, 28, 14, 7, 22, 11, 34, 17, 52, 26, 13, 40, 20, 10, 5],
     [10, 5, 16, 8, 4, 2, 1, 4, 2, 1, 4, 2, 1, 4, 2],
     [11, 34, 17, 52, 26, 13, 40, 20, 10, 5, 16, 8, 4, 2, 1],
     [12, 6, 3, 10, 5, 16, 8, 4, 2, 1, 4, 2, 1, 4, 2],
     [13, 40, 20, 10, 5, 16, 8, 4, 2, 1, 4, 2, 1, 4, 2],
     [14, 7, 22, 11, 34, 17, 52, 26, 13, 40, 20, 10, 5, 16, 8],
     [15, 46, 23, 70, 35, 106, 53, 160, 80, 40, 20, 10, 5, 16, 8],
     [16, 8, 4, 2, 1, 4, 2, 1, 4, 2, 1, 4, 2, 1, 4],
     [17, 52, 26, 13, 40, 20, 10, 5, 16, 8, 4, 2, 1, 4, 2],
     [18, 9, 28, 14, 7, 22, 11, 34, 17, 52, 26, 13, 40, 20, 10],
     [19, 58, 29, 88, 44, 22, 11, 34, 17, 52, 26, 13, 40, 20, 10]]



Fibonacci numbers
-----------------

Directly from
https://docs.python.org/3/library/functools.html#functools.cache:

.. code:: ipython3

    @functools.lru_cache()
    def factorial(n):
        print('•', end='')
        return n * factorial(n-1) if n else 1

no previously cached result, makes 11 recursive calls (count the •
symbols)

.. code:: ipython3

    factorial(10)


.. parsed-literal::

    •••••••••••



.. parsed-literal::

    3628800



just looks up cached value result

.. code:: ipython3

    factorial(5)




.. parsed-literal::

    120



makes two new recursive calls, the other 10 are cached

.. code:: ipython3

    factorial(12)


.. parsed-literal::

    ••



.. parsed-literal::

    479001600



Uniform ``random`` on segmented interval
----------------------------------------

The problem here reads as follow: sample uniformly from :math:`[a, b)`
and :math:`[c, d)` where :math:`b <= c`. Eventually, try to generate to
an arbitrary sequence of ``slice``\ s, assuming they are fed in sorted
order with respect to ``<``.

.. code:: ipython3

    help(random.random)


.. parsed-literal::

    Help on built-in function random:
    
    random() method of random.Random instance
        random() -> x in the interval [0, 1).
    


.. code:: ipython3

    def samples(*slices):
        
        step = 1/len(slices)
        
        steps = itertools.count(step, step)
        bins = [(s, sl) for sl, s in zip(slices, steps)]
        
        while True:
            r = random.random()
            i = bisect.bisect_left(bins, (r, None))
            sl = slices[i]
            yield abs(sl.stop - sl.start) * (r - (i*step))/step + sl.start

.. code:: ipython3

    samples(slice(10, 20), slice(35, 40))




.. parsed-literal::

    <generator object samples at 0x7fafff38cdd0>



Then define the generator with respect to :math:`[10, 20)` and
:math:`[35, 40)`

.. code:: ipython3

    observations = take(samples(slice(10, 20), slice(35, 40)), 1000000)

have a look at some observations

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. parsed-literal::

    [10.065881461992102,
     10.330509017333556,
     10.441575839089332,
     10.604719759225034,
     10.858605175592535,
     11.236798187996238,
     11.641011443860833,
     11.871518103236177,
     11.986890460958296,
     12.075267775426607,
     12.608099325319147,
     12.798363472905239,
     12.856743881488764,
     13.08154257522419,
     13.362210892491294,
     14.128168241443795,
     14.315705968961579,
     14.37896297484304,
     14.450627795676287,
     14.61079891522304,
     14.760075813291877,
     14.902951957145492,
     15.08834694399302,
     15.371167906444871,
     15.3899538387929,
     15.610563670725771,
     16.09503987209427,
     16.46130759991724,
     16.481315583169696,
     16.6389393148966,
     16.763965750086612,
     17.161534614964296,
     17.64373100766722,
     17.6864448329581,
     17.8612019835546,
     18.07524974261635,
     18.122155188589808,
     18.19965345762347,
     18.441417169832395,
     18.549634661008042,
     18.721268695280955,
     18.74098994120328,
     18.860144417322793,
     19.39519055306905,
     19.586777618471984,
     19.81315603849211,
     19.872737404498217,
     35.208497374763105,
     35.2716370564889,
     35.37839974752564,
     35.515220220228784,
     35.54564539798049,
     35.601439633620295,
     35.61978555506285,
     35.71956553656463,
     35.875789991904966,
     35.99354136611884,
     36.03831329597236,
     36.30504845696931,
     36.40586779191911,
     36.453695545741105,
     36.55320010711284,
     36.565632598338325,
     36.57181860190921,
     36.611399976317614,
     36.64091434197609,
     36.8497199427049,
     37.008698108611824,
     37.26189280583681,
     37.2968682945781,
     37.31188336629809,
     37.407519263139065,
     37.50022259557803,
     37.59782524171885,
     37.6433676336202,
     37.78749072008687,
     37.89417335165471,
     37.907818486628635,
     38.00838189076956,
     38.20612732931414,
     38.28062748521115,
     38.316249792537675,
     38.43330376572454,
     38.504717212958525,
     38.58303485698609,
     38.59711217550104,
     38.713565537601234,
     38.73938020735429,
     38.80049218866173,
     38.880968853368245,
     38.94168997915732,
     39.00044528747109,
     39.09218444693862,
     39.31381532329558,
     39.38104134484665,
     39.382106599952536,
     39.45089464453682,
     39.5383639149662,
     39.66637351444338,
     39.71730205629479]



then observe the quantiles:

.. code:: ipython3

    statistics.quantiles(observations)




.. parsed-literal::

    [15.020211187738782, 35.009163151922635, 37.50463277093116]



it looks uniform. By the way, use different intervals, :math:`[14, 20)`
and :math:`[35,40)`,

.. code:: ipython3

    observations = take(samples(slice(14, 20), slice(35, 40)), 1000000)

look again at some observations,

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. parsed-literal::

    [14.2894744307065,
     14.567855525427795,
     14.614501321625536,
     14.852171677145659,
     14.907211181252041,
     14.968285918761719,
     15.12747212461315,
     15.256758785197473,
     15.319819952616513,
     15.44896101893795,
     15.46949182637184,
     15.525693468769717,
     15.529163152061802,
     15.564301242054672,
     15.669988205033237,
     15.91439408404592,
     15.97406682761393,
     16.054197676349123,
     16.07790148761498,
     16.182666101173634,
     16.19499475043846,
     16.54359355182931,
     16.554672845276883,
     16.63735733131587,
     16.90013210927373,
     16.91040518268221,
     17.14407920870184,
     17.19439573863112,
     17.20656960159678,
     17.21169406763174,
     17.266377968321734,
     17.449317097232196,
     17.50499488355765,
     17.50874192896787,
     17.519388933926933,
     17.73526944538669,
     17.795135494880203,
     17.96784338955439,
     18.01182702858506,
     18.27125028419525,
     18.782276887216405,
     18.845361033130622,
     18.90887123130714,
     19.006137495735132,
     19.149765544089583,
     19.225553556099744,
     19.326002868087613,
     19.351687271024076,
     19.49275684134995,
     19.529506644760723,
     19.543141154311517,
     19.80679305871736,
     19.818909959896757,
     19.960761485115178,
     35.2264429158269,
     35.26316134160675,
     35.39717789685274,
     35.413270769481564,
     35.61631140963445,
     35.710258422050224,
     35.71078259443684,
     35.73964330872067,
     36.361266985148426,
     36.4220113449107,
     36.533993761996705,
     36.588136576654655,
     36.70500446716608,
     36.82761734963019,
     36.88363712134305,
     37.230062120983305,
     37.23497490455708,
     37.39078349992419,
     37.451982805632326,
     37.481621559867875,
     37.48253856912495,
     37.532357563267674,
     37.68122554965463,
     37.81658550477775,
     37.846704164294735,
     37.85993954384434,
     37.93646175001616,
     37.98371066447644,
     38.02974643570607,
     38.193978067218744,
     38.496944350120096,
     38.505911198634664,
     38.58921207458006,
     38.661575759662924,
     38.723735852937985,
     38.959886136067254,
     39.08005250872501,
     39.09641170421992,
     39.26899396917356,
     39.27978890748443,
     39.316603621630925,
     39.319574830618784,
     39.32744441668071,
     39.39013442125252,
     39.65058001626882,
     39.918848103357355]



and check the corresponding quantiles

.. code:: ipython3

    statistics.quantiles(observations)




.. parsed-literal::

    [17.001128260345325, 35.00061212444177, 37.50551249443724]



it should be uniform too. Finally, we test the corner case where
:math:`b=c`, so let :math:`[10, 20)` and :math:`[20,40)`,

.. code:: ipython3

    observations = take(samples(slice(10, 20), slice(20, 40)), 1000000)

look again at some observations,

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. parsed-literal::

    [10.035161524701131,
     10.40308438308476,
     10.453627899958564,
     10.894287941852813,
     11.137389035252255,
     11.271567923695416,
     11.30608699829178,
     11.544522349021864,
     11.74277921905845,
     12.012069938519831,
     12.136974751909504,
     12.324266350252092,
     12.49959389354197,
     12.61010885615925,
     13.019606869537146,
     13.288054541218981,
     13.423939916930628,
     13.511568813631374,
     13.737441129061065,
     13.861306291643,
     14.258184740155516,
     14.474335346483734,
     14.593693889035517,
     14.60801038570705,
     14.749833724505542,
     14.870016930190468,
     14.876891369223362,
     15.093252763369293,
     15.135157633206049,
     15.519619396036088,
     15.672097842350068,
     15.750183667118385,
     16.047501759768053,
     16.76510885338166,
     17.065174770528312,
     17.187341077888263,
     17.533837405071807,
     17.943968436070566,
     18.580525687521288,
     18.602115267373318,
     18.741597072479713,
     18.755126014685075,
     19.430709547193594,
     20.328030011433228,
     21.065457638872587,
     21.150029497917092,
     21.64331235554475,
     21.675688011896533,
     21.80487140703719,
     22.125989788171488,
     22.17115789998934,
     22.54764493635133,
     22.548382726885393,
     22.954332733511635,
     24.01713576294756,
     24.12811486601295,
     24.322417996460175,
     24.86966422112255,
     25.175480248086217,
     26.129652584544694,
     26.200480003591192,
     26.24826818412849,
     27.306135759910415,
     27.562416029763554,
     28.47232080819992,
     28.598240284545177,
     28.707267102509274,
     28.787179394128973,
     28.794166965621116,
     29.027213662092024,
     30.03125474152972,
     30.066841406703922,
     30.62979241143689,
     30.707624576632504,
     30.98329013071671,
     31.795902901342885,
     33.21427078869403,
     33.314753399381374,
     33.317976586713286,
     33.34406810249045,
     33.55936812683325,
     33.730632511361236,
     33.80904945274633,
     34.531591720999316,
     34.81812280038163,
     35.26864270173962,
     35.306192053740496,
     35.559079274178146,
     35.66801886022802,
     35.906444779733995,
     36.97931930984736,
     37.063335579459235,
     37.710685335336294,
     38.33042629574662,
     38.649173590761734,
     38.889346630611854,
     39.19937308645714,
     39.3384049281577,
     39.61681208427888,
     39.72508342159965]



and check the corresponding quantiles

.. code:: ipython3

    statistics.quantiles(observations)




.. parsed-literal::

    [15.008642335636235, 20.01690595801498, 30.01472494058433]



it should be uniform either. Finally, attempt a sampling from ``4``
slices,

.. code:: ipython3

    observations = take(samples(slice(0, 5), slice(10, 15), slice(20, 25), slice(30, 35)), 1000000)

look again at some observations,

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. parsed-literal::

    [0.11923026607978837,
     0.20622452924056178,
     0.6427695574235814,
     0.6931050064070732,
     0.7782212305433633,
     0.9047274444387332,
     1.0822218428878982,
     1.3726146723625865,
     1.618923732998998,
     1.8385208349809212,
     1.947641224796055,
     2.3410522857775073,
     2.4413920911374265,
     2.671911366387516,
     2.814955074658434,
     3.092914489684082,
     3.5020880409413224,
     3.592949885528516,
     4.246285417360472,
     4.559865356341081,
     4.579226492320392,
     4.767941092068093,
     10.044642533351421,
     10.08020363010737,
     10.21446040236147,
     10.248407701391995,
     10.347838101020972,
     10.463062241451,
     10.496132827744978,
     10.523422077785238,
     10.566932540109637,
     10.59104497781158,
     10.85364531530621,
     10.947077934382033,
     11.204845397741996,
     11.279279126164397,
     11.43030370617164,
     11.538631149765106,
     11.640385607920683,
     11.688425359613321,
     11.96166099423535,
     11.983792454792932,
     12.17860887400474,
     12.614946135952351,
     12.756170091509397,
     13.028057221653018,
     13.166899825688429,
     13.316065559382116,
     13.428823087943842,
     13.514662599651013,
     13.551691062794223,
     13.747270713216999,
     13.815685285706383,
     14.996054887560593,
     20.037346049071566,
     20.091942199419293,
     20.100425729725625,
     20.36815544969909,
     20.99704408429071,
     21.391432825963722,
     21.530646833486205,
     21.71999529101945,
     21.757565069058362,
     21.775545596682772,
     21.99540772118588,
     22.016378465189284,
     22.032364481742768,
     22.676534848811414,
     22.71439964846263,
     22.910516341080204,
     22.965985343136843,
     22.98277899475418,
     23.157785815015348,
     23.446981121415423,
     24.56634224541175,
     24.577255309494504,
     24.63408067498932,
     24.69115607963368,
     24.79324917427499,
     24.855393479827413,
     30.26976515728243,
     30.335775525783216,
     30.432092088269027,
     30.734904305546827,
     30.884700861618093,
     31.058427345902192,
     31.06339521360841,
     31.092998268532966,
     31.649996202110536,
     32.071258272297186,
     32.46482608354354,
     33.21858345263357,
     33.32071681501621,
     33.59112941584716,
     33.651825164261474,
     33.70124047645148,
     33.81505139831141,
     33.9157171998315,
     34.028172038278875,
     34.05409261039254]



and check the corresponding quantiles

.. code:: ipython3

    statistics.quantiles(observations)




.. parsed-literal::

    [4.997958378071746, 14.989752400547736, 24.992706341887065]



it should be uniform either.

Bernoulli random variable
-------------------------

.. code:: ipython3

    int(True) # this is a very quick check to see if a Boolean can be used as integer




.. parsed-literal::

    1



.. code:: ipython3

    def Bernoulli(p):
        'This is a generator for a Bernoulli random variable of parameter `p` for success.'
        
        while True:              # forever we loop
            r = random.random()         # get a sample
            yield int(r < p)    # if that sample denotes a success or a failure we *yield* that outcome

.. code:: ipython3

    B = Bernoulli(p=0.6) # B is our random variable
    B




.. parsed-literal::

    <generator object Bernoulli at 0x7f332bed2900>



.. code:: ipython3

    next(B)




.. parsed-literal::

    1



.. code:: ipython3

    next(B)




.. parsed-literal::

    0



.. code:: ipython3

    next(B)




.. parsed-literal::

    1



.. code:: ipython3

    next(B)




.. parsed-literal::

    0



.. code:: ipython3

    list(take(B, 20))




.. parsed-literal::

    [1, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 0, 1, 1]



.. code:: ipython3

    C = collections.Counter(take(B, 1_000_000))
    C




.. parsed-literal::

    Counter({0: 399494, 1: 600506})



.. code:: ipython3

    C[1]/(C[0]+C[1])




.. parsed-literal::

    0.600506



where

.. code:: ipython3

    print(collections.Counter.__doc__)


.. parsed-literal::

    Dict subclass for counting hashable items.  Sometimes called a bag
        or multiset.  Elements are stored as dictionary keys and their counts
        are stored as dictionary values.
    
        >>> c = Counter('abcdeabcdabcaba')  # count elements from a string
    
        >>> c.most_common(3)                # three most common elements
        [('a', 5), ('b', 4), ('c', 3)]
        >>> sorted(c)                       # list all unique elements
        ['a', 'b', 'c', 'd', 'e']
        >>> ''.join(sorted(c.elements()))   # list elements with repetitions
        'aaaaabbbbcccdde'
        >>> sum(c.values())                 # total of all counts
        15
    
        >>> c['a']                          # count of letter 'a'
        5
        >>> for elem in 'shazam':           # update counts from an iterable
        ...     c[elem] += 1                # by adding 1 to each element's count
        >>> c['a']                          # now there are seven 'a'
        7
        >>> del c['b']                      # remove all 'b'
        >>> c['b']                          # now there are zero 'b'
        0
    
        >>> d = Counter('simsalabim')       # make another counter
        >>> c.update(d)                     # add in the second counter
        >>> c['a']                          # now there are nine 'a'
        9
    
        >>> c.clear()                       # empty the counter
        >>> c
        Counter()
    
        Note:  If a count is set to zero or reduced to zero, it will remain
        in the counter until the entry is deleted or the counter is cleared:
    
        >>> c = Counter('aaabbc')
        >>> c['b'] -= 2                     # reduce the count of 'b' by two
        >>> c.most_common()                 # 'b' is still in, but its count is zero
        [('a', 3), ('c', 1), ('b', 0)]
    
        


Russian Peasant Multiplication
------------------------------

Let

.. code:: ipython3

    def halves_doubles(n, m):
        halving = n
        doubling = m
        acc = 0
        while halving:
            digit = halving % 2 
            acc = acc + digit * doubling
            yield (digit, halving, doubling, acc)
            halving = halving >> 1
            doubling = doubling << 1

in

.. code:: ipython3

    list(halves_doubles(89, 18))




.. parsed-literal::

    [(1, 89, 18, 18),
     (0, 44, 36, 18),
     (0, 22, 72, 18),
     (1, 11, 144, 162),
     (1, 5, 288, 450),
     (0, 2, 576, 450),
     (1, 1, 1152, 1602)]



see https://en.wikipedia.org/wiki/Ancient_Egyptian_multiplication and
also
https://www.cut-the-knot.org/Curriculum/Algebra/PeasantMultiplication.shtml.
Then,

.. code:: ipython3

    def rpm(n, m):
        *prefix, (b, h, d, s) = halves_doubles(n, m)
        return s

so the check passes,

.. code:: ipython3

    assert rpm(89, 18) == 89 * 18 == 1602

because

.. code:: ipython3

    bin(89)




.. parsed-literal::

    '0b1011001'



Of course, it works too when the first number is even,

.. code:: ipython3

    rpm(6, 100)




.. parsed-literal::

    600



Of course our implementation

.. code:: ipython3

    %timeit rpm(293819385789379687596845, 921038209831568476843584365)


.. parsed-literal::

    29.3 µs ± 805 ns per loop (mean ± std. dev. of 7 runs, 10000 loops each)


is *slower* than the primitive one

.. code:: ipython3

    %timeit 293819385789379687596845 * 921038209831568476843584365


.. parsed-literal::

    101 ns ± 0.0308 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)


because arithmetic is performed in the virtual machine.

Let us give a strict version also,

.. code:: ipython3

    def rpm_strict(n, m):
        halving = n
        doubling = m
        acc = 0
        while halving:
            digit = halving % 2 
            acc = acc + digit * doubling
            halving = halving >> 1
            doubling = doubling << 1
        return acc

check that it is correct,

.. code:: ipython3

    rpm_strict(89, 18)




.. parsed-literal::

    1602



and observe that it is a little bit *faster* than our former
implementation

.. code:: ipython3

    %timeit rpm_strict(293819385789379687596845, 921038209831568476843584365)


.. parsed-literal::

    22.2 µs ± 52.8 ns per loop (mean ± std. dev. of 7 runs, 10000 loops each)


Fixed sum
---------

.. code:: ipython3

    def subarrays(L):
       return (L[i:j] for i in range(len(L)) for j in range(i, len(L)+1))

.. code:: ipython3

    L = [-1, 5, 8, -9, 4, 1]

.. code:: ipython3

    list(subarrays(L))




.. parsed-literal::

    [[],
     [-1],
     [-1, 5],
     [-1, 5, 8],
     [-1, 5, 8, -9],
     [-1, 5, 8, -9, 4],
     [-1, 5, 8, -9, 4, 1],
     [],
     [5],
     [5, 8],
     [5, 8, -9],
     [5, 8, -9, 4],
     [5, 8, -9, 4, 1],
     [],
     [8],
     [8, -9],
     [8, -9, 4],
     [8, -9, 4, 1],
     [],
     [-9],
     [-9, 4],
     [-9, 4, 1],
     [],
     [4],
     [4, 1],
     [],
     [1]]



.. code:: ipython3

    def fixed_sum(L, n):
        return filter(lambda s: sum(s)==n, subarrays(L))

.. code:: ipython3

    list(fixed_sum(L, 10))




.. parsed-literal::

    []



.. code:: ipython3

    def partial_sums(L):
        g = itertools.accumulate(subarrays(L), lambda s, each: s + each[-1] if each else 0, initial=0)
        next(g) # to ignore the initial 0 given above
        return g

.. code:: ipython3

    list(partial_sums(L))




.. parsed-literal::

    [0,
     -1,
     4,
     12,
     3,
     7,
     8,
     0,
     5,
     13,
     4,
     8,
     9,
     0,
     8,
     -1,
     3,
     4,
     0,
     -9,
     -5,
     -4,
     0,
     4,
     5,
     0,
     1]



Toward an optimization…

.. code:: ipython3

    def subarrays_rev(L):
       return (tuple(L[i:j]) for i in range(len(L)-1, -1, -1) for j in range(i+1, len(L)+1))

.. code:: ipython3

    list(subarrays_rev(L))




.. parsed-literal::

    [(1,),
     (4,),
     (4, 1),
     (-9,),
     (-9, 4),
     (-9, 4, 1),
     (8,),
     (8, -9),
     (8, -9, 4),
     (8, -9, 4, 1),
     (5,),
     (5, 8),
     (5, 8, -9),
     (5, 8, -9, 4),
     (5, 8, -9, 4, 1),
     (-1,),
     (-1, 5),
     (-1, 5, 8),
     (-1, 5, 8, -9),
     (-1, 5, 8, -9, 4),
     (-1, 5, 8, -9, 4, 1)]



.. code:: ipython3

    def fixed_sum_rev(L, n, cache={}):
        for tup in subarrays_rev(L):
            rest = tup[1:]
            s = tup[0] + cache.get(rest, 0)
            cache[tup] = s
            if s == n: yield tup

.. code:: ipython3

    cache = {}
    list(fixed_sum_rev(L, 10, cache))




.. parsed-literal::

    []



.. code:: ipython3

    cache # have a look at the collected values




.. parsed-literal::

    {(1,): 1,
     (4,): 4,
     (4, 1): 5,
     (-9,): -9,
     (-9, 4): -5,
     (-9, 4, 1): -4,
     (8,): 8,
     (8, -9): -1,
     (8, -9, 4): 3,
     (8, -9, 4, 1): 4,
     (5,): 5,
     (5, 8): 13,
     (5, 8, -9): 4,
     (5, 8, -9, 4): 8,
     (5, 8, -9, 4, 1): 9,
     (-1,): -1,
     (-1, 5): 4,
     (-1, 5, 8): 12,
     (-1, 5, 8, -9): 3,
     (-1, 5, 8, -9, 4): 7,
     (-1, 5, 8, -9, 4, 1): 8}



.. code:: ipython3

    def sample(n):
        O, b, *rest = bin(random.getrandbits(n)) # because `string`s are iterable objects indeed.
        return list(map(int, rest))

where

.. code:: ipython3

    help(random.getrandbits)


.. parsed-literal::

    Help on built-in function getrandbits:
    
    getrandbits(k, /) method of random.Random instance
        getrandbits(k) -> x.  Generates an int with k random bits.
    


.. code:: ipython3

    LL = sample(1000)

.. code:: ipython3

    assert set(map(tuple, fixed_sum(LL, 10))) == set(fixed_sum_rev(LL, 10))

.. code:: ipython3

    %timeit list(fixed_sum(LL, 10))


.. parsed-literal::

    2.38 s ± 46.2 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)


.. code:: ipython3

    %timeit list(fixed_sum_rev(LL, 10))


.. parsed-literal::

    5.91 s ± 3.82 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

