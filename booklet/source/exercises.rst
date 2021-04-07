Exercises
=========

This chapter is an intermezzo that allows us to check and have a deeper
understanding of the concepts seen so far by means of exercises. We will
see how the code shown can be rewritten to take advantage of
battle-tested solutions and idioms that emerges from daily practice.

First of all, we import some modules (be free to skim the corresponding
documentation for each one of them),

.. code:: ipython3

    import functools, operator, math, itertools, random, collections, statistics, bisect, operator, heapq

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

    <generator object take at 0x7f94b2884f90>



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

.. code:: ipython3

    A([1, 2, 3])

.. code:: ipython3

    AA([1, 2, 3])

.. code:: ipython3

    B(1,)

.. code:: ipython3

    B(1, 2)

.. code:: ipython3

    B(1, 2, 3)

.. code:: ipython3

    A()

.. code:: ipython3

    A(1, 2, 3)

.. code:: ipython3

    A(1, 2, 3, 4, 5, 6, 7)

.. code:: ipython3

    container = range(5)
    A( *container  )

--------------

where

.. code:: ipython3

    help(itertools.product)

Consider the application to an empty sequence of ``slide``\ s,

.. code:: ipython3

    units = tuples()
    units

then saturate it

.. code:: ipython3

    list(units)

Now, build tuples using just a ``slide`` object,

.. code:: ipython3

    singletons = tuples(slice(5, 11))
    singletons

then saturate it

.. code:: ipython3

    list(singletons)

Now, build tuples using a twin ``slide`` object,

.. code:: ipython3

    s = slice(5, 11)
    pairs = tuples(s, s)
    pairs

then saturate it

.. code:: ipython3

    list(pairs)

Now, build tuples using a three different ``slide`` objects (taking into
account of splitting the returned generator),

.. code:: ipython3

    triples_a, triples_b = itertools.tee(tuples(slice(5, 11), slice(6, 13), slice(7, 14)))

where

.. code:: ipython3

    help(itertools.tee)

then saturate it

.. code:: ipython3

    list(triples_a)

Now a corner case, but still interesting for ensuring a sound behavior,

.. code:: ipython3

    triples = tuples(slice(5, 11), slice(6, 6), slice(7, 14)) # ouch!

.. code:: ipython3

    L = [1, 2, 3, 4]
    L[2:2]

.. code:: ipython3

    L[slice(2, 2)]

then saturate it

.. code:: ipython3

    list(triples) # who we have to blame?

Finally, let

.. code:: ipython3

    type(True)

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

.. code:: ipython3

    list(filter(is_pythagorean, triples_b)) # do a selection

and

.. code:: ipython3

    help(is_pythagorean) # just to show that writing docstrings is cool and useful.

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

and

.. code:: ipython3

    help(operator.add)

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




.. math::

    \displaystyle \left[ 37, \  19.0, \  10.4736842105263, \  7.00317376355462, \  6.14324631000345, \  6.08306027903096, \  6.0827625375852, \  6.08276253029822, \  6.08276253029822, \  6.08276253029822, \  6.08276253029822, \  6.08276253029822, \  6.08276253029822, \  6.08276253029822, \  6.08276253029822\right]



and check with respect to

.. code:: ipython3

    math.sqrt(n)




.. math::

    \displaystyle 6.08276253029822



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




.. math::

    \displaystyle \left[ 3.14058255283735, \  3.14260173506854, \  3.14058458932976, \  3.14259970267989, \  3.14058661762704, \  3.14259767846164, \  3.14058863777856, \  3.14259566236461, \  3.14059064983328, \  3.14259365434004\right]



and check against the

.. code:: ipython3

    math.pi




.. math::

    \displaystyle 3.14159265358979



where

.. code:: ipython3

    help(itertools.count)

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



.. math::

    \displaystyle 3628800



just looks up cached value result

.. code:: ipython3

    factorial(5)




.. math::

    \displaystyle 120



makes two new recursive calls, the other 10 are cached

.. code:: ipython3

    factorial(12)


.. parsed-literal::

    ••



.. math::

    \displaystyle 479001600



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

    <generator object samples at 0x7f94b236eba0>



Then define the generator with respect to :math:`[10, 20)` and
:math:`[35, 40)`

.. code:: ipython3

    observations = take(samples(slice(10, 20), slice(35, 40)), 1_000_000)
    observations




.. parsed-literal::

    <generator object take at 0x7f94b2336970>



have a look at some observations

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. math::

    \displaystyle \left[ 10.0243661059958, \  10.0873975329876, \  10.2211266444531, \  10.6092881155464, \  10.6254660850717, \  10.7334281314769, \  10.8236069247585, \  10.8727655861638, \  11.258254343462, \  11.6840320449525, \  11.8133002710598, \  11.9201805391687, \  12.5592243753303, \  12.6022481166986, \  12.6109950048561, \  13.1692133715406, \  13.1765286878284, \  13.3189999899088, \  13.5181123465823, \  13.6499197181786, \  13.9268982253803, \  13.948955821507, \  14.0787047568117, \  14.090512898636, \  14.4707698744916, \  15.0843307789221, \  15.3962540553825, \  15.5801342859493, \  15.7352414885053, \  15.7658057442765, \  15.8902591426058, \  16.1042151562522, \  16.534440064839, \  16.5504809739677, \  16.972789644161, \  17.2374649583287, \  17.4049597156253, \  17.4283690400338, \  17.733197151736, \  18.2288091940092, \  18.5437065970785, \  18.7505124649756, \  18.8917764738254, \  18.8969250921975, \  19.0729561696043, \  19.256817892779, \  19.3717672510655, \  19.5322387840929, \  35.0207872435975, \  35.2356477806215, \  35.2577105057653, \  35.2814921166459, \  35.4246450154323, \  35.4349741337585, \  35.5895045839265, \  35.7395545248987, \  35.8723069492796, \  35.9771982204247, \  35.9798350469088, \  36.0396641353563, \  36.1270941051562, \  36.2444198416499, \  36.4536612840449, \  36.4599673907274, \  36.4905858852493, \  36.5235112376273, \  36.7302976712501, \  36.7831680191092, \  36.824232661191, \  36.8509038073242, \  36.8676003280364, \  36.9681442010981, \  37.1415375170644, \  37.1723214906744, \  37.3444328548801, \  37.3795170088498, \  37.3894904758704, \  37.3966683807518, \  37.5687939026067, \  37.6258670429823, \  37.6587283288162, \  37.6706106594373, \  37.6833043922921, \  37.8782241471843, \  38.1226915465397, \  38.1491084877288, \  38.6010388441724, \  38.7280083725125, \  38.7546857802761, \  38.8527972342487, \  38.8659495130244, \  38.894218349131, \  39.0260735215189, \  39.0887130093579, \  39.3684450707367, \  39.5946364683723, \  39.796620953206, \  39.8519691918851, \  39.9253964264206, \  39.9439845552374\right]



then observe the quantiles:

.. code:: ipython3

    statistics.quantiles(observations)




.. math::

    \displaystyle \left[ 14.9947797662436, \  19.9919442968699, \  37.4972007290155\right]



it looks uniform. By the way, use different intervals, :math:`[14, 20)`
and :math:`[35,40)`,

.. code:: ipython3

    observations = take(samples(slice(14, 20), slice(35, 40)), 1_000_000)

look again at some observations,

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. math::

    \displaystyle \left[ 14.1323041960025, \  14.1563525351912, \  14.2908152420734, \  14.3720844926469, \  14.4301443300871, \  14.6357471847229, \  14.7928286025045, \  14.9562913078198, \  14.985739023817, \  15.1012283815453, \  15.2449872388595, \  15.3866557236714, \  15.4234829341249, \  15.5172448927458, \  15.5203726399967, \  15.6193240631202, \  15.9057679019172, \  16.1519150519768, \  16.3666277973103, \  16.375510843941, \  16.3886919722393, \  16.4005372120036, \  16.4352391166169, \  16.4364732145617, \  16.5968044945369, \  16.7528707322255, \  16.9254485955368, \  17.1545978036333, \  17.3903959487242, \  17.50836549493, \  17.5225245702106, \  17.5333822442405, \  17.6219839032812, \  17.6316697310435, \  17.6429865725727, \  17.7737403896486, \  18.0100780482131, \  18.1331630410681, \  18.1837078443592, \  18.2301058640675, \  18.3553490836376, \  18.4231361252609, \  18.459797523787, \  18.5097467947309, \  18.6411915573322, \  18.7058302929093, \  19.0102016600827, \  19.1893562856584, \  19.2005623457227, \  19.3663883910707, \  19.5802975599368, \  35.0313211746186, \  35.1153568379507, \  35.2155164507026, \  35.2186251436206, \  35.4376567072963, \  35.5359756689958, \  35.6875054349605, \  35.7593140588158, \  35.7604300719099, \  35.8573002965294, \  35.8963964053793, \  35.9669720403967, \  36.0996779475967, \  36.1989963334615, \  36.3199973000606, \  36.5479244259248, \  36.6107982413955, \  36.661066608938, \  36.8699532912034, \  37.0148538841098, \  37.0658760331007, \  37.3954122292724, \  37.4036119647786, \  37.6426721420912, \  37.7851607990447, \  37.8129381771257, \  37.8358115991689, \  37.8664971315149, \  37.9943152158376, \  38.0120378881196, \  38.1722755764654, \  38.269815307088, \  38.5040681648737, \  38.5356912014615, \  38.5973522695451, \  39.1880929625333, \  39.2764345461328, \  39.2815996091282, \  39.2954666796793, \  39.3799528621052, \  39.4272831044918, \  39.4292908246285, \  39.4927533988742, \  39.5160180706083, \  39.6903052723282, \  39.7502097169046, \  39.9140939164098, \  39.936621091931, \  39.9736215524307\right]



and check the corresponding quantiles

.. code:: ipython3

    statistics.quantiles(observations)




.. math::

    \displaystyle \left[ 17.0120270873622, \  35.0091410950936, \  37.5047555769823\right]



it should be uniform too. Finally, we test the corner case where
:math:`b=c`, so let :math:`[10, 20)` and :math:`[20,40)`,

.. code:: ipython3

    observations = take(samples(slice(10, 20), slice(20, 40)), 1_000_000)

look again at some observations,

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. math::

    \displaystyle \left[ 10.3138042492069, \  10.4617132954871, \  10.5809908902109, \  10.6010442620482, \  10.6887497588823, \  11.7254181853172, \  11.8271470660681, \  12.048102938299, \  12.247139166854, \  12.2549353500679, \  13.2914091982908, \  13.4459073192196, \  13.647656459474, \  13.6910471558124, \  14.123566779006, \  14.2551894831251, \  14.5939427033854, \  14.6234703446175, \  14.9279109995807, \  14.9973541378411, \  15.0791166070317, \  15.2549787961636, \  15.4904856657929, \  16.0545164052765, \  16.2117379525326, \  16.4562397604181, \  16.6026015779431, \  16.7088902884986, \  16.710077039622, \  16.7992315042294, \  17.0321951774865, \  17.0352934965467, \  17.1050312631865, \  17.1532373657116, \  17.2682305118815, \  17.6599373467649, \  17.7370926273273, \  17.9235109312741, \  18.0087370813096, \  18.3273991743432, \  18.453592676932, \  18.8665261441337, \  19.1234550824922, \  19.2080876073163, \  19.5772209175442, \  19.6682654821401, \  20.3324520675891, \  20.3935779739885, \  20.8497271574752, \  21.133667544165, \  22.4415811131435, \  23.3999361089338, \  23.7715554382267, \  23.9257832035858, \  24.0776513632948, \  24.1989723261161, \  24.397401473484, \  24.7098113954197, \  24.8170714792839, \  24.9288957236155, \  26.2281486965927, \  26.4088786487729, \  26.7216839700722, \  26.8329313991179, \  26.8534141521857, \  26.9212145847347, \  27.0736507746228, \  27.364310658668, \  27.5780019679952, \  27.6041135453553, \  28.6692581540964, \  29.2261026975492, \  29.8942781614073, \  29.9434199769997, \  30.5522517953459, \  30.5629687527648, \  30.6138031778256, \  31.6349049382327, \  32.0261404496609, \  32.3687354050969, \  33.5183891001866, \  33.6681315671644, \  33.7476739047184, \  34.0354106514961, \  34.1369089638411, \  34.8266328131936, \  34.8938204741322, \  35.0510595035298, \  35.2891255966106, \  35.3063843466018, \  35.600883275618, \  36.6177060928967, \  36.8418045240622, \  37.2710083645502, \  37.822150301474, \  38.140279734327, \  38.5882057987763, \  38.8307542283743, \  39.8252720322953, \  39.9201137902001\right]



and check the corresponding quantiles

.. code:: ipython3

    statistics.quantiles(observations)




.. math::

    \displaystyle \left[ 15.0015750374179, \  20.0017670659526, \  30.0210196691344\right]



it should be uniform either. Finally, attempt a sampling from ``4``
slices,

.. code:: ipython3

    observations = take(samples(slice(0, 5), slice(10, 15), slice(20, 25), slice(30, 35)), 1_000_000)

look again at some observations,

.. code:: ipython3

    sorted([i for _, i in zip(range(100), observations)])




.. math::

    \displaystyle \left[ 0.0721670174820011, \  0.251638150885252, \  0.454589866470005, \  0.797351604131717, \  1.04201184990024, \  1.09601049158395, \  1.12910750448021, \  1.39510803467466, \  1.56747852768705, \  2.26465608665783, \  2.49460993241168, \  2.77596902018942, \  2.78401315008064, \  3.24602453892028, \  3.27130359090896, \  3.80736075048964, \  4.58829681683315, \  4.79749739461101, \  4.80836601616213, \  4.81025028577513, \  10.1305526947971, \  10.7491394328919, \  10.755628043989, \  10.7719744270599, \  10.8535106453669, \  11.0189194785127, \  11.2086447641603, \  11.2190090156744, \  11.2237108329476, \  11.842905910204, \  11.86879879227, \  11.9138896820439, \  12.3076402142551, \  12.4908710057721, \  12.5955375064324, \  12.6579629437233, \  12.761062485281, \  12.802849955083, \  13.0995393418366, \  13.4186839125608, \  13.6094875003084, \  13.6436994354719, \  13.7808136432215, \  14.0152045883098, \  14.273444820187, \  14.4031999317319, \  14.5287018465516, \  14.5716215580827, \  14.6523918120587, \  14.7922512390712, \  14.9092089240726, \  14.9210711970253, \  20.09201231095, \  20.2316827131464, \  20.4319958792265, \  20.5882144556517, \  20.6839480954171, \  20.9495072655182, \  21.0392822592264, \  21.5490769500205, \  21.6099700754839, \  22.1670165044208, \  22.2202641136998, \  22.2334323790502, \  22.2428963606999, \  22.322312074206, \  22.5961433112942, \  23.1575726062898, \  23.52815490794, \  23.780633763837, \  24.0563980112762, \  24.3177335923029, \  24.4090378298098, \  24.5454848322612, \  24.6093629967583, \  24.6281061651609, \  30.1162361655058, \  30.1269636184324, \  30.3184926896898, \  30.3736768338581, \  30.3890273183079, \  30.4755508925172, \  30.6602246415675, \  31.0146511055455, \  31.0521118790551, \  31.313281056821, \  31.3488854274348, \  31.7768642602927, \  32.2154220724097, \  32.2174254745533, \  32.5372781276159, \  32.6677262075565, \  32.7504668126776, \  33.2928179251376, \  33.3175785766162, \  33.8002294825965, \  33.9799188461708, \  34.0653799094503, \  34.4869921698902, \  34.8345926013318\right]



and check the corresponding quantiles

.. code:: ipython3

    statistics.quantiles(observations)




.. math::

    \displaystyle \left[ 10.0082975897974, \  20.0087565216306, \  30.0077741144062\right]



it should be uniform either.

Bernoulli random variable
-------------------------

.. code:: ipython3

    int(True) # this is a very quick check to see if a Boolean can be used as integer

.. code:: ipython3

    def Bernoulli(p):
        'This is a generator for a Bernoulli random variable of parameter `p` for success.'
        
        while True:              # forever we loop
            r = random.random()  # get a sample
            yield int(r < p)     # if that sample denotes a success or a failure we *yield* that outcome

.. code:: ipython3

    B = Bernoulli(p=0.6) # B is our random variable
    B

.. code:: ipython3

    next(B)

.. code:: ipython3

    next(B)

.. code:: ipython3

    next(B)

.. code:: ipython3

    next(B)

.. code:: ipython3

    list(take(B, 20))

.. code:: ipython3

    C = collections.Counter(take(B, 1_000_000))
    C

.. code:: ipython3

    C[1]/(C[0]+C[1])

where

.. code:: ipython3

    print(collections.Counter.__doc__)

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
            halving = halving >> 1 # int(halving / 2)
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

    33.2 µs ± 111 ns per loop (mean ± std. dev. of 7 runs, 10000 loops each)


is *slower* than the primitive one

.. code:: ipython3

    %timeit 293819385789379687596845 * 921038209831568476843584365


.. parsed-literal::

    98.8 ns ± 0.164 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)


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

and observe that it is a little bit *faster* than our former
implementation

.. code:: ipython3

    %timeit rpm_strict(293819385789379687596845, 921038209831568476843584365)

Fixed sum
---------

.. code:: ipython3

    def subarrays(L):
       return (L[i:j] for i in range(len(L)) for j in range(i, len(L)+1))

.. code:: ipython3

    L = [-1, 5, 8, -9, 4, 1]

.. code:: ipython3

    list(subarrays(L))

.. code:: ipython3

    def fixed_sum(L, n):
        return filter(lambda s: sum(s)==n, subarrays(L))

.. code:: ipython3

    list(fixed_sum(L, 10))

.. code:: ipython3

    def partial_sums(L):
        g = itertools.accumulate(subarrays(L), lambda s, each: s + each[-1] if each else 0, initial=0)
        next(g) # to ignore the initial 0 given above
        return g

.. code:: ipython3

    list(partial_sums(L))

Toward an optimization…

.. code:: ipython3

    def subarrays_rev(L):
       return (tuple(L[i:j]) for i in range(len(L)-1, -1, -1) for j in range(i+1, len(L)+1))

.. code:: ipython3

    list(subarrays_rev(L))

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

.. code:: ipython3

    cache # have a look at the collected values

.. code:: ipython3

    def sample(n):
        O, b, *rest = bin(random.getrandbits(n)) # because `string`s are iterable objects indeed.
        return list(map(int, rest))

where

.. code:: ipython3

    help(random.getrandbits)

.. code:: ipython3

    LL = sample(1000)

.. code:: ipython3

    assert set(map(tuple, fixed_sum(LL, 10))) == set(fixed_sum_rev(LL, 10))

.. code:: ipython3

    %timeit list(fixed_sum(LL, 10))

.. code:: ipython3

    %timeit list(fixed_sum_rev(LL, 10))

**INTERMEZZO**

.. code:: ipython3

    if 4 < 8:
        print('a')
    else:
        pass

.. code:: ipython3

    b = if 4 < 8:
           '''
           
           
           lots of code
           
           
           
           
           '''
        else:
           6

.. code:: ipython3

    b = 5 if 4 < 8 else 6

.. code:: ipython3

    b

Some strange uses of recursion
------------------------------

For more on this recursion schemata see
https://www.cs.ox.ac.uk/people/ralf.hinze/publications/ICFP09.pdf and
also
https://www.sciencedirect.com/science/article/pii/S1571066104809721.

Constants
~~~~~~~~~

.. code:: ipython3

    def const(n):
        
        yield n
        
        yield from const(n)

.. code:: ipython3

    const(1)




.. parsed-literal::

    <generator object const at 0x7f425c332970>



.. code:: ipython3

    ones = const(1)

.. code:: ipython3

    list(take(ones, 10))




.. parsed-literal::

    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1]



Nats
~~~~

.. code:: ipython3

    def nats():
        
        yield 0
        
        g = nats() # !!
        
        yield from map(lambda n: n + 1, g)

.. code:: ipython3

    list(take(nats(), 10))




.. parsed-literal::

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



Primes
~~~~~~

Consider the following functional specification for the naturals that
are also *primes*

.. code:: haskell

   primes = filterPrime [2..]
     where filterPrime (p:xs) =
             p : filterPrime [x | x <- xs, x `mod` p /= 0]

.. code:: ipython3

    def primes():
        
        def P(numbers):
            
            prime = next(numbers) # get the next prime from the iterator `it`.
            
            yield prime # yield the next prime number
            
            def not_divisible_by_prime(n):  # a mnemonic predicate.
                q, r = divmod(n, prime)
                return r != 0 
            
            yield from P(filter(not_divisible_by_prime, numbers)) # `numbers` has been advanced before.
        
        yield from P(itertools.count(2))

.. code:: ipython3

    list(take(primes(), 20))




.. parsed-literal::

    [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71]



Fibonacci, again
~~~~~~~~~~~~~~~~

Remember,

.. math::


       f_{n+2} = f_{n+1} + f_{n}, \quad \text{where} \quad f_{0} = 0 \wedge f_{1} = 1

.. code:: ipython3

    def fibs(first=0, second=1):
        
        yield first  # the first number in the Fibonacci series,
        yield second # ... and the second one.
        
        f, ff = itertools.tee(fibs(first, second)) # duplicate the stream of fibonacci numbers.
        
        next(ff) # advance just one of them
        
        yield from map(operator.add, f, ff) # according to the Fibonacci rule, yield all the rest.

.. code:: ipython3

    list(take(fibs(), 20))




.. parsed-literal::

    [0,
     1,
     1,
     2,
     3,
     5,
     8,
     13,
     21,
     34,
     55,
     89,
     144,
     233,
     377,
     610,
     987,
     1597,
     2584,
     4181]



…and again
^^^^^^^^^^

.. code:: ipython3

    from sympy import IndexedBase, init_printing # SymPy for symbolic computation
    
    init_printing() # pretty printing math symbols and expressions

.. code:: ipython3

    x = IndexedBase('x')
    x[1] # indexing as done in math.




.. math::

    \displaystyle {x}_{1}



.. code:: ipython3

    fibos = list(take(fibs(x[0], x[1]), 20)) # generate an abstract schema
    fibos




.. math::

    \displaystyle \left[ {x}_{0}, \  {x}_{1}, \  {x}_{0} + {x}_{1}, \  {x}_{0} + 2 {x}_{1}, \  2 {x}_{0} + 3 {x}_{1}, \  3 {x}_{0} + 5 {x}_{1}, \  5 {x}_{0} + 8 {x}_{1}, \  8 {x}_{0} + 13 {x}_{1}, \  13 {x}_{0} + 21 {x}_{1}, \  21 {x}_{0} + 34 {x}_{1}, \  34 {x}_{0} + 55 {x}_{1}, \  55 {x}_{0} + 89 {x}_{1}, \  89 {x}_{0} + 144 {x}_{1}, \  144 {x}_{0} + 233 {x}_{1}, \  233 {x}_{0} + 377 {x}_{1}, \  377 {x}_{0} + 610 {x}_{1}, \  610 {x}_{0} + 987 {x}_{1}, \  987 {x}_{0} + 1597 {x}_{1}, \  1597 {x}_{0} + 2584 {x}_{1}, \  2584 {x}_{0} + 4181 {x}_{1}\right]



.. code:: ipython3

    [expr.subs({x[0]:0, x[1]:1}) for expr in fibos] # Fibonacci numbers, as usual.




.. math::

    \displaystyle \left[ 0, \  1, \  1, \  2, \  3, \  5, \  8, \  13, \  21, \  34, \  55, \  89, \  144, \  233, \  377, \  610, \  987, \  1597, \  2584, \  4181\right]



.. code:: ipython3

    [expr.subs({x[0]:2, x[1]:1}) for expr in fibos] # Lucas numbers, less usual.




.. math::

    \displaystyle \left[ 2, \  1, \  3, \  4, \  7, \  11, \  18, \  29, \  47, \  76, \  123, \  199, \  322, \  521, \  843, \  1364, \  2207, \  3571, \  5778, \  9349\right]


