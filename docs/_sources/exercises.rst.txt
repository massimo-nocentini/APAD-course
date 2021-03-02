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

    <generator object take at 0x7fc40bf47e40>



is a actually generator and its content equals

.. code:: ipython3

    assert list(taken) == list(range(50))

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

    @functools.cache
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

    [14.099613130242322,
     15.042430039294727,
     15.054925822215415,
     15.400719904549653,
     15.714577581986779,
     16.09454750184962,
     16.169172163612973,
     16.672057492150884,
     16.711732516830907,
     16.803542016379748,
     17.175014561251693,
     17.240811855776695,
     17.772625718884473,
     17.987971331377178,
     18.19395704461553,
     18.42354783087974,
     18.667352819315536,
     18.682055196236355,
     18.74217199872048,
     18.816441065824897,
     19.083221061479573,
     19.183024972873685,
     19.209936295249342,
     19.43480148067072,
     19.61117446876884,
     19.627303300465165,
     19.923738316438143,
     35.12305214548047,
     35.18142206774488,
     35.18411858785431,
     35.22664081632368,
     35.27251614283628,
     35.40904435345992,
     35.43912467552985,
     35.68429940080377,
     35.71888190108937,
     35.7271442388945,
     35.75783263239378,
     35.872470640844284,
     35.89195950774178,
     35.980757049799315,
     35.981775604695045,
     35.98434046759749,
     35.98473482345873,
     36.07737189602656,
     36.11351094158937,
     36.11952812083598,
     36.157499696186704,
     36.180553828747875,
     36.18802609066498,
     36.22657655241199,
     36.243473589718356,
     36.46865347170687,
     36.805659562487676,
     36.82474577813992,
     37.07773865377199,
     37.0780698604367,
     37.07854758591836,
     37.079515903350924,
     37.36094263623299,
     37.40626697392589,
     37.42153929699421,
     37.48756387147119,
     37.491016452646676,
     37.55697665858069,
     37.58872841059282,
     37.62717861617942,
     37.679355622177994,
     37.70489687642647,
     37.7481581145108,
     37.88358813292845,
     37.88770055968942,
     37.91267386834141,
     37.93199215717868,
     37.95408410704644,
     38.056929861397975,
     38.095721330360234,
     38.13602604283887,
     38.15292939276711,
     38.25067211387302,
     38.39266083030915,
     38.464135746965944,
     38.49457858166925,
     38.590996465501185,
     38.65329345037358,
     38.75102574605713,
     38.77274430938016,
     38.819249082159075,
     38.84201684521456,
     39.10687878541622,
     39.127167753676034,
     39.24073266998679,
     39.24213187134309,
     39.25258447458132,
     39.30314518065818,
     39.52161609334226,
     39.638322973070714,
     39.70200203063223,
     39.70925513471737,
     39.922903856012475]



and check the corresponding quantiles

.. code:: ipython3

    statistics.quantiles(observations)




.. parsed-literal::

    [19.02225548120831, 36.43524239264308, 38.22073926901422]



it should be uniform too. Finally, we test the corner case where
:math:`b=c`, so let :math:`[10, 20)` and :math:`[20,40)`,

.. code:: ipython3

    observations = take(samples(slice(10, 20), slice(20, 40)), 1000000)

look again at some observations,

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. parsed-literal::

    [10.29443967255584,
     10.389582851361535,
     10.751803056983524,
     11.292356727115278,
     11.388428251836682,
     11.461856587348677,
     11.596674101681474,
     11.77511398954632,
     12.01569935558677,
     12.222155549127304,
     12.279085221483117,
     12.301425945566313,
     12.921546513858807,
     12.959893801770384,
     14.156887631621961,
     14.711457129780037,
     14.736947433880584,
     14.774414405894628,
     15.030943610162508,
     15.092833805259025,
     15.148548419843436,
     15.162966656289903,
     15.272230478182166,
     15.630515301172018,
     15.67229149723666,
     15.810394189658044,
     15.94338711287187,
     16.284960496589807,
     16.45543016468218,
     16.47371438529651,
     16.911799312375187,
     17.03975152472178,
     17.170833892131427,
     17.25436778360172,
     17.32744960946101,
     17.404337721732265,
     17.444921313078254,
     17.45413381549554,
     17.989541837428828,
     18.085746826185847,
     18.332269054958687,
     18.584995913258552,
     18.990264761683168,
     19.098687838804537,
     19.311621011420634,
     19.47334051653072,
     19.70317513159338,
     19.8588492840531,
     19.97872576227502,
     20.11767716639163,
     20.150592220732868,
     20.22751839644842,
     20.581460757660274,
     20.853682832398054,
     21.19475550948293,
     21.450044132391973,
     21.617152507972346,
     22.13952962160949,
     22.168250775920416,
     22.26095582669094,
     22.950246737152845,
     23.530518187290554,
     23.74954640393728,
     25.181588875922955,
     25.44095626751403,
     25.744905205209303,
     26.263245724832935,
     26.77866675200537,
     26.85227195075615,
     27.217436442346212,
     28.003210378672065,
     28.225755967675465,
     28.74801620415561,
     28.840033458738166,
     28.947601640541233,
     29.23424321009936,
     29.267543341480714,
     29.655389146378738,
     29.864917396264605,
     30.13383841405905,
     30.695480992481286,
     30.70666981185014,
     31.70997411709306,
     32.47493817587067,
     32.61592854003962,
     32.822684380690546,
     33.02611771273637,
     33.38591261644087,
     33.53334749587938,
     33.69155999551302,
     34.0572572820421,
     34.178303357761685,
     34.53318255967711,
     36.08847203528956,
     36.47311476852358,
     37.29241949096567,
     37.41084139391073,
     39.32355513566131,
     39.790964180290636,
     39.85414323920364]



and check the corresponding quantiles

.. code:: ipython3

    statistics.quantiles(observations)




.. parsed-literal::

    [14.990949386533936, 19.981113933657763, 30.00265344224226]



it should be uniform either.
