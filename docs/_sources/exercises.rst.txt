Exercises
=========

This chapter is an intermezzo that allows us to check and have a deeper
understanding of the concepts seen so far by means of exercises. We will
see how the code shown can be rewritten to take advantage of
battle-tested solutions and idioms that emerges from daily practice.

First of all, we import some modules (be free to skim the corresponding
documentation for each one of them),

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

    <generator object take at 0x7f88a402d120>



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

**INTERMEZZO**

.. code:: ipython3

    def A(a, b, c, d):
        pass

.. code:: ipython3

    def A(*args):
        return list(map(lambda i: i + 4, args))

.. code:: ipython3

    def AA(args):
        return list(map(lambda i: i + 4, args))

.. code:: ipython3

    def B(a, b, *args):
        return [a, b] + list(map(lambda i: i + 4, args))

.. code:: ipython3

    A(1, 2, 3)




.. parsed-literal::

    [5, 6, 7]



.. code:: ipython3

    A([1, 2, 3])


::


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-12-ff414da197ec> in <module>
    ----> 1 A([1, 2, 3])
    

    <ipython-input-8-224bcd67b941> in A(*args)
          1 def A(*args):
    ----> 2     return list(map(lambda i: i + 4, args))
    

    <ipython-input-8-224bcd67b941> in <lambda>(i)
          1 def A(*args):
    ----> 2     return list(map(lambda i: i + 4, args))
    

    TypeError: can only concatenate list (not "int") to list


.. code:: ipython3

    AA([1, 2, 3])




.. parsed-literal::

    [5, 6, 7]



.. code:: ipython3

    B(1,)


::


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-14-e0b11ae7ec43> in <module>
    ----> 1 B(1,)
    

    TypeError: B() missing 1 required positional argument: 'b'


.. code:: ipython3

    B(1, 2)




.. parsed-literal::

    [1, 2]



.. code:: ipython3

    B(1, 2, 3)




.. parsed-literal::

    [1, 2, 7]



.. code:: ipython3

    A()




.. parsed-literal::

    []



.. code:: ipython3

    A(1, 2, 3)




.. parsed-literal::

    [5, 6, 7]



.. code:: ipython3

    A(1, 2, 3, 4, 5, 6, 7)




.. parsed-literal::

    [5, 6, 7, 8, 9, 10, 11]



.. code:: ipython3

    container = range(5)
    A( *container  )




.. parsed-literal::

    [4, 5, 6, 7, 8]



--------------

where

.. code:: ipython3

    help(itertools.product)

Consider the application to an empty sequence of ``slide``\ s,

.. code:: ipython3

    units = tuples()
    units




.. parsed-literal::

    <itertools.product at 0x7f888f745600>



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

    <itertools.product at 0x7f888f747580>



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

    <itertools.product at 0x7f888f745500>



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

.. code:: ipython3

    L = [1, 2, 3, 4]
    L[2:2]




.. parsed-literal::

    []



.. code:: ipython3

    L[slice(2, 2)]




.. parsed-literal::

    []



then saturate it

.. code:: ipython3

    list(triples) # who we have to blame?




.. parsed-literal::

    []



Finally, let

.. code:: ipython3

    type(True)




.. parsed-literal::

    bool



.. code:: ipython3

    def is_pythagorean(tup: tuple, n=2) -> bool: # is_pythagorean is a *predicate*
        '''A Pythagorean triple consists of three positive integers a, b, and c, such that a^2 + b^2 = c^2. 
        
        Such a triple is commonly written (a, b, c), and a well-known example is (3, 4, 5). 
        If (a, b, c) is a Pythagorean triple, then so is (ka, kb, kc) for any positive integer k. 
        
        A primitive Pythagorean triple is one in which a, b and c are coprime (that is, 
        they have no common divisor larger than 1).
        
        See also https://en.wikipedia.org/wiki/Pythagorean_triple.
        '''
        a, b, c = tup # tuple unpacking
        return (a**n + b**n == c**n) if a <= b <= c else False

in

.. code:: ipython3

    filter(is_pythagorean, triples_b)




.. parsed-literal::

    <filter at 0x7f888f7bc370>



.. code:: ipython3

    list(filter(is_pythagorean, triples_b)) # do a selection




.. parsed-literal::

    [(5, 12, 13), (6, 8, 10)]



and

.. code:: ipython3

    help(is_pythagorean) # just to show that writing docstrings is cool and useful.


.. parsed-literal::

    Help on function is_pythagorean in module __main__:
    
    is_pythagorean(tup: tuple, n=2) -> bool
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
    gauss = (n*(n+1)/2)
    assert v == gauss == 5050

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
        
        refined = n
        while True:
            yield refined
            refined = (n/refined + refined)/2

to enumerate 15 approximation of the square root of 37

.. code:: ipython3

    n = 37
    list(take(sqrt(37), 15))




.. parsed-literal::

    [37,
     19.0,
     10.473684210526315,
     7.003173763554615,
     6.143246310003449,
     6.08306027903096,
     6.082762537585202,
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
        
        while True:
            
            yield n
            n = 3*n + 1 if n % 2 else n // 2 # be aware that we lose track of the original `n`!

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

    <generator object samples at 0x7f888f74ceb0>



Then define the generator with respect to :math:`[10, 20)` and
:math:`[35, 40)`

.. code:: ipython3

    observations = take(samples(slice(10, 20), slice(35, 40)), 1000000)

have a look at some observations

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. parsed-literal::

    [10.01089874111141,
     10.300014738992099,
     10.601651784495772,
     10.83760672739692,
     11.1910221033771,
     11.197868058821534,
     11.404469991198518,
     11.460766876673958,
     11.467971067768483,
     11.742883967043403,
     11.802343459238605,
     11.813410749836788,
     11.88246912458437,
     12.00665504367766,
     12.0209261791341,
     12.330159752794112,
     12.359833588391016,
     12.619341674332393,
     13.149881902803248,
     13.553562563967322,
     13.693206877097532,
     13.804165255958583,
     13.813688305807322,
     14.194348294592222,
     14.214204428688788,
     14.257313460181788,
     14.264867371550451,
     14.59331803215811,
     14.838860273304295,
     14.860258402751247,
     14.927758977862599,
     15.165551157232409,
     15.395511737002854,
     15.395899543811966,
     15.563257993277716,
     15.780798946284042,
     15.926495251978928,
     16.06802525249051,
     16.08290299741979,
     16.305544340311,
     16.579108510954597,
     16.651712509266957,
     17.73027063411869,
     17.885492741441087,
     17.948487761585625,
     18.007996098369823,
     18.415112344928417,
     18.810622333133136,
     18.94849755002043,
     19.047591070196372,
     19.146597631991153,
     19.27868922446569,
     19.313001401995464,
     19.399745520273328,
     19.995463044135832,
     35.07841273062271,
     35.119086390418055,
     35.19124114764077,
     35.283812661704786,
     35.59295141610395,
     35.59772386080496,
     35.82734798167552,
     35.87384828849897,
     36.01283843581939,
     36.15562704578571,
     36.29454912234024,
     36.29882720216802,
     36.402917079191774,
     36.4183565659046,
     36.43715123711167,
     36.53922533533841,
     36.62449531890368,
     36.934384825412394,
     37.078096214979496,
     37.3189421571435,
     37.58040516993759,
     37.66287886410407,
     37.72689689772823,
     37.78510858676651,
     37.92976872519952,
     37.97021511843974,
     38.034115030820715,
     38.096445343671775,
     38.40215549492862,
     38.42427128518532,
     38.46583621881179,
     38.47309773302804,
     38.5437690170123,
     38.84865112748971,
     39.102719281041814,
     39.24210584023729,
     39.58042383319813,
     39.640762165937645,
     39.647577811255665,
     39.709290562186915,
     39.80358941174292,
     39.82193420798778,
     39.89301697510172,
     39.956916416561995,
     39.97656200463084]



then observe the quantiles:

.. code:: ipython3

    statistics.quantiles(observations)




.. parsed-literal::

    [14.985642471160405, 19.985354416460936, 37.49597915696282]



it looks uniform. By the way, use different intervals, :math:`[14, 20)`
and :math:`[35,40)`,

.. code:: ipython3

    observations = take(samples(slice(14, 20), slice(35, 40)), 1000000)

look again at some observations,

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. parsed-literal::

    [14.014619663597463,
     14.052438519792538,
     14.132675986671885,
     14.365572869327819,
     14.375279651043002,
     14.44005687888616,
     14.494164154855097,
     14.52365935169827,
     14.75495260607719,
     15.010419226971491,
     15.087980162635876,
     15.1521083235012,
     15.535534625198157,
     15.561348870019186,
     15.566597002913658,
     15.901528022924364,
     15.905917212697016,
     15.991399993945283,
     16.110867407949364,
     16.18995183090716,
     16.35613893522816,
     16.36937349290418,
     16.447222854087045,
     16.4543077391816,
     16.682461924694984,
     17.05059846735326,
     17.237752433229527,
     17.34808057156958,
     17.441144893103193,
     17.459483446565894,
     17.53415548556346,
     17.662529093751296,
     17.920664038903396,
     17.930288584380648,
     18.183673786496605,
     18.342478974997196,
     18.44297582937519,
     18.457021424020276,
     18.63991829104162,
     18.93728551640552,
     19.126223958247124,
     19.25030747898533,
     19.33506588429524,
     19.338155055318495,
     19.4437737017626,
     19.5540907356674,
     19.623060350639307,
     19.719343270455752,
     35.02078724359754,
     35.235647780621505,
     35.257710505765345,
     35.28149211664586,
     35.42464501543233,
     35.434974133758494,
     35.58950458392653,
     35.73955452489873,
     35.87230694927965,
     35.977198220424725,
     35.97983504690883,
     36.03966413535634,
     36.127094105156154,
     36.24441984164987,
     36.45366128404488,
     36.45996739072744,
     36.490585885249324,
     36.52351123762731,
     36.73029767125007,
     36.78316801910921,
     36.824232661191004,
     36.850903807324194,
     36.86760032803637,
     36.96814420109811,
     37.14153751706439,
     37.17232149067439,
     37.344432854880054,
     37.3795170088498,
     37.38949047587039,
     37.39666838075184,
     37.568793902606686,
     37.62586704298228,
     37.6587283288162,
     37.67061065943729,
     37.683304392292136,
     37.87822414718431,
     38.12269154653974,
     38.14910848772875,
     38.601038844172386,
     38.72800837251255,
     38.754685780276105,
     38.852797234248655,
     38.865949513024425,
     38.89421834913099,
     39.02607352151885,
     39.08871300935788,
     39.36844507073667,
     39.59463646837225,
     39.79662095320597,
     39.85196919188511,
     39.9253964264206,
     39.94398455523737]



and check the corresponding quantiles

.. code:: ipython3

    statistics.quantiles(observations)




.. parsed-literal::

    [16.99686785974616, 19.995166578121935, 37.49720072901549]



it should be uniform too. Finally, we test the corner case where
:math:`b=c`, so let :math:`[10, 20)` and :math:`[20,40)`,

.. code:: ipython3

    observations = take(samples(slice(10, 20), slice(20, 40)), 1000000)

look again at some observations,

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. parsed-literal::

    [10.220506993337498,
     10.260587558652016,
     10.484692070122366,
     10.62014082107819,
     10.716907216811817,
     11.059578641204899,
     11.32138100417417,
     11.593818846366288,
     11.642898373028407,
     11.835380635908852,
     12.07497873143257,
     12.311092872785627,
     12.372471556874899,
     12.528741487909699,
     12.533954399994427,
     12.698873438533695,
     13.176279836528684,
     13.586525086627985,
     13.944379662183836,
     13.959184739901707,
     13.981153287065515,
     14.000895353339368,
     14.058731861028168,
     14.06078869093613,
     14.32800749089486,
     14.588117887042511,
     14.875747659228065,
     15.257663006055465,
     15.650659914540373,
     15.847275824883377,
     15.870874283684405,
     15.88897040706754,
     16.036639838802042,
     16.05278288507251,
     16.07164428762117,
     16.289567316081033,
     16.683463413688443,
     16.888605068446786,
     16.972846407265372,
     17.050176440112484,
     17.258915139395995,
     17.371893542101574,
     17.43299587297839,
     17.51624465788484,
     17.735319262220383,
     17.84305048818223,
     18.350336100137756,
     18.64892714276403,
     18.66760390953786,
     18.94398065178452,
     19.300495933227964,
     20.12528469847449,
     20.46142735180286,
     20.86206580281022,
     20.874500574482518,
     21.750626829185087,
     22.14390267598317,
     22.75002173984211,
     23.03725623526333,
     23.041720287639468,
     23.429201186117467,
     23.585585621517072,
     23.86788816158681,
     24.39871179038679,
     24.795985333846097,
     25.279989200242422,
     26.191697703699198,
     26.44319296558182,
     26.64426643575196,
     27.479813164813667,
     28.05941553643912,
     28.26350413240277,
     29.581648917089623,
     29.61444785911453,
     30.570688568364677,
     31.140643196178605,
     31.25175270850296,
     31.343246396675582,
     31.465988526059693,
     31.977260863350544,
     32.04815155247824,
     32.68910230586178,
     33.079261228352046,
     34.016272659494696,
     34.1427648058461,
     34.389409078180506,
     36.75237185013329,
     37.10573818453108,
     37.12639843651299,
     37.181866718717146,
     37.51981144842088,
     37.70913241796731,
     37.717163298513995,
     37.97101359549681,
     38.06407228243324,
     38.76122108931287,
     39.000838867618455,
     39.65637566563923,
     39.74648436772389,
     39.894486209722814]



and check the corresponding quantiles

.. code:: ipython3

    statistics.quantiles(observations)




.. parsed-literal::

    [15.020045145603692, 20.03656438037431, 30.019022307929152]



it should be uniform either. Finally, attempt a sampling from ``4``
slices,

.. code:: ipython3

    observations = take(samples(slice(0, 5), slice(10, 15), slice(20, 25), slice(30, 35)), 1000000)

look again at some observations,

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. parsed-literal::

    [0.31380424920689043,
     0.46171329548710505,
     0.5809908902109329,
     0.6010442620481715,
     0.6887497588823455,
     1.7254181853171602,
     1.8271470660681088,
     2.0481029382989546,
     2.2471391668540175,
     2.2549353500678992,
     3.291409198290818,
     3.4459073192196366,
     3.647656459473969,
     3.691047155812428,
     4.123566779005962,
     4.25518948312511,
     4.593942703385368,
     4.623470344617491,
     4.927910999580658,
     4.997354137841135,
     10.07911660703169,
     10.254978796163634,
     10.490485665792912,
     11.054516405276523,
     11.211737952532603,
     11.45623976041808,
     11.60260157794309,
     11.708890288498583,
     11.710077039622028,
     11.799231504229418,
     12.03219517748649,
     12.035293496546648,
     12.105031263186536,
     12.15323736571159,
     12.268230511881487,
     12.65993734676487,
     12.737092627327328,
     12.923510931274123,
     13.008737081309608,
     13.327399174343233,
     13.453592676932015,
     13.866526144133722,
     14.123455082492187,
     14.208087607316337,
     14.577220917544247,
     14.668265482140114,
     20.16622603379456,
     20.196788986994257,
     20.42486357873761,
     20.56683377208248,
     21.22079055657177,
     21.69996805446689,
     21.88577771911336,
     21.962891601792883,
     22.03882568164739,
     22.099486163058046,
     22.198700736742005,
     22.35490569770986,
     22.408535739641955,
     22.464447861807745,
     23.11407434829636,
     23.20443932438646,
     23.360841985036092,
     23.416465699558955,
     23.426707076092853,
     23.460607292367335,
     23.536825387311424,
     23.682155329333984,
     23.789000983997582,
     23.80205677267765,
     24.33462907704822,
     24.61305134877459,
     24.947139080703636,
     24.971709988499857,
     30.276125897672962,
     30.281484376382416,
     30.306901588912826,
     30.817452469116336,
     31.013070224830447,
     31.184367702548442,
     31.759194550093298,
     31.834065783582197,
     31.873836952359216,
     32.01770532574806,
     32.06845448192055,
     32.41331640659681,
     32.446910237066085,
     32.52552975176491,
     32.64456279830529,
     32.653192173300894,
     32.80044163780898,
     33.308853046448334,
     33.4209022620311,
     33.6355041822751,
     33.911075150737,
     34.0701398671635,
     34.294102899388164,
     34.41537711418714,
     34.91263601614765,
     34.96005689510007]



and check the corresponding quantiles

.. code:: ipython3

    statistics.quantiles(observations)




.. parsed-literal::

    [10.001575037417881, 20.000883532976324, 30.010509834567223]



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
            r = random.random()  # get a sample
            yield int(r < p)     # if that sample denotes a success or a failure we *yield* that outcome

.. code:: ipython3

    B = Bernoulli(p=0.6) # B is our random variable
    B




.. parsed-literal::

    <generator object Bernoulli at 0x7f888f769660>



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

    [1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 0, 0, 0, 0, 0]



.. code:: ipython3

    C = collections.Counter(take(B, 1_000_000))
    C




.. parsed-literal::

    Counter({1: 599560, 0: 400440})



.. code:: ipython3

    C[1]/(C[0]+C[1])




.. parsed-literal::

    0.59956



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

    30 µs ± 178 ns per loop (mean ± std. dev. of 7 runs, 10000 loops each)


is *slower* than the primitive one

.. code:: ipython3

    %timeit 293819385789379687596845 * 921038209831568476843584365


.. parsed-literal::

    96.9 ns ± 0.415 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)


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

    22.9 µs ± 62.6 ns per loop (mean ± std. dev. of 7 runs, 10000 loops each)


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

    2.64 s ± 4.93 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)


.. code:: ipython3

    %timeit list(fixed_sum_rev(LL, 10))


.. parsed-literal::

    5.14 s ± 4.72 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)


**INTERMEZZO**

.. code:: ipython3

    if 4 < 8:
        print('a')
    else:
        pass


.. parsed-literal::

    a


.. code:: ipython3

    b = if 4 < 8:
           '''
           
           
           lots of code
           
           
           
           
           '''
        else:
           6


::


      File "<tokenize>", line 11
        else:
        ^
    IndentationError: unindent does not match any outer indentation level



.. code:: ipython3

    b = 5 if 4 < 8 else 6

.. code:: ipython3

    b




.. parsed-literal::

    5


