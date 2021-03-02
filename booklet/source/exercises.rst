Exercises
=========

This chapter is an intermezzo that allows us to check and have a deeper
understanding of the concepts seen so far by means of exercises. We will
see how the code shown can be rewritten to take advantage of
battle-tested solutions and idioms that emerges from daily practice.

First of all, we import some modules

.. code:: ipython3

    import functools, operator, math, itertools, random, collections, statistics

that contains useful definitions for the code that we are going to
write. Moreover, an utility for generators:

.. code:: ipython3

    def take(iterable, n):
        yield from map(lambda p: p[1], zip(range(n), iterable))

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
    list(zip(range(15), sqrt(37)))




.. parsed-literal::

    [(0, 37),
     (1, 18.5),
     (2, 10.25),
     (3, 6.929878048780488),
     (4, 6.134538672432479),
     (5, 6.082981028300877),
     (6, 6.082762534222396),
     (7, 6.08276253029822),
     (8, 6.08276253029822),
     (9, 6.08276253029822),
     (10, 6.08276253029822),
     (11, 6.08276253029822),
     (12, 6.08276253029822),
     (13, 6.08276253029822),
     (14, 6.08276253029822)]



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



Fibonacci numbers
-----------------

Directly from
https://docs.python.org/3/library/functools.html#functools.cache:

.. code:: ipython3

    @functools.cache
    def factorial(n):
        return n * factorial(n-1) if n else 1

no previously cached result, makes 11 recursive calls

.. code:: ipython3

    factorial(10)




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

    479001600



Uniform ``random`` on segmented interval
----------------------------------------

The problem here reads as follow:

   Sample uniformly from :math:`[a, b)` and :math:`[c, d)` where
   :math:`b <= c`.

.. code:: ipython3

    random.seed(11)

.. code:: ipython3

    help(random.random)


.. parsed-literal::

    Help on built-in function random:
    
    random() method of random.Random instance
        random() -> x in the interval [0, 1).
    


.. code:: ipython3

    def samples(firstSlice, secondSlice):
        secondLength = secondSlice.stop - secondSlice.start
        ratio = secondLength/firstSlice.start
        while True:
            sample = random.random()*firstSlice.stop
            yield (sample*ratio + secondSlice.start) if sample < firstSlice.start else sample

Then define the generator with respect to :math:`[10, 20)` and
:math:`[35, 40)`

.. code:: ipython3

    observations = take(samples(slice(10, 20), slice(35, 40)), 1000000)

have a look at some observations

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. parsed-literal::

    [10.156825461245422,
     10.23817278083611,
     10.382482295281534,
     10.567625323409576,
     11.185902832207898,
     11.19544772160992,
     11.65469596335104,
     11.747696576997939,
     12.025676871638776,
     12.311254091571415,
     12.589098244680484,
     12.597654404336039,
     12.805834158383542,
     12.83671313180921,
     12.874302474223342,
     13.078450670676808,
     13.248990637807363,
     13.868769650824781,
     14.156192429958988,
     14.637884314287007,
     15.160810339875177,
     15.32575772820814,
     15.453793795456463,
     15.570217173533017,
     15.859537450399053,
     15.940430236879482,
     16.06823006164143,
     16.19289068734355,
     16.804310989857235,
     16.84854257037064,
     16.931672437623572,
     16.94619546605609,
     17.08753803402461,
     17.697302254979416,
     18.20543856208363,
     18.484211680474587,
     19.160847666396272,
     19.281524331875282,
     19.295155622511334,
     19.41858112437383,
     19.607178823485842,
     19.643868415975565,
     19.786033950203446,
     19.913832833123983,
     19.953124009261686,
     35.0054493705557,
     35.15000736949605,
     35.300825892247886,
     35.41880336369846,
     35.59551105168855,
     35.59893402941077,
     35.70223499559926,
     35.73038343833698,
     35.733985533884244,
     35.8714419835217,
     35.9011717296193,
     35.906705374918396,
     35.94123456229219,
     36.00332752183883,
     36.010463089567054,
     36.16507987639706,
     36.179916794195506,
     36.309670837166195,
     36.57494095140162,
     36.77678128198366,
     36.84660343854877,
     36.90208262797929,
     36.90684415290366,
     37.09717414729611,
     37.107102214344394,
     37.128656730090896,
     37.132433685775226,
     37.296659016079055,
     37.419430136652146,
     37.43012920137562,
     37.4638794889313,
     37.58277557861621,
     37.697755868501424,
     37.69794977190598,
     37.78162899663886,
     37.89039947314202,
     37.96324762598947,
     38.03401262624526,
     38.0414514987099,
     38.1527721701555,
     38.2895542554773,
     38.32585625463348,
     38.86513531705935,
     38.94274637072054,
     38.97424388079281,
     39.00399804918491,
     39.20755617246421,
     39.40531116656657,
     39.47424877501022,
     39.52379553509819,
     39.57329881599558,
     39.63934461223285,
     39.65650070099773,
     39.699872760136664,
     39.997731522067916]



then observe the quantiles:

.. code:: ipython3

    statistics.quantiles(observations)




.. parsed-literal::

    [15.006802680976536, 35.007462054114015, 37.49996307800869]



it looks uniform. By the way, use different intervals, :math:`[14, 20)`
and :math:`[35,40)`,

.. code:: ipython3

    observations = take(samples(slice(14, 20), slice(35, 40)), 1000000)

look again at some observations,

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. parsed-literal::

    [14.283075034128789,
     14.344642981348786,
     14.688865709760112,
     14.759034017699605,
     14.778980951740772,
     14.793336761503678,
     15.137587805213371,
     15.25173408596455,
     15.317456657632407,
     15.341221318874577,
     15.366608784584269,
     15.75644829436862,
     16.245383093079482,
     16.298216975457503,
     17.20207768834477,
     17.456016745025092,
     17.509371560552218,
     17.705594468497303,
     17.731899026048854,
     17.78843669826197,
     18.0521470430377,
     18.177426018715767,
     18.73689014147333,
     19.189272936744505,
     19.593241906411937,
     19.703938383770225,
     19.8507928528412,
     19.887969110474742,
     35.008702180712774,
     35.031213404638414,
     35.07897380159041,
     35.21760289840942,
     35.223380744668454,
     35.26193861838462,
     35.294145330270894,
     35.31170199505849,
     35.449376551236426,
     35.60144001605446,
     35.64760723966421,
     35.68577876398881,
     35.91400870547509,
     35.929374327392374,
     35.932498216020036,
     36.13186191840736,
     36.134474531367275,
     36.18535713925314,
     36.25646869520796,
     36.303542756492355,
     36.40246365192152,
     36.41034136482392,
     36.45668027028991,
     36.460897463798574,
     36.59670352660416,
     36.81583242104361,
     36.92723359120805,
     36.99290510212475,
     37.048300531609044,
     37.0592163372416,
     37.10366397950206,
     37.18007684151863,
     37.33372859458535,
     37.33945749070277,
     37.49028201577179,
     37.58480891368881,
     37.644628469866184,
     37.652988942869214,
     37.761856125620014,
     37.93886042643186,
     38.051323784670906,
     38.12518302320556,
     38.175634454937644,
     38.17747324721339,
     38.240341489144406,
     38.30600639027821,
     38.3470597325234,
     38.4043709943189,
     38.58627660256967,
     38.73974841472965,
     38.75550750411811,
     38.772494369032756,
     38.874746439594524,
     38.88212438125607,
     38.992503274233236,
     39.099681803499095,
     39.19450496377118,
     39.26942730030338,
     39.27131074779202,
     39.314045810968814,
     39.376495789397254,
     39.46029988689277,
     39.60975806003205,
     39.614262421948176,
     39.63613277517809,
     39.659650884019506,
     39.807355479464334,
     39.845120013649435,
     39.87445190085072,
     39.893502719517286,
     39.905428805740264,
     39.97724585792723]



and check the corresponding quantiles

.. code:: ipython3

    statistics.quantiles(observations)




.. parsed-literal::

    [19.00663637522749, 36.4308193958336, 38.21339522407641]



it should be uniform too.
