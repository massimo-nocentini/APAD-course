Object Orientation
==================

.. code:: ipython3

    import this


.. parsed-literal::

    The Zen of Python, by Tim Peters
    
    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!


.. code:: ipython3

    def a():
        pass

.. code:: ipython3

    a




.. parsed-literal::

    <function __main__.a()>



.. code:: ipython3

    callable(a)




.. parsed-literal::

    True



.. code:: ipython3

    a()

.. code:: ipython3

    type(_)




.. parsed-literal::

    bool



.. code:: ipython3

    class A(object):
        pass

.. code:: ipython3

    a = A() # `a` is an instance of the class `A`

.. code:: ipython3

    type(a)




.. parsed-literal::

    __main__.A



.. code:: java


   public class A {

       private String name;
       
       public A(String name){
           this.name = name;
       }

       public String a(String a){}
       public String a(int a){}

   }

.. code:: ipython3

    class B(object):
        
        def __init__(self, v):
            print('The ctor of the class B has been invoked.')
            self.slot = v
            
        def yourself(self):
            return self
            

.. code:: ipython3

    B




.. parsed-literal::

    __main__.B



.. code:: ipython3

    dir(B)




.. parsed-literal::

    ['__class__',
     '__delattr__',
     '__dict__',
     '__dir__',
     '__doc__',
     '__eq__',
     '__format__',
     '__ge__',
     '__getattribute__',
     '__gt__',
     '__hash__',
     '__init__',
     '__init_subclass__',
     '__le__',
     '__lt__',
     '__module__',
     '__ne__',
     '__new__',
     '__reduce__',
     '__reduce_ex__',
     '__repr__',
     '__setattr__',
     '__sizeof__',
     '__str__',
     '__subclasshook__',
     '__weakref__',
     'yourself']



.. code:: ipython3

    b = B(v=4) # I'm creating an instance of the class B, referencing it with `b`.


.. parsed-literal::

    The ctor of the class B has been invoked.


.. code:: ipython3

    b.slot




.. parsed-literal::

    4



.. code:: ipython3

    b.yourself()




.. parsed-literal::

    <__main__.B at 0x7ffecfa820a0>



.. code:: ipython3

    assert b == b.yourself()

.. code:: ipython3

    callable(B)




.. parsed-literal::

    True



.. code:: ipython3

    class Node(object):
        
        def __init__(self, left, value, right):
            # the followings are all slots for each instance of this class.
            self.left = left
            self.value = value
            self.right = right
            
        def __repr__(self) -> str:
            return 'Node({0}, {1}, {2})'.format(repr(self.left), repr(self.value), repr(self.right))
        
        def __str__(self) -> str:
            return '({}) <- {} -> ({})'.format(self.left, self.value, self.right)
        
        def visit_inorder(self, f):
            assert callable(f) # `f` is a Callable object
            self.left.visit_inorder(f)
            f(self.value) #!
            self.right.visit_inorder(f)
            
    class EmptyTree:
        
        def visit_inorder(self, f):
            'There is no value that should be passed to `f`'
            pass
    
        def __str__(self):
            return '•'
        
        def __repr__(self):
            return 'EmptyTree()'
        
    empty_tree = EmptyTree() # we will use the lone instance of an empty tree.

.. code:: ipython3

    n1 = Node(left=empty_tree, value=3, right=empty_tree)

.. code:: ipython3

    n1




.. parsed-literal::

    Node(EmptyTree(), 3, EmptyTree())



.. code:: ipython3

    callable(print)




.. parsed-literal::

    True



.. code:: ipython3

    n1.visit_inorder(f=print)


.. parsed-literal::

    3


.. code:: ipython3

    type(n1)




.. parsed-literal::

    __main__.Node



.. code:: ipython3

    Node(None, 3, None)




.. parsed-literal::

    Node(None, 3, None)



.. code:: ipython3

    repr(n1)




.. parsed-literal::

    'Node(EmptyTree(), 3, EmptyTree())'



.. code:: ipython3

    str(n1)




.. parsed-literal::

    '(•) <- 3 -> (•)'



.. code:: ipython3

    n2 = Node(left=n1, value=2, right=n1)
    n2




.. parsed-literal::

    Node(Node(EmptyTree(), 3, EmptyTree()), 2, Node(EmptyTree(), 3, EmptyTree()))



.. code:: ipython3

    print(str(n2))


.. parsed-literal::

    ((•) <- 3 -> (•)) <- 2 -> ((•) <- 3 -> (•))


.. code:: ipython3

    str(Node(left=[], value=object(), right=4))




.. parsed-literal::

    '([]) <- <object object at 0x7ffecf65db30> -> (4)'



.. code:: ipython3

    n2.visit_inorder(f=print)


.. parsed-literal::

    3
    2
    3


.. code:: ipython3

    n3 = Node(left=n2, value=0, right=n1)

.. code:: ipython3

    s = []
    n3.visit_inorder(f=lambda v: s.append(v))
    s




.. parsed-literal::

    [3, 2, 3, 0, 3]



.. code:: ipython3

    def p(a):
        print(a)
        a + '4'

.. code:: ipython3

    def p_equiv(a):
        print(a)
        a + '4'
        return None

.. code:: ipython3

    a = p('hello world')


.. parsed-literal::

    hello world


.. code:: ipython3

    a = p_equiv('hello world')


.. parsed-literal::

    hello world


.. code:: ipython3

    type(a)




.. parsed-literal::

    NoneType



.. code:: ipython3

    b = lambda: print('hello world')
    a = b()


.. parsed-literal::

    hello world


.. code:: ipython3

    type(a)




.. parsed-literal::

    NoneType



--------------

Back to the past…

.. code:: ipython3

    class Node(object):
        
        def __init__(self, left, value, right):
            # the followings are all slots for each instance of this class.
            self.left = left
            self.value = value
            self.right = right
            
        def __repr__(self) -> str:
            return 'Node({0}, {1}, {2})'.format(repr(self.left), repr(self.value), repr(self.right))
        
        def __str__(self) -> str:
            return '({}) <- {} -> ({})'.format(self.left, self.value, self.right)
        
        def visit_inorder(self, f):
            assert callable(f) # `f` is a Callable object
            if self.left: self.left.visit_inorder(f)
            f(self.value) #!
            if self.right: self.right.visit_inorder(f)

.. code:: ipython3

    n1 = Node(left=None, value=3, right=None)

.. code:: ipython3

    n1.visit_inorder(f=print)


.. parsed-literal::

    3


.. code:: ipython3

    n2 = Node(left=n1, value=2, right=n1)
    n2




.. parsed-literal::

    Node(Node(None, 3, None), 2, Node(None, 3, None))



.. code:: ipython3

    n2.visit_inorder(f=print)


.. parsed-literal::

    3
    2
    3


--------------

Back to the future…

.. code:: ipython3

    True, False




.. parsed-literal::

    (True, False)



.. code:: ipython3

    type(True)




.. parsed-literal::

    bool



.. code:: ipython3

    type(False)




.. parsed-literal::

    bool



.. code:: ipython3

    class Node(object):
        
        def __init__(self, left, value, right):
            # the followings are all slots for each instance of this class.
            self.left = left
            self.value = value
            self.right = right
            
        def __repr__(self) -> str:
            return 'Node({0}, {1}, {2})'.format(repr(self.left), repr(self.value), repr(self.right))
        
        def __str__(self) -> str:
            return '({}) <- {} -> ({})'.format(self.left, self.value, self.right)
        
        def visit_inorder(self, f):
            assert callable(f) # `f` is a Callable object
            
            if self.left: self.left.visit_inorder(f)
                
            f(self.value) #!
            
            if self.right: self.right.visit_inorder(f)
                
        def __bool__(self):
            print('Print from Node.__bool__')
            return True
        
          
    class EmptyTree:
        
        def __str__(self):
            return '•'
        
        def __repr__(self):
            return 'EmptyTree()'
        
        def __bool__(self):
            print('Print from EmptyTree.__bool__')
            return False
        
    empty_tree = EmptyTree() # we will use the lone instance of an empty tree.

.. code:: ipython3

    n2 = Node(left=n1, value=2, right=n1)
    n2.visit_inorder(f=print)


.. parsed-literal::

    Print from EmptyTree.__bool__
    3
    Print from EmptyTree.__bool__
    2
    Print from EmptyTree.__bool__
    3
    Print from EmptyTree.__bool__


--------------

.. code:: ipython3

    class F(object):
        
        def __init__(self):
            self.accumulator = []
        
        def __call__(self, arg):
            self.accumulator.append(arg)
            
        def __iter__(self):
            return iter(self.accumulator)
        
        def __next__(self):
            yield from self.accumulator

.. code:: ipython3

    f = F()

.. code:: ipython3

    f.__call__(4)

.. code:: ipython3

    f.accumulator




.. parsed-literal::

    [4]



.. code:: ipython3

    [f(i) for i in range(0, 100, 2)]




.. parsed-literal::

    [None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None,
     None]



.. code:: ipython3

    f.accumulator




.. parsed-literal::

    [0,
     2,
     4,
     6,
     8,
     10,
     12,
     14,
     16,
     18,
     20,
     22,
     24,
     26,
     28,
     30,
     32,
     34,
     36,
     38,
     40,
     42,
     44,
     46,
     48,
     50,
     52,
     54,
     56,
     58,
     60,
     62,
     64,
     66,
     68,
     70,
     72,
     74,
     76,
     78,
     80,
     82,
     84,
     86,
     88,
     90,
     92,
     94,
     96,
     98]



.. code:: ipython3

    g = F()

.. code:: ipython3

    n1 = Node(left=empty_tree, value=3, right=empty_tree)
    n2 = Node(left=n1, value=2, right=n1)
    n2.visit_inorder(f=g)

.. code:: ipython3

    g.accumulator




.. parsed-literal::

    [3, 2, 3]



.. code:: ipython3

    for i in g:
        print(i+1)


.. parsed-literal::

    4
    3
    4


.. code:: ipython3

    next(g)




.. parsed-literal::

    <generator object F.__next__ at 0x7ffed0cf19e0>



.. code:: ipython3

    next(_)




.. parsed-literal::

    3


