Exercises
=========

.. code:: ipython3

    import functools, operator, math, itertools

--------------

Let

.. code:: ipython3

    def sumUpto(n):
        return functools.reduce(operator.add, range(n+1))

in

.. code:: ipython3

    n = 10
    assert sumUpto(n) == (n*(n+1)/2)

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
    


--------------

Let

.. code:: ipython3

    def sqrt(n):
        
        yield n
        refined = n/2
        d = n - refined
        while d:
            yield refined
            d = refined
            refined = (n/refined + refined)/2
            d = refined - d

in, by ensuring at most :math:`30` refinements,

.. code:: ipython3

    n = 37

.. code:: ipython3

    list(zip(range(30), sqrt(37)))




.. parsed-literal::

    [(0, 37),
     (1, 18.5),
     (2, 10.25),
     (3, 6.929878048780488),
     (4, 6.134538672432479),
     (5, 6.082981028300877),
     (6, 6.082762534222396),
     (7, 6.08276253029822)]



but it stops before. Check with respect to

.. code:: ipython3

    math.sqrt(n)




.. parsed-literal::

    6.082762530298219



.. code:: ipython3

    help(math.sqrt)


.. parsed-literal::

    Help on built-in function sqrt in module math:
    
    sqrt(x, /)
        Return the square root of x.
    


--------------

.. code:: ipython3

    def pi_Leibniz():
        '''https://en.wikipedia.org/wiki/Leibniz_formula_for_%CF%80'''
        
        d = 0
        for i, coeff in enumerate(itertools.count(1, step=2)):
            yield 4*d
            d += (-1)**i/coeff

.. code:: ipython3

    list(zip(range(1000), pi_Leibniz()))[-10:]




.. parsed-literal::

    [(990, 3.140582552837346),
     (991, 3.1426017350685425),
     (992, 3.140584589329763),
     (993, 3.1425997026798886),
     (994, 3.140586617627045),
     (995, 3.142597678461635),
     (996, 3.1405886377785612),
     (997, 3.1425956623646125),
     (998, 3.140590649833284),
     (999, 3.142593654340044)]



.. code:: ipython3

    math.pi




.. parsed-literal::

    3.141592653589793



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
    


--------------

.. code:: ipython3

    def collatz(n):
        '''The Collatz conjecture.
        
        Consider the following operation on an arbitrary positive integer:
        If the number is even, divide it by two.
        If the number is odd, triple it and add one.
        
        See also https://en.wikipedia.org/wiki/Collatz_conjecture.'''
        
        yield n
        
        while True:
            n = 3*n + 1 if n % 2 else n // 2
            yield n

.. code:: ipython3

    [list(map(lambda p: p[1], zip(range(15), collatz(n)))) for n in range(1, 20)]




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



.. code:: ipython3

    help(collatz)


.. parsed-literal::

    Help on function collatz in module __main__:
    
    collatz(n)
        The Collatz conjecture.
        
        Consider the following operation on an arbitrary positive integer:
        If the number is even, divide it by two.
        If the number is odd, triple it and add one.
        
        See also https://en.wikipedia.org/wiki/Collatz_conjecture.
    

