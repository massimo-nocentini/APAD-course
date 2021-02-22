Generators
##########


The Python language documentation
=================================

Start at https://docs.python.org/3/index.html for everything.

For the tutorial have a look at
https://docs.python.org/3/tutorial/index.html, nicely written and
complete.

.. code:: ipython3

    def increment(a):
        return a + 1

.. code:: ipython3

    increment(0)




.. parsed-literal::

    1



.. code:: ipython3

    increment(1)




.. parsed-literal::

    2



.. code:: ipython3

    L = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    L




.. parsed-literal::

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



.. code:: ipython3

    LL = [increment(a) for a in L]
    LL




.. parsed-literal::

    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]



.. code:: ipython3

    LLL = [increment(a) for a in LL]
    LLL




.. parsed-literal::

    [2, 3, 4, 5, 6, 7, 8, 9, 10, 11]



.. code:: ipython3

    r = range(10)
    r




.. parsed-literal::

    range(0, 10)



.. code:: ipython3

    list(r)




.. parsed-literal::

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



.. code:: ipython3

    map(lambda i: i + 1, L)




.. parsed-literal::

    <map at 0x7f0590b631c0>



.. code:: ipython3

    (lambda i: i + 1)(0)




.. parsed-literal::

    1



.. code:: ipython3

    (lambda i: i + 1)(1)




.. parsed-literal::

    2



.. code:: ipython3

    list(map(lambda i: i + 1, L))




.. parsed-literal::

    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]



.. code:: ipython3

    M = map(lambda i: i + 1, L)
    M




.. parsed-literal::

    <map at 0x7f0590b630d0>



.. code:: ipython3

    next(M)




.. parsed-literal::

    1



.. code:: ipython3

    next(M)




.. parsed-literal::

    2



.. code:: ipython3

    next(M)




.. parsed-literal::

    3



.. code:: ipython3

    next(M)




.. parsed-literal::

    4



.. code:: ipython3

    next(M)




.. parsed-literal::

    5



.. code:: ipython3

    next(M)




.. parsed-literal::

    6



.. code:: ipython3

    next(M)




.. parsed-literal::

    7



.. code:: ipython3

    next(M)




.. parsed-literal::

    8



.. code:: ipython3

    next(M)




.. parsed-literal::

    9



.. code:: ipython3

    next(M)




.. parsed-literal::

    10



.. code:: ipython3

    next(M)


::


    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    <ipython-input-96-0666361e9047> in <module>
    ----> 1 next(M)
    

    StopIteration: 


.. code:: ipython3

    list(range(10))




.. parsed-literal::

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



.. code:: ipython3

    list(i for i in range(10))




.. parsed-literal::

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



.. code:: ipython3

    N = (i for i in range(10))
    N




.. parsed-literal::

    <generator object <genexpr> at 0x7f0568ef1040>



.. code:: ipython3

    list(N)




.. parsed-literal::

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



.. code:: ipython3

    next(N)


::


    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    <ipython-input-103-0f4723c98d79> in <module>
    ----> 1 next(N)
    

    StopIteration: 


we want to build an object that denotes a Bernoulli random variable.

it is desired to be able to sample from that variable an arbitrary
number of times, not known at design time.

.. code:: ipython3

    from random import random # import the random generator, to be used to sample from the uniform distribution

.. code:: ipython3

    random() # a quick check that the random function works




.. parsed-literal::

    0.03068991525009368



.. code:: ipython3

    int(True) # this is a very quick check to see if a Boolean can be used as integer




.. parsed-literal::

    1



.. code:: ipython3

    def Bernoulli(p):
        'This is a generator for a Bernoulli random variable of parameter `p` for success.'
        
        while True:              # forever we loop
            r = random()         # get a sample
            yield int(r <= p)    # if that sample denotes a success or a failure we *yield* that outcome

.. code:: ipython3

    yield # if we evaluate *yield* not in a context, Python raises an error because it is a construct


::


      File "<ipython-input-112-f8373fab0a85>", line 1
        yield # if we evaluate *yield* not in a context, Python raises an error because it is a construct
        ^
    SyntaxError: 'yield' outside function



.. code:: ipython3

    help(Bernoulli)


.. parsed-literal::

    Help on function Bernoulli in module __main__:
    
    Bernoulli(p)
        This is a generator for a Bernoulli random variable of parameter `p` for success.
    


.. code:: ipython3

    B = Bernoulli(p=0.6) # B is our random variable
    B




.. parsed-literal::

    <generator object Bernoulli at 0x7f0568d4cf20>



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

    sample = [next(B) for _ in range(1000)]
    sample[:20] # just for a quick evaluation, we print the first 20 elements




.. parsed-literal::

    [0, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0]



.. code:: ipython3

    from collections import Counter

.. code:: ipython3

    Counter(sample)




.. parsed-literal::

    Counter({0: 421, 1: 579})



.. code:: ipython3

    B_flip = map(lambda o: 1-o, B)
    B_flip




.. parsed-literal::

    <map at 0x7f0568efe040>



.. code:: ipython3

    sample = [next(B_flip) for _ in range(1000)]
    sample[:20] # just for a quick evaluation, we print the first 20 elements




.. parsed-literal::

    [0, 1, 1, 1, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0]



.. code:: ipython3

    def Bernoulli(p):
        'This is a generator for a Bernoulli random variable of parameter `p` for success.'
        
        while True:              # forever we loop
            r = random()         # get a sample
            o = int(r <= p)      # if that sample denotes a success or a failure we *yield* that outcome
            print('B ' + str(o))
            yield o

.. code:: ipython3

    def flip(o):
        print('flip')
        return 1-o

.. code:: ipython3

    B_flip = map(flip, Bernoulli(p=0.9))
    B_flip




.. parsed-literal::

    <map at 0x7f0568ef89a0>



.. code:: ipython3

    sample = [next(B_flip) for _ in range(20)]
    Counter(sample)


.. parsed-literal::

    B 0
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 0
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip
    B 1
    flip




.. parsed-literal::

    Counter({1: 2, 0: 18})



.. code:: ipython3

    class A(object):
        
        def __init__(self, j):
            self.j = j
        
        def __add__(self, i):
            return self.j + i
        
        def __radd__(self, i):
            return self.j + i
        
        def __lt__(self, i):
            return self.j < i

.. code:: ipython3

    def B(b):
        pass

.. code:: ipython3

    B




.. parsed-literal::

    <function __main__.B(b)>



.. code:: ipython3

    B(3) is None




.. parsed-literal::

    True



.. code:: ipython3

    def B(b):
        ...

.. code:: ipython3

    increment(4)




.. parsed-literal::

    5



.. code:: ipython3

    a = A()
    increment(a)


::


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-143-a46ef281d6c6> in <module>
          1 a = A()
    ----> 2 increment(a)
    

    <ipython-input-69-d2b3a35173e0> in increment(a)
          1 def increment(a):
    ----> 2     return a + 1
    

    TypeError: unsupported operand type(s) for +: 'A' and 'int'


.. code:: ipython3

    a = A()
    increment(a)




.. parsed-literal::

    2



.. code:: ipython3

    A(3) + 1




.. parsed-literal::

    4



.. code:: ipython3

    1 + A(3)


::


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-148-087a68ad802d> in <module>
    ----> 1 1 + A(3)
    

    TypeError: unsupported operand type(s) for +: 'int' and 'A'


.. code:: ipython3

    1 + A(3)




.. parsed-literal::

    4



.. code:: ipython3

    A(4) < 2




.. parsed-literal::

    False


