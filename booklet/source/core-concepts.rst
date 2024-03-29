Core concepts
=============

This chapter deepens our understanding of some concepts seen in
http://www.andreamarino.it/python/thinkcspy/.

We quote from the official help system.

| First,
| >\ *Objects* are Python’s abstraction for data. All data in a Python
  program is represented by objects or by relations between objects. (In
  a sense, and in conformance to Von Neumann’s model of a “stored
  program computer”, code is also represented by objects.)

Second, >Every object has an identity, a type and a value. An object’s
*identity* never changes once it has been created; you may think of it
as the object’s address in memory. The ‘“is”’ operator compares the
identity of two objects; the “id()” function returns an integer
representing its identity.

where

.. code:: ipython3

    id




.. parsed-literal::

    <function id(obj, /)>



.. code:: ipython3

    help(id)


.. parsed-literal::

    Help on built-in function id in module builtins:
    
    id(obj, /)
        Return the identity of an object.
        
        This is guaranteed to be unique among simultaneously existing objects.
        (CPython uses the object's memory address.)
    


.. code:: ipython3

    help(help)


.. parsed-literal::

    Help on _Helper in module _sitebuiltins object:
    
    class _Helper(builtins.object)
     |  Define the builtin 'help'.
     |  
     |  This is a wrapper around pydoc.help that provides a helpful message
     |  when 'help' is typed at the Python interactive prompt.
     |  
     |  Calling help() at the Python prompt starts an interactive help session.
     |  Calling help(thing) prints help for the python object 'thing'.
     |  
     |  Methods defined here:
     |  
     |  __call__(self, *args, **kwds)
     |      Call self as a function.
     |  
     |  __repr__(self)
     |      Return repr(self).
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors defined here:
     |  
     |  __dict__
     |      dictionary for instance variables (if defined)
     |  
     |  __weakref__
     |      list of weak references to the object (if defined)
    


.. code:: ipython3

    help(object)


.. parsed-literal::

    Help on class object in module builtins:
    
    class object
     |  The base class of the class hierarchy.
     |  
     |  When called, it accepts no arguments and returns a new featureless
     |  instance that has no instance attributes and cannot be given any.
     |  
     |  Built-in subclasses:
     |      anext_awaitable
     |      async_generator
     |      async_generator_asend
     |      async_generator_athrow
     |      ... and 91 other subclasses
     |  
     |  Methods defined here:
     |  
     |  __delattr__(self, name, /)
     |      Implement delattr(self, name).
     |  
     |  __dir__(self, /)
     |      Default dir() implementation.
     |  
     |  __eq__(self, value, /)
     |      Return self==value.
     |  
     |  __format__(self, format_spec, /)
     |      Default object formatter.
     |  
     |  __ge__(self, value, /)
     |      Return self>=value.
     |  
     |  __getattribute__(self, name, /)
     |      Return getattr(self, name).
     |  
     |  __gt__(self, value, /)
     |      Return self>value.
     |  
     |  __hash__(self, /)
     |      Return hash(self).
     |  
     |  __init__(self, /, *args, **kwargs)
     |      Initialize self.  See help(type(self)) for accurate signature.
     |  
     |  __le__(self, value, /)
     |      Return self<=value.
     |  
     |  __lt__(self, value, /)
     |      Return self<value.
     |  
     |  __ne__(self, value, /)
     |      Return self!=value.
     |  
     |  __reduce__(self, /)
     |      Helper for pickle.
     |  
     |  __reduce_ex__(self, protocol, /)
     |      Helper for pickle.
     |  
     |  __repr__(self, /)
     |      Return repr(self).
     |  
     |  __setattr__(self, name, value, /)
     |      Implement setattr(self, name, value).
     |  
     |  __sizeof__(self, /)
     |      Size of object in memory, in bytes.
     |  
     |  __str__(self, /)
     |      Return str(self).
     |  
     |  ----------------------------------------------------------------------
     |  Class methods defined here:
     |  
     |  __init_subclass__(...) from builtins.type
     |      This method is called when a class is subclassed.
     |      
     |      The default implementation does nothing. It may be
     |      overridden to extend subclasses.
     |  
     |  __subclasshook__(...) from builtins.type
     |      Abstract classes can override this to customize issubclass().
     |      
     |      This is invoked early on by abc.ABCMeta.__subclasscheck__().
     |      It should return True, False or NotImplemented.  If it returns
     |      NotImplemented, the normal algorithm is used.  Otherwise, it
     |      overrides the normal algorithm (and the outcome is cached).
     |  
     |  ----------------------------------------------------------------------
     |  Static methods defined here:
     |  
     |  __new__(*args, **kwargs) from builtins.type
     |      Create and return a new object.  See help(type) for accurate signature.
     |  
     |  ----------------------------------------------------------------------
     |  Data and other attributes defined here:
     |  
     |  __class__ = <class 'type'>
     |      type(object_or_name, bases, dict)
     |      type(object) -> the object's type
     |      type(name, bases, dict) -> a new type
    


.. code:: ipython3

    o = object()

.. code:: ipython3

    o




.. parsed-literal::

    <object at 0x10d85b310>



.. code:: ipython3

    id(o)




.. parsed-literal::

    4521833232



.. code:: ipython3

    w = object()

.. code:: ipython3

    o is w




.. parsed-literal::

    False



.. code:: ipython3

    o is o




.. parsed-literal::

    True



.. code:: ipython3

    type(object)




.. parsed-literal::

    type



.. code:: ipython3

    type(type)




.. parsed-literal::

    type



.. code:: ipython3

    help(type)


.. parsed-literal::

    Help on class type in module builtins:
    
    class type(object)
     |  type(object_or_name, bases, dict)
     |  type(object) -> the object's type
     |  type(name, bases, dict) -> a new type
     |  
     |  Methods defined here:
     |  
     |  __call__(self, /, *args, **kwargs)
     |      Call self as a function.
     |  
     |  __delattr__(self, name, /)
     |      Implement delattr(self, name).
     |  
     |  __dir__(self, /)
     |      Specialized __dir__ implementation for types.
     |  
     |  __getattribute__(self, name, /)
     |      Return getattr(self, name).
     |  
     |  __init__(self, /, *args, **kwargs)
     |      Initialize self.  See help(type(self)) for accurate signature.
     |  
     |  __instancecheck__(self, instance, /)
     |      Check if an object is an instance.
     |  
     |  __or__(self, value, /)
     |      Return self|value.
     |  
     |  __repr__(self, /)
     |      Return repr(self).
     |  
     |  __ror__(self, value, /)
     |      Return value|self.
     |  
     |  __setattr__(self, name, value, /)
     |      Implement setattr(self, name, value).
     |  
     |  __sizeof__(self, /)
     |      Return memory consumption of the type object.
     |  
     |  __subclasscheck__(self, subclass, /)
     |      Check if a class is a subclass.
     |  
     |  __subclasses__(self, /)
     |      Return a list of immediate subclasses.
     |  
     |  mro(self, /)
     |      Return a type's method resolution order.
     |  
     |  ----------------------------------------------------------------------
     |  Class methods defined here:
     |  
     |  __prepare__(...)
     |      __prepare__() -> dict
     |      used to create the namespace for the class statement
     |  
     |  ----------------------------------------------------------------------
     |  Static methods defined here:
     |  
     |  __new__(*args, **kwargs)
     |      Create and return a new object.  See help(type) for accurate signature.
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors defined here:
     |  
     |  __abstractmethods__
     |  
     |  __annotations__
     |  
     |  __dict__
     |  
     |  __text_signature__
     |  
     |  ----------------------------------------------------------------------
     |  Data and other attributes defined here:
     |  
     |  __base__ = <class 'object'>
     |      The base class of the class hierarchy.
     |      
     |      When called, it accepts no arguments and returns a new featureless
     |      instance that has no instance attributes and cannot be given any.
     |  
     |  
     |  __bases__ = (<class 'object'>,)
     |  
     |  __basicsize__ = 888
     |  
     |  __dictoffset__ = 264
     |  
     |  __flags__ = 2148031744
     |  
     |  __itemsize__ = 40
     |  
     |  __mro__ = (<class 'type'>, <class 'object'>)
     |  
     |  __weakrefoffset__ = 368
    


.. code:: ipython3

    type(3.14)




.. parsed-literal::

    float



.. code:: ipython3

    type([1,2,3])




.. parsed-literal::

    list



.. code:: ipython3

    type(help)




.. parsed-literal::

    _sitebuiltins._Helper



.. code:: ipython3

    type(id)




.. parsed-literal::

    builtin_function_or_method



.. code:: ipython3

    type(1)




.. parsed-literal::

    int



Third, >An object’s type determines the operations that the object
supports >(e.g., “does it have a length?”) and also defines the possible
values >for objects of that type. The “type()” function returns an
object’s >type (which is an object itself). Like its identity, an
object’s >\ *type* is also unchangeable.

Fourth, >The *value* of some objects can change. Objects whose value can
change are said to be *mutable*; objects whose value is unchangeable
once they are created are called *immutable*. (The value of an immutable
container object that contains a reference to a mutable object can
change when the latter’s value is changed; however the container is
still considered immutable, because the collection of objects it
contains cannot be changed. So, immutability is not strictly the same as
having an unchangeable value, it is more subtle.) An object’s mutability
is determined by its type; for instance, numbers, strings and tuples are
immutable, while dictionaries and lists are mutable.

Fifth, >Some objects contain references to other objects; these are
called *containers*. Examples of containers are tuples, lists and
dictionaries. The references are part of a container’s value. In most
cases, when we talk about the value of a container, we imply the values,
not the identities of the contained objects; however, when we talk about
the mutability of a container, only the identities of the immediately
contained objects are implied. So, if an immutable container (like a
tuple) contains a reference to a mutable object, its value changes if
that mutable object is changed.

Sixth, >Types affect almost all aspects of object behavior. Even the
importance of object identity is affected in some sense: for immutable
types, operations that compute new values may actually return a
reference to any existing object with the same type and value, while for
mutable objects this is not allowed.

.. code:: ipython3

    class A(object):
        
        x = 3
        y = 4

.. code:: ipython3

    a = A()

.. code:: ipython3

    id(a)




.. parsed-literal::

    4534062624



.. code:: ipython3

    type(a)




.. parsed-literal::

    __main__.A



.. code:: ipython3

    A().y

--------------

.. code:: ipython3

    L = []
    L




.. parsed-literal::

    []



.. code:: ipython3

    L.append(1)
    L.append(3.14)

.. code:: ipython3

    L




.. parsed-literal::

    [1, 3.14]



.. code:: ipython3

    3, 3.14, 3 + 5J




.. parsed-literal::

    (3, 3.14, (3+5j))



.. code:: ipython3

    t = (1, 3.14)
    t




.. parsed-literal::

    (1, 3.14)



.. code:: ipython3

    type(t)




.. parsed-literal::

    tuple



.. code:: ipython3

    t.index(3.14)




.. parsed-literal::

    1



.. code:: ipython3

    tt = (3.14, [])
    tt, id(tt)




.. parsed-literal::

    ((3.14, []), 4533921408)



.. code:: ipython3

    tt[1].append(4)

.. code:: ipython3

    tt, id(tt)




.. parsed-literal::

    ((3.14, [4]), 4533921408)



--------------

.. code:: ipython3

    type({})




.. parsed-literal::

    dict



.. code:: ipython3

    type(set())




.. parsed-literal::

    set



.. code:: ipython3

    S = set()

.. code:: ipython3

    S




.. parsed-literal::

    set()



.. code:: ipython3

    S.add(3.14)
    S




.. parsed-literal::

    {3.14}



.. code:: ipython3

    S.add('hello')
    S.add(tuple())

.. code:: ipython3

    S




.. parsed-literal::

    {(), 3.14, 'hello'}



.. code:: ipython3

    S.add(tuple())

.. code:: ipython3

    S




.. parsed-literal::

    {(), 3.14, 'hello'}



.. code:: ipython3

    S.add([])


::


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    /var/folders/0k/y3r96gt12zn1mzgztql9m_8m0000gn/T/ipykernel_2745/334196350.py in <module>
    ----> 1 S.add([])
    

    TypeError: unhashable type: 'list'


.. code:: ipython3

    L = []
    t = (3.14, L)
    tt = (1.618, L)

.. code:: ipython3

    id(L), id(t), id(tt)




.. parsed-literal::

    (4613187392, 4612579136, 4613347328)



.. code:: ipython3

    S = { t, tt }
    S


::


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    /var/folders/0k/y3r96gt12zn1mzgztql9m_8m0000gn/T/ipykernel_2745/4157842914.py in <module>
    ----> 1 S = { t, tt }
          2 S


    TypeError: unhashable type: 'list'


.. code:: ipython3

    hash(L)


::


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    /var/folders/0k/y3r96gt12zn1mzgztql9m_8m0000gn/T/ipykernel_2745/1001898879.py in <module>
    ----> 1 hash(L)
    

    TypeError: unhashable type: 'list'


.. code:: ipython3

    {(3.14, ()), (1.618, frozenset())}




.. parsed-literal::

    {(1.618, frozenset()), (3.14, ())}



.. code:: ipython3

    D = {}
    D[[]] = 3.14


::


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    /var/folders/0k/y3r96gt12zn1mzgztql9m_8m0000gn/T/ipykernel_2745/1357002942.py in <module>
          1 D = {}
    ----> 2 D[[]] = 3.14
    

    TypeError: unhashable type: 'list'


--------------

.. code:: ipython3

    a = 1; b = 1 # "a" and "b" may or may not refer to the same object with the value one,
    id(a), id(b) # depending on the implementation




.. parsed-literal::

    (94869411972864, 94869411972864)



.. code:: ipython3

    c = []; d = []
    id(c), id(d) # "c" and "d" are guaranteed to refer to two different, unique, newly created empty lists.




.. parsed-literal::

    (140325921500736, 140325921500224)



.. code:: ipython3

    type(c), type(d)




.. parsed-literal::

    (list, list)



A full description can be read by evaluating the following,

.. code:: ipython3

    help('OBJECTS')


.. parsed-literal::

    Objects, values and types
    *************************
    
    *Objects* are Python’s abstraction for data.  All data in a Python
    program is represented by objects or by relations between objects. (In
    a sense, and in conformance to Von Neumann’s model of a “stored
    program computer”, code is also represented by objects.)
    
    Every object has an identity, a type and a value.  An object’s
    *identity* never changes once it has been created; you may think of it
    as the object’s address in memory.  The ‘"is"’ operator compares the
    identity of two objects; the "id()" function returns an integer
    representing its identity.
    
    **CPython implementation detail:** For CPython, "id(x)" is the memory
    address where "x" is stored.
    
    An object’s type determines the operations that the object supports
    (e.g., “does it have a length?”) and also defines the possible values
    for objects of that type.  The "type()" function returns an object’s
    type (which is an object itself).  Like its identity, an object’s
    *type* is also unchangeable. [1]
    
    The *value* of some objects can change.  Objects whose value can
    change are said to be *mutable*; objects whose value is unchangeable
    once they are created are called *immutable*. (The value of an
    immutable container object that contains a reference to a mutable
    object can change when the latter’s value is changed; however the
    container is still considered immutable, because the collection of
    objects it contains cannot be changed.  So, immutability is not
    strictly the same as having an unchangeable value, it is more subtle.)
    An object’s mutability is determined by its type; for instance,
    numbers, strings and tuples are immutable, while dictionaries and
    lists are mutable.
    
    Objects are never explicitly destroyed; however, when they become
    unreachable they may be garbage-collected.  An implementation is
    allowed to postpone garbage collection or omit it altogether — it is a
    matter of implementation quality how garbage collection is
    implemented, as long as no objects are collected that are still
    reachable.
    
    **CPython implementation detail:** CPython currently uses a reference-
    counting scheme with (optional) delayed detection of cyclically linked
    garbage, which collects most objects as soon as they become
    unreachable, but is not guaranteed to collect garbage containing
    circular references.  See the documentation of the "gc" module for
    information on controlling the collection of cyclic garbage. Other
    implementations act differently and CPython may change. Do not depend
    on immediate finalization of objects when they become unreachable (so
    you should always close files explicitly).
    
    Note that the use of the implementation’s tracing or debugging
    facilities may keep objects alive that would normally be collectable.
    Also note that catching an exception with a ‘"try"…"except"’ statement
    may keep objects alive.
    
    Some objects contain references to “external” resources such as open
    files or windows.  It is understood that these resources are freed
    when the object is garbage-collected, but since garbage collection is
    not guaranteed to happen, such objects also provide an explicit way to
    release the external resource, usually a "close()" method. Programs
    are strongly recommended to explicitly close such objects.  The
    ‘"try"…"finally"’ statement and the ‘"with"’ statement provide
    convenient ways to do this.
    
    Some objects contain references to other objects; these are called
    *containers*. Examples of containers are tuples, lists and
    dictionaries.  The references are part of a container’s value.  In
    most cases, when we talk about the value of a container, we imply the
    values, not the identities of the contained objects; however, when we
    talk about the mutability of a container, only the identities of the
    immediately contained objects are implied.  So, if an immutable
    container (like a tuple) contains a reference to a mutable object, its
    value changes if that mutable object is changed.
    
    Types affect almost all aspects of object behavior.  Even the
    importance of object identity is affected in some sense: for immutable
    types, operations that compute new values may actually return a
    reference to any existing object with the same type and value, while
    for mutable objects this is not allowed.  E.g., after "a = 1; b = 1",
    "a" and "b" may or may not refer to the same object with the value
    one, depending on the implementation, but after "c = []; d = []", "c"
    and "d" are guaranteed to refer to two different, unique, newly
    created empty lists. (Note that "c = d = []" assigns the same object
    to both "c" and "d".)
    
    Related help topics: TYPES
    


Values and their types
----------------------

Some playground:

.. code:: ipython3

    "Hello, World!", type("Hello, World!")




.. parsed-literal::

    ('Hello, World!', str)



.. code:: ipython3

    _




.. parsed-literal::

    ('Hello, World!', str)



.. code:: ipython3

    _[1]




.. parsed-literal::

    str



.. code:: ipython3

    __




.. parsed-literal::

    ('Hello, World!', str)



.. code:: ipython3

    type(_)




.. parsed-literal::

    tuple



.. code:: ipython3

    type(tuple)




.. parsed-literal::

    type



.. code:: ipython3

    tuple_doc = tuple.__doc__

.. code:: ipython3

    type(tuple_doc)




.. parsed-literal::

    str



.. code:: ipython3

    tuple_doc + ' maybe that is actually true'




.. parsed-literal::

    "Built-in immutable sequence.\n\nIf no argument is given, the constructor returns an empty tuple.\nIf iterable is specified the tuple is initialized from iterable's items.\n\nIf the argument is a tuple, the return value is the same object. maybe that is actually true"



.. code:: ipython3

    help(tuple)


.. parsed-literal::

    Help on class tuple in module builtins:
    
    class tuple(object)
     |  tuple(iterable=(), /)
     |  
     |  Built-in immutable sequence.
     |  
     |  If no argument is given, the constructor returns an empty tuple.
     |  If iterable is specified the tuple is initialized from iterable's items.
     |  
     |  If the argument is a tuple, the return value is the same object.
     |  
     |  Built-in subclasses:
     |      asyncgen_hooks
     |      UnraisableHookArgs
     |  
     |  Methods defined here:
     |  
     |  __add__(self, value, /)
     |      Return self+value.
     |  
     |  __contains__(self, key, /)
     |      Return key in self.
     |  
     |  __eq__(self, value, /)
     |      Return self==value.
     |  
     |  __ge__(self, value, /)
     |      Return self>=value.
     |  
     |  __getattribute__(self, name, /)
     |      Return getattr(self, name).
     |  
     |  __getitem__(self, key, /)
     |      Return self[key].
     |  
     |  __getnewargs__(self, /)
     |  
     |  __gt__(self, value, /)
     |      Return self>value.
     |  
     |  __hash__(self, /)
     |      Return hash(self).
     |  
     |  __iter__(self, /)
     |      Implement iter(self).
     |  
     |  __le__(self, value, /)
     |      Return self<=value.
     |  
     |  __len__(self, /)
     |      Return len(self).
     |  
     |  __lt__(self, value, /)
     |      Return self<value.
     |  
     |  __mul__(self, value, /)
     |      Return self*value.
     |  
     |  __ne__(self, value, /)
     |      Return self!=value.
     |  
     |  __repr__(self, /)
     |      Return repr(self).
     |  
     |  __rmul__(self, value, /)
     |      Return value*self.
     |  
     |  count(self, value, /)
     |      Return number of occurrences of value.
     |  
     |  index(self, value, start=0, stop=9223372036854775807, /)
     |      Return first index of value.
     |      
     |      Raises ValueError if the value is not present.
     |  
     |  ----------------------------------------------------------------------
     |  Class methods defined here:
     |  
     |  __class_getitem__(...) from builtins.type
     |      See PEP 585
     |  
     |  ----------------------------------------------------------------------
     |  Static methods defined here:
     |  
     |  __new__(*args, **kwargs) from builtins.type
     |      Create and return a new object.  See help(type) for accurate signature.
    


.. code:: ipython3

    print(tuple.__doc__)


.. parsed-literal::

    Built-in immutable sequence.
    
    If no argument is given, the constructor returns an empty tuple.
    If iterable is specified the tuple is initialized from iterable's items.
    
    If the argument is a tuple, the return value is the same object.


.. code:: ipython3

    3.2, type(3.2), 3 + 9J, type(3 + 9j)




.. parsed-literal::

    (3.2, float, (3+9j), complex)



.. code:: ipython3

    'I''m a string'




.. parsed-literal::

    'Im a string'



.. code:: ipython3

    '''"Oh no", she exclaimed, "Ben's bike is broken!"'''




.. parsed-literal::

    '"Oh no", she exclaimed, "Ben\'s bike is broken!"'



.. code:: ipython3

    def a(b):
        '''A short summary here.
        
        Some more description here.
        
        >>> a(4)
        None
        
        '''
        ...

.. code:: ipython3

    def b(c):
        '''A short summary here.
        
        Some more description here.
        '''
        pass

.. code:: ipython3

    help(a)


.. parsed-literal::

    Help on function a in module __main__:
    
    a(b)
        A short summary here.
        
        Some more description here.
        
        >>> a(4)
        None
    


.. code:: ipython3

    a.__doc__




.. parsed-literal::

    'A short summary here.\n    \n    Some more description here.\n    \n    >>> a(4)\n    None\n    \n    '



.. code:: ipython3

    print(a.__doc__)


.. parsed-literal::

    A short summary here.
        
        Some more description here.
        
        >>> a(4)
        None
        
        


.. code:: ipython3

    assert a(4) == b(3) == None

.. code:: ipython3

    assert True == False, 'This is quite usual'


::


    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    /var/folders/0k/y3r96gt12zn1mzgztql9m_8m0000gn/T/ipykernel_2745/1794376097.py in <module>
    ----> 1 assert True == False, 'This is quite usual'
    

    AssertionError: This is quite usual


.. code:: ipython3

    id(None), id(None), id(None)




.. parsed-literal::

    (94869411793504, 94869411793504, 94869411793504)



.. code:: ipython3

    ((3 < 4) < 10) < 7




.. parsed-literal::

    True



.. code:: ipython3

    _




.. parsed-literal::

    '"Oh no", she exclaimed, "Ben\'s bike is broken!"'



.. code:: ipython3

    type(_)




.. parsed-literal::

    str



.. code:: ipython3

    help('TYPES')


.. parsed-literal::

    The standard type hierarchy
    ***************************
    
    Below is a list of the types that are built into Python.  Extension
    modules (written in C, Java, or other languages, depending on the
    implementation) can define additional types.  Future versions of
    Python may add types to the type hierarchy (e.g., rational numbers,
    efficiently stored arrays of integers, etc.), although such additions
    will often be provided via the standard library instead.
    
    Some of the type descriptions below contain a paragraph listing
    ‘special attributes.’  These are attributes that provide access to the
    implementation and are not intended for general use.  Their definition
    may change in the future.
    
    None
       This type has a single value.  There is a single object with this
       value. This object is accessed through the built-in name "None". It
       is used to signify the absence of a value in many situations, e.g.,
       it is returned from functions that don’t explicitly return
       anything. Its truth value is false.
    
    NotImplemented
       This type has a single value.  There is a single object with this
       value. This object is accessed through the built-in name
       "NotImplemented". Numeric methods and rich comparison methods
       should return this value if they do not implement the operation for
       the operands provided.  (The interpreter will then try the
       reflected operation, or some other fallback, depending on the
       operator.)  It should not be evaluated in a boolean context.
    
       See Implementing the arithmetic operations for more details.
    
       Changed in version 3.9: Evaluating "NotImplemented" in a boolean
       context is deprecated. While it currently evaluates as true, it
       will emit a "DeprecationWarning". It will raise a "TypeError" in a
       future version of Python.
    
    Ellipsis
       This type has a single value.  There is a single object with this
       value. This object is accessed through the literal "..." or the
       built-in name "Ellipsis".  Its truth value is true.
    
    "numbers.Number"
       These are created by numeric literals and returned as results by
       arithmetic operators and arithmetic built-in functions.  Numeric
       objects are immutable; once created their value never changes.
       Python numbers are of course strongly related to mathematical
       numbers, but subject to the limitations of numerical representation
       in computers.
    
       Python distinguishes between integers, floating point numbers, and
       complex numbers:
    
       "numbers.Integral"
          These represent elements from the mathematical set of integers
          (positive and negative).
    
          There are two types of integers:
    
          Integers ("int")
    
             These represent numbers in an unlimited range, subject to
             available (virtual) memory only.  For the purpose of shift
             and mask operations, a binary representation is assumed, and
             negative numbers are represented in a variant of 2’s
             complement which gives the illusion of an infinite string of
             sign bits extending to the left.
    
          Booleans ("bool")
             These represent the truth values False and True.  The two
             objects representing the values "False" and "True" are the
             only Boolean objects. The Boolean type is a subtype of the
             integer type, and Boolean values behave like the values 0 and
             1, respectively, in almost all contexts, the exception being
             that when converted to a string, the strings ""False"" or
             ""True"" are returned, respectively.
    
          The rules for integer representation are intended to give the
          most meaningful interpretation of shift and mask operations
          involving negative integers.
    
       "numbers.Real" ("float")
          These represent machine-level double precision floating point
          numbers. You are at the mercy of the underlying machine
          architecture (and C or Java implementation) for the accepted
          range and handling of overflow. Python does not support single-
          precision floating point numbers; the savings in processor and
          memory usage that are usually the reason for using these are
          dwarfed by the overhead of using objects in Python, so there is
          no reason to complicate the language with two kinds of floating
          point numbers.
    
       "numbers.Complex" ("complex")
          These represent complex numbers as a pair of machine-level
          double precision floating point numbers.  The same caveats apply
          as for floating point numbers. The real and imaginary parts of a
          complex number "z" can be retrieved through the read-only
          attributes "z.real" and "z.imag".
    
    Sequences
       These represent finite ordered sets indexed by non-negative
       numbers. The built-in function "len()" returns the number of items
       of a sequence. When the length of a sequence is *n*, the index set
       contains the numbers 0, 1, …, *n*-1.  Item *i* of sequence *a* is
       selected by "a[i]".
    
       Sequences also support slicing: "a[i:j]" selects all items with
       index *k* such that *i* "<=" *k* "<" *j*.  When used as an
       expression, a slice is a sequence of the same type.  This implies
       that the index set is renumbered so that it starts at 0.
    
       Some sequences also support “extended slicing” with a third “step”
       parameter: "a[i:j:k]" selects all items of *a* with index *x* where
       "x = i + n*k", *n* ">=" "0" and *i* "<=" *x* "<" *j*.
    
       Sequences are distinguished according to their mutability:
    
       Immutable sequences
          An object of an immutable sequence type cannot change once it is
          created.  (If the object contains references to other objects,
          these other objects may be mutable and may be changed; however,
          the collection of objects directly referenced by an immutable
          object cannot change.)
    
          The following types are immutable sequences:
    
          Strings
             A string is a sequence of values that represent Unicode code
             points. All the code points in the range "U+0000 - U+10FFFF"
             can be represented in a string.  Python doesn’t have a "char"
             type; instead, every code point in the string is represented
             as a string object with length "1".  The built-in function
             "ord()" converts a code point from its string form to an
             integer in the range "0 - 10FFFF"; "chr()" converts an
             integer in the range "0 - 10FFFF" to the corresponding length
             "1" string object. "str.encode()" can be used to convert a
             "str" to "bytes" using the given text encoding, and
             "bytes.decode()" can be used to achieve the opposite.
    
          Tuples
             The items of a tuple are arbitrary Python objects. Tuples of
             two or more items are formed by comma-separated lists of
             expressions.  A tuple of one item (a ‘singleton’) can be
             formed by affixing a comma to an expression (an expression by
             itself does not create a tuple, since parentheses must be
             usable for grouping of expressions).  An empty tuple can be
             formed by an empty pair of parentheses.
    
          Bytes
             A bytes object is an immutable array.  The items are 8-bit
             bytes, represented by integers in the range 0 <= x < 256.
             Bytes literals (like "b'abc'") and the built-in "bytes()"
             constructor can be used to create bytes objects.  Also, bytes
             objects can be decoded to strings via the "decode()" method.
    
       Mutable sequences
          Mutable sequences can be changed after they are created.  The
          subscription and slicing notations can be used as the target of
          assignment and "del" (delete) statements.
    
          There are currently two intrinsic mutable sequence types:
    
          Lists
             The items of a list are arbitrary Python objects.  Lists are
             formed by placing a comma-separated list of expressions in
             square brackets. (Note that there are no special cases needed
             to form lists of length 0 or 1.)
    
          Byte Arrays
             A bytearray object is a mutable array. They are created by
             the built-in "bytearray()" constructor.  Aside from being
             mutable (and hence unhashable), byte arrays otherwise provide
             the same interface and functionality as immutable "bytes"
             objects.
    
          The extension module "array" provides an additional example of a
          mutable sequence type, as does the "collections" module.
    
    Set types
       These represent unordered, finite sets of unique, immutable
       objects. As such, they cannot be indexed by any subscript. However,
       they can be iterated over, and the built-in function "len()"
       returns the number of items in a set. Common uses for sets are fast
       membership testing, removing duplicates from a sequence, and
       computing mathematical operations such as intersection, union,
       difference, and symmetric difference.
    
       For set elements, the same immutability rules apply as for
       dictionary keys. Note that numeric types obey the normal rules for
       numeric comparison: if two numbers compare equal (e.g., "1" and
       "1.0"), only one of them can be contained in a set.
    
       There are currently two intrinsic set types:
    
       Sets
          These represent a mutable set. They are created by the built-in
          "set()" constructor and can be modified afterwards by several
          methods, such as "add()".
    
       Frozen sets
          These represent an immutable set.  They are created by the
          built-in "frozenset()" constructor.  As a frozenset is immutable
          and *hashable*, it can be used again as an element of another
          set, or as a dictionary key.
    
    Mappings
       These represent finite sets of objects indexed by arbitrary index
       sets. The subscript notation "a[k]" selects the item indexed by "k"
       from the mapping "a"; this can be used in expressions and as the
       target of assignments or "del" statements. The built-in function
       "len()" returns the number of items in a mapping.
    
       There is currently a single intrinsic mapping type:
    
       Dictionaries
          These represent finite sets of objects indexed by nearly
          arbitrary values.  The only types of values not acceptable as
          keys are values containing lists or dictionaries or other
          mutable types that are compared by value rather than by object
          identity, the reason being that the efficient implementation of
          dictionaries requires a key’s hash value to remain constant.
          Numeric types used for keys obey the normal rules for numeric
          comparison: if two numbers compare equal (e.g., "1" and "1.0")
          then they can be used interchangeably to index the same
          dictionary entry.
    
          Dictionaries preserve insertion order, meaning that keys will be
          produced in the same order they were added sequentially over the
          dictionary. Replacing an existing key does not change the order,
          however removing a key and re-inserting it will add it to the
          end instead of keeping its old place.
    
          Dictionaries are mutable; they can be created by the "{...}"
          notation (see section Dictionary displays).
    
          The extension modules "dbm.ndbm" and "dbm.gnu" provide
          additional examples of mapping types, as does the "collections"
          module.
    
          Changed in version 3.7: Dictionaries did not preserve insertion
          order in versions of Python before 3.6. In CPython 3.6,
          insertion order was preserved, but it was considered an
          implementation detail at that time rather than a language
          guarantee.
    
    Callable types
       These are the types to which the function call operation (see
       section Calls) can be applied:
    
       User-defined functions
          A user-defined function object is created by a function
          definition (see section Function definitions).  It should be
          called with an argument list containing the same number of items
          as the function’s formal parameter list.
    
          Special attributes:
    
          +---------------------------+---------------------------------+-------------+
          | Attribute                 | Meaning                         |             |
          |===========================|=================================|=============|
          | "__doc__"                 | The function’s documentation    | Writable    |
          |                           | string, or "None" if            |             |
          |                           | unavailable; not inherited by   |             |
          |                           | subclasses.                     |             |
          +---------------------------+---------------------------------+-------------+
          | "__name__"                | The function’s name.            | Writable    |
          +---------------------------+---------------------------------+-------------+
          | "__qualname__"            | The function’s *qualified       | Writable    |
          |                           | name*.  New in version 3.3.     |             |
          +---------------------------+---------------------------------+-------------+
          | "__module__"              | The name of the module the      | Writable    |
          |                           | function was defined in, or     |             |
          |                           | "None" if unavailable.          |             |
          +---------------------------+---------------------------------+-------------+
          | "__defaults__"            | A tuple containing default      | Writable    |
          |                           | argument values for those       |             |
          |                           | arguments that have defaults,   |             |
          |                           | or "None" if no arguments have  |             |
          |                           | a default value.                |             |
          +---------------------------+---------------------------------+-------------+
          | "__code__"                | The code object representing    | Writable    |
          |                           | the compiled function body.     |             |
          +---------------------------+---------------------------------+-------------+
          | "__globals__"             | A reference to the dictionary   | Read-only   |
          |                           | that holds the function’s       |             |
          |                           | global variables — the global   |             |
          |                           | namespace of the module in      |             |
          |                           | which the function was defined. |             |
          +---------------------------+---------------------------------+-------------+
          | "__dict__"                | The namespace supporting        | Writable    |
          |                           | arbitrary function attributes.  |             |
          +---------------------------+---------------------------------+-------------+
          | "__closure__"             | "None" or a tuple of cells that | Read-only   |
          |                           | contain bindings for the        |             |
          |                           | function’s free variables. See  |             |
          |                           | below for information on the    |             |
          |                           | "cell_contents" attribute.      |             |
          +---------------------------+---------------------------------+-------------+
          | "__annotations__"         | A dict containing annotations   | Writable    |
          |                           | of parameters.  The keys of the |             |
          |                           | dict are the parameter names,   |             |
          |                           | and "'return'" for the return   |             |
          |                           | annotation, if provided.        |             |
          +---------------------------+---------------------------------+-------------+
          | "__kwdefaults__"          | A dict containing defaults for  | Writable    |
          |                           | keyword-only parameters.        |             |
          +---------------------------+---------------------------------+-------------+
    
          Most of the attributes labelled “Writable” check the type of the
          assigned value.
    
          Function objects also support getting and setting arbitrary
          attributes, which can be used, for example, to attach metadata
          to functions.  Regular attribute dot-notation is used to get and
          set such attributes. *Note that the current implementation only
          supports function attributes on user-defined functions. Function
          attributes on built-in functions may be supported in the
          future.*
    
          A cell object has the attribute "cell_contents". This can be
          used to get the value of the cell, as well as set the value.
    
          Additional information about a function’s definition can be
          retrieved from its code object; see the description of internal
          types below. The "cell" type can be accessed in the "types"
          module.
    
       Instance methods
          An instance method object combines a class, a class instance and
          any callable object (normally a user-defined function).
    
          Special read-only attributes: "__self__" is the class instance
          object, "__func__" is the function object; "__doc__" is the
          method’s documentation (same as "__func__.__doc__"); "__name__"
          is the method name (same as "__func__.__name__"); "__module__"
          is the name of the module the method was defined in, or "None"
          if unavailable.
    
          Methods also support accessing (but not setting) the arbitrary
          function attributes on the underlying function object.
    
          User-defined method objects may be created when getting an
          attribute of a class (perhaps via an instance of that class), if
          that attribute is a user-defined function object or a class
          method object.
    
          When an instance method object is created by retrieving a user-
          defined function object from a class via one of its instances,
          its "__self__" attribute is the instance, and the method object
          is said to be bound.  The new method’s "__func__" attribute is
          the original function object.
    
          When an instance method object is created by retrieving a class
          method object from a class or instance, its "__self__" attribute
          is the class itself, and its "__func__" attribute is the
          function object underlying the class method.
    
          When an instance method object is called, the underlying
          function ("__func__") is called, inserting the class instance
          ("__self__") in front of the argument list.  For instance, when
          "C" is a class which contains a definition for a function "f()",
          and "x" is an instance of "C", calling "x.f(1)" is equivalent to
          calling "C.f(x, 1)".
    
          When an instance method object is derived from a class method
          object, the “class instance” stored in "__self__" will actually
          be the class itself, so that calling either "x.f(1)" or "C.f(1)"
          is equivalent to calling "f(C,1)" where "f" is the underlying
          function.
    
          Note that the transformation from function object to instance
          method object happens each time the attribute is retrieved from
          the instance.  In some cases, a fruitful optimization is to
          assign the attribute to a local variable and call that local
          variable. Also notice that this transformation only happens for
          user-defined functions; other callable objects (and all non-
          callable objects) are retrieved without transformation.  It is
          also important to note that user-defined functions which are
          attributes of a class instance are not converted to bound
          methods; this *only* happens when the function is an attribute
          of the class.
    
       Generator functions
          A function or method which uses the "yield" statement (see
          section The yield statement) is called a *generator function*.
          Such a function, when called, always returns an iterator object
          which can be used to execute the body of the function:  calling
          the iterator’s "iterator.__next__()" method will cause the
          function to execute until it provides a value using the "yield"
          statement.  When the function executes a "return" statement or
          falls off the end, a "StopIteration" exception is raised and the
          iterator will have reached the end of the set of values to be
          returned.
    
       Coroutine functions
          A function or method which is defined using "async def" is
          called a *coroutine function*.  Such a function, when called,
          returns a *coroutine* object.  It may contain "await"
          expressions, as well as "async with" and "async for" statements.
          See also the Coroutine Objects section.
    
       Asynchronous generator functions
          A function or method which is defined using "async def" and
          which uses the "yield" statement is called a *asynchronous
          generator function*.  Such a function, when called, returns an
          asynchronous iterator object which can be used in an "async for"
          statement to execute the body of the function.
    
          Calling the asynchronous iterator’s "aiterator.__anext__()"
          method will return an *awaitable* which when awaited will
          execute until it provides a value using the "yield" expression.
          When the function executes an empty "return" statement or falls
          off the end, a "StopAsyncIteration" exception is raised and the
          asynchronous iterator will have reached the end of the set of
          values to be yielded.
    
       Built-in functions
          A built-in function object is a wrapper around a C function.
          Examples of built-in functions are "len()" and "math.sin()"
          ("math" is a standard built-in module). The number and type of
          the arguments are determined by the C function. Special read-
          only attributes: "__doc__" is the function’s documentation
          string, or "None" if unavailable; "__name__" is the function’s
          name; "__self__" is set to "None" (but see the next item);
          "__module__" is the name of the module the function was defined
          in or "None" if unavailable.
    
       Built-in methods
          This is really a different disguise of a built-in function, this
          time containing an object passed to the C function as an
          implicit extra argument.  An example of a built-in method is
          "alist.append()", assuming *alist* is a list object. In this
          case, the special read-only attribute "__self__" is set to the
          object denoted by *alist*.
    
       Classes
          Classes are callable.  These objects normally act as factories
          for new instances of themselves, but variations are possible for
          class types that override "__new__()".  The arguments of the
          call are passed to "__new__()" and, in the typical case, to
          "__init__()" to initialize the new instance.
    
       Class Instances
          Instances of arbitrary classes can be made callable by defining
          a "__call__()" method in their class.
    
    Modules
       Modules are a basic organizational unit of Python code, and are
       created by the import system as invoked either by the "import"
       statement, or by calling functions such as
       "importlib.import_module()" and built-in "__import__()".  A module
       object has a namespace implemented by a dictionary object (this is
       the dictionary referenced by the "__globals__" attribute of
       functions defined in the module).  Attribute references are
       translated to lookups in this dictionary, e.g., "m.x" is equivalent
       to "m.__dict__["x"]". A module object does not contain the code
       object used to initialize the module (since it isn’t needed once
       the initialization is done).
    
       Attribute assignment updates the module’s namespace dictionary,
       e.g., "m.x = 1" is equivalent to "m.__dict__["x"] = 1".
    
       Predefined (writable) attributes: "__name__" is the module’s name;
       "__doc__" is the module’s documentation string, or "None" if
       unavailable; "__annotations__" (optional) is a dictionary
       containing *variable annotations* collected during module body
       execution; "__file__" is the pathname of the file from which the
       module was loaded, if it was loaded from a file. The "__file__"
       attribute may be missing for certain types of modules, such as C
       modules that are statically linked into the interpreter; for
       extension modules loaded dynamically from a shared library, it is
       the pathname of the shared library file.
    
       Special read-only attribute: "__dict__" is the module’s namespace
       as a dictionary object.
    
       **CPython implementation detail:** Because of the way CPython
       clears module dictionaries, the module dictionary will be cleared
       when the module falls out of scope even if the dictionary still has
       live references.  To avoid this, copy the dictionary or keep the
       module around while using its dictionary directly.
    
    Custom classes
       Custom class types are typically created by class definitions (see
       section Class definitions).  A class has a namespace implemented by
       a dictionary object. Class attribute references are translated to
       lookups in this dictionary, e.g., "C.x" is translated to
       "C.__dict__["x"]" (although there are a number of hooks which allow
       for other means of locating attributes). When the attribute name is
       not found there, the attribute search continues in the base
       classes. This search of the base classes uses the C3 method
       resolution order which behaves correctly even in the presence of
       ‘diamond’ inheritance structures where there are multiple
       inheritance paths leading back to a common ancestor. Additional
       details on the C3 MRO used by Python can be found in the
       documentation accompanying the 2.3 release at
       https://www.python.org/download/releases/2.3/mro/.
    
       When a class attribute reference (for class "C", say) would yield a
       class method object, it is transformed into an instance method
       object whose "__self__" attribute is "C".  When it would yield a
       static method object, it is transformed into the object wrapped by
       the static method object. See section Implementing Descriptors for
       another way in which attributes retrieved from a class may differ
       from those actually contained in its "__dict__".
    
       Class attribute assignments update the class’s dictionary, never
       the dictionary of a base class.
    
       A class object can be called (see above) to yield a class instance
       (see below).
    
       Special attributes: "__name__" is the class name; "__module__" is
       the module name in which the class was defined; "__dict__" is the
       dictionary containing the class’s namespace; "__bases__" is a tuple
       containing the base classes, in the order of their occurrence in
       the base class list; "__doc__" is the class’s documentation string,
       or "None" if undefined; "__annotations__" (optional) is a
       dictionary containing *variable annotations* collected during class
       body execution.
    
    Class instances
       A class instance is created by calling a class object (see above).
       A class instance has a namespace implemented as a dictionary which
       is the first place in which attribute references are searched.
       When an attribute is not found there, and the instance’s class has
       an attribute by that name, the search continues with the class
       attributes.  If a class attribute is found that is a user-defined
       function object, it is transformed into an instance method object
       whose "__self__" attribute is the instance.  Static method and
       class method objects are also transformed; see above under
       “Classes”.  See section Implementing Descriptors for another way in
       which attributes of a class retrieved via its instances may differ
       from the objects actually stored in the class’s "__dict__".  If no
       class attribute is found, and the object’s class has a
       "__getattr__()" method, that is called to satisfy the lookup.
    
       Attribute assignments and deletions update the instance’s
       dictionary, never a class’s dictionary.  If the class has a
       "__setattr__()" or "__delattr__()" method, this is called instead
       of updating the instance dictionary directly.
    
       Class instances can pretend to be numbers, sequences, or mappings
       if they have methods with certain special names.  See section
       Special method names.
    
       Special attributes: "__dict__" is the attribute dictionary;
       "__class__" is the instance’s class.
    
    I/O objects (also known as file objects)
       A *file object* represents an open file.  Various shortcuts are
       available to create file objects: the "open()" built-in function,
       and also "os.popen()", "os.fdopen()", and the "makefile()" method
       of socket objects (and perhaps by other functions or methods
       provided by extension modules).
    
       The objects "sys.stdin", "sys.stdout" and "sys.stderr" are
       initialized to file objects corresponding to the interpreter’s
       standard input, output and error streams; they are all open in text
       mode and therefore follow the interface defined by the
       "io.TextIOBase" abstract class.
    
    Internal types
       A few types used internally by the interpreter are exposed to the
       user. Their definitions may change with future versions of the
       interpreter, but they are mentioned here for completeness.
    
       Code objects
          Code objects represent *byte-compiled* executable Python code,
          or *bytecode*. The difference between a code object and a
          function object is that the function object contains an explicit
          reference to the function’s globals (the module in which it was
          defined), while a code object contains no context; also the
          default argument values are stored in the function object, not
          in the code object (because they represent values calculated at
          run-time).  Unlike function objects, code objects are immutable
          and contain no references (directly or indirectly) to mutable
          objects.
    
          Special read-only attributes: "co_name" gives the function name;
          "co_argcount" is the total number of positional arguments
          (including positional-only arguments and arguments with default
          values); "co_posonlyargcount" is the number of positional-only
          arguments (including arguments with default values);
          "co_kwonlyargcount" is the number of keyword-only arguments
          (including arguments with default values); "co_nlocals" is the
          number of local variables used by the function (including
          arguments); "co_varnames" is a tuple containing the names of the
          local variables (starting with the argument names);
          "co_cellvars" is a tuple containing the names of local variables
          that are referenced by nested functions; "co_freevars" is a
          tuple containing the names of free variables; "co_code" is a
          string representing the sequence of bytecode instructions;
          "co_consts" is a tuple containing the literals used by the
          bytecode; "co_names" is a tuple containing the names used by the
          bytecode; "co_filename" is the filename from which the code was
          compiled; "co_firstlineno" is the first line number of the
          function; "co_lnotab" is a string encoding the mapping from
          bytecode offsets to line numbers (for details see the source
          code of the interpreter); "co_stacksize" is the required stack
          size; "co_flags" is an integer encoding a number of flags for
          the interpreter.
    
          The following flag bits are defined for "co_flags": bit "0x04"
          is set if the function uses the "*arguments" syntax to accept an
          arbitrary number of positional arguments; bit "0x08" is set if
          the function uses the "**keywords" syntax to accept arbitrary
          keyword arguments; bit "0x20" is set if the function is a
          generator.
    
          Future feature declarations ("from __future__ import division")
          also use bits in "co_flags" to indicate whether a code object
          was compiled with a particular feature enabled: bit "0x2000" is
          set if the function was compiled with future division enabled;
          bits "0x10" and "0x1000" were used in earlier versions of
          Python.
    
          Other bits in "co_flags" are reserved for internal use.
    
          If a code object represents a function, the first item in
          "co_consts" is the documentation string of the function, or
          "None" if undefined.
    
       Frame objects
          Frame objects represent execution frames.  They may occur in
          traceback objects (see below), and are also passed to registered
          trace functions.
    
          Special read-only attributes: "f_back" is to the previous stack
          frame (towards the caller), or "None" if this is the bottom
          stack frame; "f_code" is the code object being executed in this
          frame; "f_locals" is the dictionary used to look up local
          variables; "f_globals" is used for global variables;
          "f_builtins" is used for built-in (intrinsic) names; "f_lasti"
          gives the precise instruction (this is an index into the
          bytecode string of the code object).
    
          Special writable attributes: "f_trace", if not "None", is a
          function called for various events during code execution (this
          is used by the debugger). Normally an event is triggered for
          each new source line - this can be disabled by setting
          "f_trace_lines" to "False".
    
          Implementations *may* allow per-opcode events to be requested by
          setting "f_trace_opcodes" to "True". Note that this may lead to
          undefined interpreter behaviour if exceptions raised by the
          trace function escape to the function being traced.
    
          "f_lineno" is the current line number of the frame — writing to
          this from within a trace function jumps to the given line (only
          for the bottom-most frame).  A debugger can implement a Jump
          command (aka Set Next Statement) by writing to f_lineno.
    
          Frame objects support one method:
    
          frame.clear()
    
             This method clears all references to local variables held by
             the frame.  Also, if the frame belonged to a generator, the
             generator is finalized.  This helps break reference cycles
             involving frame objects (for example when catching an
             exception and storing its traceback for later use).
    
             "RuntimeError" is raised if the frame is currently executing.
    
             New in version 3.4.
    
       Traceback objects
          Traceback objects represent a stack trace of an exception.  A
          traceback object is implicitly created when an exception occurs,
          and may also be explicitly created by calling
          "types.TracebackType".
    
          For implicitly created tracebacks, when the search for an
          exception handler unwinds the execution stack, at each unwound
          level a traceback object is inserted in front of the current
          traceback.  When an exception handler is entered, the stack
          trace is made available to the program. (See section The try
          statement.) It is accessible as the third item of the tuple
          returned by "sys.exc_info()", and as the "__traceback__"
          attribute of the caught exception.
    
          When the program contains no suitable handler, the stack trace
          is written (nicely formatted) to the standard error stream; if
          the interpreter is interactive, it is also made available to the
          user as "sys.last_traceback".
    
          For explicitly created tracebacks, it is up to the creator of
          the traceback to determine how the "tb_next" attributes should
          be linked to form a full stack trace.
    
          Special read-only attributes: "tb_frame" points to the execution
          frame of the current level; "tb_lineno" gives the line number
          where the exception occurred; "tb_lasti" indicates the precise
          instruction. The line number and last instruction in the
          traceback may differ from the line number of its frame object if
          the exception occurred in a "try" statement with no matching
          except clause or with a finally clause.
    
          Special writable attribute: "tb_next" is the next level in the
          stack trace (towards the frame where the exception occurred), or
          "None" if there is no next level.
    
          Changed in version 3.7: Traceback objects can now be explicitly
          instantiated from Python code, and the "tb_next" attribute of
          existing instances can be updated.
    
       Slice objects
          Slice objects are used to represent slices for "__getitem__()"
          methods.  They are also created by the built-in "slice()"
          function.
    
          Special read-only attributes: "start" is the lower bound; "stop"
          is the upper bound; "step" is the step value; each is "None" if
          omitted.  These attributes can have any type.
    
          Slice objects support one method:
    
          slice.indices(self, length)
    
             This method takes a single integer argument *length* and
             computes information about the slice that the slice object
             would describe if applied to a sequence of *length* items.
             It returns a tuple of three integers; respectively these are
             the *start* and *stop* indices and the *step* or stride
             length of the slice. Missing or out-of-bounds indices are
             handled in a manner consistent with regular slices.
    
       Static method objects
          Static method objects provide a way of defeating the
          transformation of function objects to method objects described
          above. A static method object is a wrapper around any other
          object, usually a user-defined method object. When a static
          method object is retrieved from a class or a class instance, the
          object actually returned is the wrapped object, which is not
          subject to any further transformation. Static method objects are
          not themselves callable, although the objects they wrap usually
          are. Static method objects are created by the built-in
          "staticmethod()" constructor.
    
       Class method objects
          A class method object, like a static method object, is a wrapper
          around another object that alters the way in which that object
          is retrieved from classes and class instances. The behaviour of
          class method objects upon such retrieval is described above,
          under “User-defined methods”. Class method objects are created
          by the built-in "classmethod()" constructor.
    
    Related help topics: STRINGS, UNICODE, NUMBERS, SEQUENCES, MAPPINGS,
    FUNCTIONS, CLASSES, MODULES, FILES, inspect
    


.. code:: ipython3

    min, next, object, open, property




.. parsed-literal::

    (<function min>,
     <function next>,
     object,
     <function io.open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)>,
     property)



.. code:: ipython3

    True, False, None




.. parsed-literal::

    (True, False, None)



.. code:: ipython3

    a




.. parsed-literal::

    <function __main__.a(b)>



.. code:: ipython3

    assert a(42) is None

.. code:: ipython3

    set()




.. parsed-literal::

    set()



.. code:: ipython3

    type(set)




.. parsed-literal::

    type



.. code:: ipython3

    (lambda x: x + 1)(4)




.. parsed-literal::

    5



.. code:: ipython3

    (lambda x: x + 1)




.. parsed-literal::

    <function __main__.<lambda>(x)>



.. code:: ipython3

    class A(object):
        
        def __call__(self, b):
            return b + 1

.. code:: ipython3

    a = A()
    a(4)




.. parsed-literal::

    5



.. code:: ipython3

    type(A)




.. parsed-literal::

    type



.. code:: ipython3

    L = list(range(10))
    L




.. parsed-literal::

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



.. code:: ipython3

    def inc(a): return a + 1

.. code:: ipython3

    list(map(inc, L))




.. parsed-literal::

    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]



.. code:: ipython3

    list(map(print, L))


.. parsed-literal::

    0
    1
    2
    3
    4
    5
    6
    7
    8
    9




.. parsed-literal::

    [None, None, None, None, None, None, None, None, None, None]



.. code:: ipython3

    for i in L:
        print(i)


.. parsed-literal::

    0
    1
    2
    3
    4
    5
    6
    7
    8
    9


.. code:: ipython3

    LL = []
    for i in L:
        LL.append(i+1)
    LL




.. parsed-literal::

    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]



.. code:: ipython3

    an_int = 541
    my_lambda = lambda x: x + an_int

.. code:: ipython3

    my_lambda




.. parsed-literal::

    <function __main__.<lambda>(x)>



.. code:: ipython3

    my_lambda(1)




.. parsed-literal::

    542



.. code:: ipython3

    list(map(my_lambda, L))




.. parsed-literal::

    [541, 542, 543, 544, 545, 546, 547, 548, 549, 550]



Variables
---------

.. code:: ipython3

    a = 2
    b = 3
    a, b




.. parsed-literal::

    (2, 3)



.. code:: ipython3

    3 + (a := 4)




.. parsed-literal::

    7



.. code:: ipython3

    a




.. parsed-literal::

    4



.. code:: ipython3

    # the usual way...
    c = a
    a = b
    b = c

.. code:: ipython3

    b, a = a, b # swap
    a, b




.. parsed-literal::

    (3, 2)



.. code:: ipython3

    a, (b, *c, d), e, *f = (1, [2, 3, 4, 5, 6], 7, 8, 9) # unpacking of iterables.

.. code:: ipython3

    a, b, c, d, e, f




.. parsed-literal::

    (1, 2, [3, 4, 5], 6, 7, [8, 9])



.. code:: ipython3

    (1, [2, 3, 4, 5, 6], 7, 8, 9)[1][-1]




.. parsed-literal::

    6



.. code:: ipython3

    d




.. parsed-literal::

    6



Operators and operands
----------------------

Built-ins are described at
https://docs.python.org/3/library/functions.html#built-in-funcs.

.. code:: ipython3

    int(1.1)




.. parsed-literal::

    1



.. code:: ipython3

    help('INTEGER')


.. parsed-literal::

    Integer literals
    ****************
    
    Integer literals are described by the following lexical definitions:
    
       integer      ::= decinteger | bininteger | octinteger | hexinteger
       decinteger   ::= nonzerodigit (["_"] digit)* | "0"+ (["_"] "0")*
       bininteger   ::= "0" ("b" | "B") (["_"] bindigit)+
       octinteger   ::= "0" ("o" | "O") (["_"] octdigit)+
       hexinteger   ::= "0" ("x" | "X") (["_"] hexdigit)+
       nonzerodigit ::= "1"..."9"
       digit        ::= "0"..."9"
       bindigit     ::= "0" | "1"
       octdigit     ::= "0"..."7"
       hexdigit     ::= digit | "a"..."f" | "A"..."F"
    
    There is no limit for the length of integer literals apart from what
    can be stored in available memory.
    
    Underscores are ignored for determining the numeric value of the
    literal.  They can be used to group digits for enhanced readability.
    One underscore can occur between digits, and after base specifiers
    like "0x".
    
    Note that leading zeros in a non-zero decimal number are not allowed.
    This is for disambiguation with C-style octal literals, which Python
    used before version 3.0.
    
    Some examples of integer literals:
    
       7     2147483647                        0o177    0b100110111
       3     79228162514264337593543950336     0o377    0xdeadbeef
             100_000_000_000                   0b_1110_0101
    
    Changed in version 3.6: Underscores are now allowed for grouping
    purposes in literals.
    
    Related help topics: int, range
    


.. code:: ipython3

    'banana' * 3




.. parsed-literal::

    'bananabananabanana'



.. code:: ipython3

    type(_)




.. parsed-literal::

    str



.. code:: ipython3

    help('STRINGS')


.. parsed-literal::

    String and Bytes literals
    *************************
    
    String literals are described by the following lexical definitions:
    
       stringliteral   ::= [stringprefix](shortstring | longstring)
       stringprefix    ::= "r" | "u" | "R" | "U" | "f" | "F"
                        | "fr" | "Fr" | "fR" | "FR" | "rf" | "rF" | "Rf" | "RF"
       shortstring     ::= "'" shortstringitem* "'" | '"' shortstringitem* '"'
       longstring      ::= "'''" longstringitem* "'''" | '"""' longstringitem* '"""'
       shortstringitem ::= shortstringchar | stringescapeseq
       longstringitem  ::= longstringchar | stringescapeseq
       shortstringchar ::= <any source character except "\" or newline or the quote>
       longstringchar  ::= <any source character except "\">
       stringescapeseq ::= "\" <any source character>
    
       bytesliteral   ::= bytesprefix(shortbytes | longbytes)
       bytesprefix    ::= "b" | "B" | "br" | "Br" | "bR" | "BR" | "rb" | "rB" | "Rb" | "RB"
       shortbytes     ::= "'" shortbytesitem* "'" | '"' shortbytesitem* '"'
       longbytes      ::= "'''" longbytesitem* "'''" | '"""' longbytesitem* '"""'
       shortbytesitem ::= shortbyteschar | bytesescapeseq
       longbytesitem  ::= longbyteschar | bytesescapeseq
       shortbyteschar ::= <any ASCII character except "\" or newline or the quote>
       longbyteschar  ::= <any ASCII character except "\">
       bytesescapeseq ::= "\" <any ASCII character>
    
    One syntactic restriction not indicated by these productions is that
    whitespace is not allowed between the "stringprefix" or "bytesprefix"
    and the rest of the literal. The source character set is defined by
    the encoding declaration; it is UTF-8 if no encoding declaration is
    given in the source file; see section Encoding declarations.
    
    In plain English: Both types of literals can be enclosed in matching
    single quotes ("'") or double quotes (""").  They can also be enclosed
    in matching groups of three single or double quotes (these are
    generally referred to as *triple-quoted strings*).  The backslash
    ("\") character is used to escape characters that otherwise have a
    special meaning, such as newline, backslash itself, or the quote
    character.
    
    Bytes literals are always prefixed with "'b'" or "'B'"; they produce
    an instance of the "bytes" type instead of the "str" type.  They may
    only contain ASCII characters; bytes with a numeric value of 128 or
    greater must be expressed with escapes.
    
    Both string and bytes literals may optionally be prefixed with a
    letter "'r'" or "'R'"; such strings are called *raw strings* and treat
    backslashes as literal characters.  As a result, in string literals,
    "'\U'" and "'\u'" escapes in raw strings are not treated specially.
    Given that Python 2.x’s raw unicode literals behave differently than
    Python 3.x’s the "'ur'" syntax is not supported.
    
    New in version 3.3: The "'rb'" prefix of raw bytes literals has been
    added as a synonym of "'br'".
    
    New in version 3.3: Support for the unicode legacy literal
    ("u'value'") was reintroduced to simplify the maintenance of dual
    Python 2.x and 3.x codebases. See **PEP 414** for more information.
    
    A string literal with "'f'" or "'F'" in its prefix is a *formatted
    string literal*; see Formatted string literals.  The "'f'" may be
    combined with "'r'", but not with "'b'" or "'u'", therefore raw
    formatted strings are possible, but formatted bytes literals are not.
    
    In triple-quoted literals, unescaped newlines and quotes are allowed
    (and are retained), except that three unescaped quotes in a row
    terminate the literal.  (A “quote” is the character used to open the
    literal, i.e. either "'" or """.)
    
    Unless an "'r'" or "'R'" prefix is present, escape sequences in string
    and bytes literals are interpreted according to rules similar to those
    used by Standard C.  The recognized escape sequences are:
    
    +-------------------+-----------------------------------+---------+
    | Escape Sequence   | Meaning                           | Notes   |
    |===================|===================================|=========|
    | "\newline"        | Backslash and newline ignored     |         |
    +-------------------+-----------------------------------+---------+
    | "\\"              | Backslash ("\")                   |         |
    +-------------------+-----------------------------------+---------+
    | "\'"              | Single quote ("'")                |         |
    +-------------------+-----------------------------------+---------+
    | "\""              | Double quote (""")                |         |
    +-------------------+-----------------------------------+---------+
    | "\a"              | ASCII Bell (BEL)                  |         |
    +-------------------+-----------------------------------+---------+
    | "\b"              | ASCII Backspace (BS)              |         |
    +-------------------+-----------------------------------+---------+
    | "\f"              | ASCII Formfeed (FF)               |         |
    +-------------------+-----------------------------------+---------+
    | "\n"              | ASCII Linefeed (LF)               |         |
    +-------------------+-----------------------------------+---------+
    | "\r"              | ASCII Carriage Return (CR)        |         |
    +-------------------+-----------------------------------+---------+
    | "\t"              | ASCII Horizontal Tab (TAB)        |         |
    +-------------------+-----------------------------------+---------+
    | "\v"              | ASCII Vertical Tab (VT)           |         |
    +-------------------+-----------------------------------+---------+
    | "\ooo"            | Character with octal value *ooo*  | (1,3)   |
    +-------------------+-----------------------------------+---------+
    | "\xhh"            | Character with hex value *hh*     | (2,3)   |
    +-------------------+-----------------------------------+---------+
    
    Escape sequences only recognized in string literals are:
    
    +-------------------+-----------------------------------+---------+
    | Escape Sequence   | Meaning                           | Notes   |
    |===================|===================================|=========|
    | "\N{name}"        | Character named *name* in the     | (4)     |
    |                   | Unicode database                  |         |
    +-------------------+-----------------------------------+---------+
    | "\uxxxx"          | Character with 16-bit hex value   | (5)     |
    |                   | *xxxx*                            |         |
    +-------------------+-----------------------------------+---------+
    | "\Uxxxxxxxx"      | Character with 32-bit hex value   | (6)     |
    |                   | *xxxxxxxx*                        |         |
    +-------------------+-----------------------------------+---------+
    
    Notes:
    
    1. As in Standard C, up to three octal digits are accepted.
    
    2. Unlike in Standard C, exactly two hex digits are required.
    
    3. In a bytes literal, hexadecimal and octal escapes denote the byte
       with the given value. In a string literal, these escapes denote a
       Unicode character with the given value.
    
    4. Changed in version 3.3: Support for name aliases [1] has been
       added.
    
    5. Exactly four hex digits are required.
    
    6. Any Unicode character can be encoded this way.  Exactly eight hex
       digits are required.
    
    Unlike Standard C, all unrecognized escape sequences are left in the
    string unchanged, i.e., *the backslash is left in the result*.  (This
    behavior is useful when debugging: if an escape sequence is mistyped,
    the resulting output is more easily recognized as broken.)  It is also
    important to note that the escape sequences only recognized in string
    literals fall into the category of unrecognized escapes for bytes
    literals.
    
       Changed in version 3.6: Unrecognized escape sequences produce a
       "DeprecationWarning".  In a future Python version they will be a
       "SyntaxWarning" and eventually a "SyntaxError".
    
    Even in a raw literal, quotes can be escaped with a backslash, but the
    backslash remains in the result; for example, "r"\""" is a valid
    string literal consisting of two characters: a backslash and a double
    quote; "r"\"" is not a valid string literal (even a raw string cannot
    end in an odd number of backslashes).  Specifically, *a raw literal
    cannot end in a single backslash* (since the backslash would escape
    the following quote character).  Note also that a single backslash
    followed by a newline is interpreted as those two characters as part
    of the literal, *not* as a line continuation.
    
    Related help topics: str, UNICODE, SEQUENCES, STRINGMETHODS, FORMATTING,
    TYPES
    


.. code:: ipython3

    ''.join(reversed('prova')) # `string`s are collections too




.. parsed-literal::

    'avorp'



.. code:: ipython3

    a = 1; b = 2 # string interpolation
    '{} < {} is actually true.'.format(a, b)




.. parsed-literal::

    '1 < 2 is actually true.'



.. code:: ipython3

    f'{a} < {b} is actually true.'




.. parsed-literal::

    '1 < 2 is actually true.'



.. code:: ipython3

    total_secs = 43943
    hours = total_secs // 3600
    secs_still_remaining = total_secs % 3600
    minutes =  secs_still_remaining // 60
    secs_finally_remaining = secs_still_remaining  % 60
    
    "{}h {}' {}''".format(hours, minutes, secs_finally_remaining)




.. parsed-literal::

    "12h 12' 23''"



operators are described here
https://docs.python.org/3/library/operator.html?highlight=operator.

.. code:: ipython3

    help('OPERATORS')


.. parsed-literal::

    Operator precedence
    *******************
    
    The following table summarizes the operator precedence in Python, from
    lowest precedence (least binding) to highest precedence (most
    binding).  Operators in the same box have the same precedence.  Unless
    the syntax is explicitly given, operators are binary.  Operators in
    the same box group left to right (except for exponentiation, which
    groups from right to left).
    
    Note that comparisons, membership tests, and identity tests, all have
    the same precedence and have a left-to-right chaining feature as
    described in the Comparisons section.
    
    +-------------------------------------------------+---------------------------------------+
    | Operator                                        | Description                           |
    |=================================================|=======================================|
    | ":="                                            | Assignment expression                 |
    +-------------------------------------------------+---------------------------------------+
    | "lambda"                                        | Lambda expression                     |
    +-------------------------------------------------+---------------------------------------+
    | "if" – "else"                                   | Conditional expression                |
    +-------------------------------------------------+---------------------------------------+
    | "or"                                            | Boolean OR                            |
    +-------------------------------------------------+---------------------------------------+
    | "and"                                           | Boolean AND                           |
    +-------------------------------------------------+---------------------------------------+
    | "not" "x"                                       | Boolean NOT                           |
    +-------------------------------------------------+---------------------------------------+
    | "in", "not in", "is", "is not", "<", "<=", ">", | Comparisons, including membership     |
    | ">=", "!=", "=="                                | tests and identity tests              |
    +-------------------------------------------------+---------------------------------------+
    | "|"                                             | Bitwise OR                            |
    +-------------------------------------------------+---------------------------------------+
    | "^"                                             | Bitwise XOR                           |
    +-------------------------------------------------+---------------------------------------+
    | "&"                                             | Bitwise AND                           |
    +-------------------------------------------------+---------------------------------------+
    | "<<", ">>"                                      | Shifts                                |
    +-------------------------------------------------+---------------------------------------+
    | "+", "-"                                        | Addition and subtraction              |
    +-------------------------------------------------+---------------------------------------+
    | "*", "@", "/", "//", "%"                        | Multiplication, matrix                |
    |                                                 | multiplication, division, floor       |
    |                                                 | division, remainder [5]               |
    +-------------------------------------------------+---------------------------------------+
    | "+x", "-x", "~x"                                | Positive, negative, bitwise NOT       |
    +-------------------------------------------------+---------------------------------------+
    | "**"                                            | Exponentiation [6]                    |
    +-------------------------------------------------+---------------------------------------+
    | "await" "x"                                     | Await expression                      |
    +-------------------------------------------------+---------------------------------------+
    | "x[index]", "x[index:index]",                   | Subscription, slicing, call,          |
    | "x(arguments...)", "x.attribute"                | attribute reference                   |
    +-------------------------------------------------+---------------------------------------+
    | "(expressions...)",  "[expressions...]", "{key: | Binding or parenthesized expression,  |
    | value...}", "{expressions...}"                  | list display, dictionary display, set |
    |                                                 | display                               |
    +-------------------------------------------------+---------------------------------------+
    
    -[ Footnotes ]-
    
    [1] While "abs(x%y) < abs(y)" is true mathematically, for floats it
        may not be true numerically due to roundoff.  For example, and
        assuming a platform on which a Python float is an IEEE 754 double-
        precision number, in order that "-1e-100 % 1e100" have the same
        sign as "1e100", the computed result is "-1e-100 + 1e100", which
        is numerically exactly equal to "1e100".  The function
        "math.fmod()" returns a result whose sign matches the sign of the
        first argument instead, and so returns "-1e-100" in this case.
        Which approach is more appropriate depends on the application.
    
    [2] If x is very close to an exact integer multiple of y, it’s
        possible for "x//y" to be one larger than "(x-x%y)//y" due to
        rounding.  In such cases, Python returns the latter result, in
        order to preserve that "divmod(x,y)[0] * y + x % y" be very close
        to "x".
    
    [3] The Unicode standard distinguishes between *code points* (e.g.
        U+0041) and *abstract characters* (e.g. “LATIN CAPITAL LETTER A”).
        While most abstract characters in Unicode are only represented
        using one code point, there is a number of abstract characters
        that can in addition be represented using a sequence of more than
        one code point.  For example, the abstract character “LATIN
        CAPITAL LETTER C WITH CEDILLA” can be represented as a single
        *precomposed character* at code position U+00C7, or as a sequence
        of a *base character* at code position U+0043 (LATIN CAPITAL
        LETTER C), followed by a *combining character* at code position
        U+0327 (COMBINING CEDILLA).
    
        The comparison operators on strings compare at the level of
        Unicode code points. This may be counter-intuitive to humans.  For
        example, ""\u00C7" == "\u0043\u0327"" is "False", even though both
        strings represent the same abstract character “LATIN CAPITAL
        LETTER C WITH CEDILLA”.
    
        To compare strings at the level of abstract characters (that is,
        in a way intuitive to humans), use "unicodedata.normalize()".
    
    [4] Due to automatic garbage-collection, free lists, and the dynamic
        nature of descriptors, you may notice seemingly unusual behaviour
        in certain uses of the "is" operator, like those involving
        comparisons between instance methods, or constants.  Check their
        documentation for more info.
    
    [5] The "%" operator is also used for string formatting; the same
        precedence applies.
    
    [6] The power operator "**" binds less tightly than an arithmetic or
        bitwise unary operator on its right, that is, "2**-1" is "0.5".
    
    Related help topics: lambda, or, and, not, in, is, BOOLEAN, COMPARISON,
    BITWISE, SHIFTING, BINARY, FORMATTING, POWER, UNARY, ATTRIBUTES,
    SUBSCRIPTS, SLICINGS, CALLS, TUPLES, LISTS, DICTIONARIES
    


``Slice``\ s
------------

.. code:: ipython3

    'hello'[1:-2]




.. parsed-literal::

    'el'



.. code:: ipython3

    s = slice(5, 11, 2)
    s.start, s.stop, s.step




.. parsed-literal::

    (5, 11, 2)



.. code:: ipython3

    'hello'[slice(1, 3)]




.. parsed-literal::

    'el'



.. code:: ipython3

    help('SLICINGS')


.. parsed-literal::

    Slicings
    ********
    
    A slicing selects a range of items in a sequence object (e.g., a
    string, tuple or list).  Slicings may be used as expressions or as
    targets in assignment or "del" statements.  The syntax for a slicing:
    
       slicing      ::= primary "[" slice_list "]"
       slice_list   ::= slice_item ("," slice_item)* [","]
       slice_item   ::= expression | proper_slice
       proper_slice ::= [lower_bound] ":" [upper_bound] [ ":" [stride] ]
       lower_bound  ::= expression
       upper_bound  ::= expression
       stride       ::= expression
    
    There is ambiguity in the formal syntax here: anything that looks like
    an expression list also looks like a slice list, so any subscription
    can be interpreted as a slicing.  Rather than further complicating the
    syntax, this is disambiguated by defining that in this case the
    interpretation as a subscription takes priority over the
    interpretation as a slicing (this is the case if the slice list
    contains no proper slice).
    
    The semantics for a slicing are as follows.  The primary is indexed
    (using the same "__getitem__()" method as normal subscription) with a
    key that is constructed from the slice list, as follows.  If the slice
    list contains at least one comma, the key is a tuple containing the
    conversion of the slice items; otherwise, the conversion of the lone
    slice item is the key.  The conversion of a slice item that is an
    expression is that expression.  The conversion of a proper slice is a
    slice object (see section The standard type hierarchy) whose "start",
    "stop" and "step" attributes are the values of the expressions given
    as lower bound, upper bound and stride, respectively, substituting
    "None" for missing expressions.
    
    Related help topics: SEQUENCEMETHODS
    


``range``\ s
------------

.. code:: ipython3

    range




.. parsed-literal::

    range



.. code:: ipython3

    type(range)




.. parsed-literal::

    type



.. code:: ipython3

    r = range(2, 20)
    r




.. parsed-literal::

    range(2, 20)



.. code:: ipython3

    len(r)




.. parsed-literal::

    18



.. code:: ipython3

    id(_)




.. parsed-literal::

    4389733408



.. code:: ipython3

    print(range.__doc__)


.. parsed-literal::

    range(stop) -> range object
    range(start, stop[, step]) -> range object
    
    Return an object that produces a sequence of integers from start (inclusive)
    to stop (exclusive) by step.  range(i, j) produces i, i+1, i+2, ..., j-1.
    start defaults to 0, and stop is omitted!  range(4) produces 0, 1, 2, 3.
    These are exactly the valid indices for a list of 4 elements.
    When step is given, it specifies the increment (or decrement).


.. code:: ipython3

    range(1_000_000_000_000_000)




.. parsed-literal::

    range(0, 1000000000000000)



.. code:: ipython3

    range(1_000000000_000_000)




.. parsed-literal::

    range(0, 1000000000000000)



.. code:: ipython3

    type(_)




.. parsed-literal::

    range



.. code:: ipython3

    list(range(10))




.. parsed-literal::

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



.. code:: ipython3

    list()




.. parsed-literal::

    []



.. code:: ipython3

    []




.. parsed-literal::

    []



.. code:: ipython3

    set()




.. parsed-literal::

    set()



.. code:: ipython3

    ranges = [range(10), range(10, 20)]

.. code:: ipython3

    list(map(list, ranges))




.. parsed-literal::

    [[0, 1, 2, 3, 4, 5, 6, 7, 8, 9], [10, 11, 12, 13, 14, 15, 16, 17, 18, 19]]



.. code:: ipython3

    my_map = map(list, ranges)

In Haskell, we can define the naturals using the following definition:

.. code:: haskell

   nats = [1..]

.. code:: ipython3

    type(my_map)




.. parsed-literal::

    map



.. code:: ipython3

    list(my_map)




.. parsed-literal::

    [[0, 1, 2, 3, 4, 5, 6, 7, 8, 9], [10, 11, 12, 13, 14, 15, 16, 17, 18, 19]]



--------------

.. code:: ipython3

    L = range(20000000000000000000000000000000000000)
    L




.. parsed-literal::

    range(0, 20000000000000000000000000000000000000)



.. code:: ipython3

    M = map(lambda x: x % 2, L)
    M




.. parsed-literal::

    <map at 0x1054d7d90>



.. code:: ipython3

    next(M)




.. parsed-literal::

    0



.. code:: ipython3

    next(M)




.. parsed-literal::

    1



.. code:: ipython3

    next(M)




.. parsed-literal::

    0



.. code:: ipython3

    next(M)




.. parsed-literal::

    1



.. code:: ipython3

    def one_two_three():
        yield 1
        yield 2
        yield 3

.. code:: ipython3

    one_two_three




.. parsed-literal::

    <function __main__.one_two_three()>



.. code:: ipython3

    OTT = one_two_three()

.. code:: ipython3

    id(OTT)




.. parsed-literal::

    4391761872



.. code:: ipython3

    type(OTT)




.. parsed-literal::

    generator



.. code:: ipython3

    next(OTT)




.. parsed-literal::

    1



.. code:: ipython3

    next(OTT)




.. parsed-literal::

    2



.. code:: ipython3

    next(OTT)




.. parsed-literal::

    3



.. code:: ipython3

    next(OTT)


::


    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    /var/folders/0k/y3r96gt12zn1mzgztql9m_8m0000gn/T/ipykernel_1055/2932888578.py in <module>
    ----> 1 next(OTT)
    

    StopIteration: 


.. code:: ipython3

    for i in one_two_three():
        print(i)


.. parsed-literal::

    1
    2
    3


.. code:: ipython3

    M = one_two_three()
    while True:
        try:
            i = next(M)
            print(i)
        except StopIteration:
            break


.. parsed-literal::

    1
    2
    3


.. code:: ipython3

    L = range(10)
    L




.. parsed-literal::

    range(0, 10)



.. code:: ipython3

    def f(x):
        print('From f')
        return x + 1
    
    M = map(f, L)

.. code:: ipython3

    type(M)




.. parsed-literal::

    map



.. code:: ipython3

    def g(x):
        print('From g')
        return x + 2
    
    N = map(g, M)

.. code:: ipython3

    type(N)




.. parsed-literal::

    map



.. code:: ipython3

    def h(x):
        print('From h')
        return x + 3
    
    O = map(h, N)

.. code:: ipython3

    type(O)




.. parsed-literal::

    map



.. code:: ipython3

    next(O)


.. parsed-literal::

    From f
    From g
    From h




.. parsed-literal::

    6



.. code:: ipython3

    next(O)


.. parsed-literal::

    From f
    From g
    From h




.. parsed-literal::

    7



.. code:: ipython3

    list(O)


.. parsed-literal::

    From f
    From g
    From h
    From f
    From g
    From h
    From f
    From g
    From h
    From f
    From g
    From h
    From f
    From g
    From h
    From f
    From g
    From h
    From f
    From g
    From h
    From f
    From g
    From h




.. parsed-literal::

    [8, 9, 10, 11, 12, 13, 14, 15]



.. code:: ipython3

    next()


::


    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    /var/folders/0k/y3r96gt12zn1mzgztql9m_8m0000gn/T/ipykernel_1055/3089814063.py in <module>
    ----> 1 next(O)
    

    StopIteration: 


.. code:: ipython3

    M = [1, 2, 3, 4]

.. code:: ipython3

    G = (x + 1 for x in range(100))

.. code:: ipython3

    H = [x + 1 for x in M]
    H




.. parsed-literal::

    [2, 3, 4, 5]



.. code:: ipython3

    list(G)




.. parsed-literal::

    [2, 3, 4, 5]



.. code:: ipython3

    list(map(lambda x: x+1, M))




.. parsed-literal::

    [2, 3, 4, 5]



``lambda``\ s
-------------

.. code:: ipython3

    def add(a, b):
        return a + b

.. code:: ipython3

    type(add)




.. parsed-literal::

    function



.. code:: ipython3

    callable(add)




.. parsed-literal::

    True



.. code:: ipython3

    add(3, 4)




.. parsed-literal::

    7



.. code:: ipython3

    help('FUNCTIONS')


.. parsed-literal::

    Functions
    *********
    
    Function objects are created by function definitions.  The only
    operation on a function object is to call it: "func(argument-list)".
    
    There are really two flavors of function objects: built-in functions
    and user-defined functions.  Both support the same operation (to call
    the function), but the implementation is different, hence the
    different object types.
    
    See Function definitions for more information.
    
    Related help topics: def, TYPES
    


.. code:: ipython3

    add_l = lambda a, b: a + b # is a lambda expression

.. code:: ipython3

    type(add_l)




.. parsed-literal::

    function



.. code:: ipython3

    callable(add_l)




.. parsed-literal::

    True



.. code:: ipython3

    help('lambda')


.. parsed-literal::

    Lambdas
    *******
    
       lambda_expr        ::= "lambda" [parameter_list] ":" expression
       lambda_expr_nocond ::= "lambda" [parameter_list] ":" expression_nocond
    
    Lambda expressions (sometimes called lambda forms) are used to create
    anonymous functions. The expression "lambda parameters: expression"
    yields a function object.  The unnamed object behaves like a function
    object defined with:
    
       def <lambda>(parameters):
           return expression
    
    See section Function definitions for the syntax of parameter lists.
    Note that functions created with lambda expressions cannot contain
    statements or annotations.
    
    Related help topics: FUNCTIONS
    


.. code:: ipython3

    assert add(1, 2) == add_l(1, 2)

``yield``\ s
------------

.. code:: ipython3

    L = range(5)
    L




.. parsed-literal::

    range(0, 5)



.. code:: ipython3

    M = map(lambda i: i + 1, L)
    M




.. parsed-literal::

    <map at 0x7fb7c6626880>



.. code:: ipython3

    type(_)




.. parsed-literal::

    map



.. code:: ipython3

    next(M)




.. parsed-literal::

    1



.. code:: ipython3

    help(next)


.. parsed-literal::

    Help on built-in function next in module builtins:
    
    next(...)
        next(iterator[, default])
        
        Return the next item from the iterator. If default is given and the iterator
        is exhausted, it is returned instead of raising StopIteration.
    


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


::


    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    <ipython-input-10-0666361e9047> in <module>
    ----> 1 next(M)
    

    StopIteration: 


.. code:: ipython3

    help(StopIterationx)

**INTERMEZZO**

.. code:: ipython3

    1/0


::


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-11-9e1622b385b6> in <module>
    ----> 1 1/0
    

    ZeroDivisionError: division by zero


.. code:: ipython3

    try:
        1/0
    except ZeroDivisionError:
        pass

--------------

.. math::


       \mathcal{B} = \lbrace x\in\mathbb{R} : \sqrt{x + 1} > 4 \rbrace

.. code:: ipython3

    N = ( i for i in range(1, 10) ) # another way to build a generator
    N




.. parsed-literal::

    <generator object <genexpr> at 0x7fb7c687c120>



.. code:: ipython3

    type(N)




.. parsed-literal::

    generator



.. code:: ipython3

    list(N)




.. parsed-literal::

    [1, 2, 3, 4, 5, 6, 7, 8, 9]



.. code:: ipython3

    def saturate(g, L=[]):
        while True:
            try:
                L.append(next(g))
            except StopIteration:
                break
        return L

.. code:: ipython3

    saturate(N, [0])




.. parsed-literal::

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



Help about ``help``
-------------------

.. code:: ipython3

    help()


.. parsed-literal::

    
    Welcome to Python 3.8's help utility!
    
    If this is your first time using Python, you should definitely check out
    the tutorial on the Internet at https://docs.python.org/3.8/tutorial/.
    
    Enter the name of any module, keyword, or topic to get help on writing
    Python programs and using Python modules.  To quit this help utility and
    return to the interpreter, just type "quit".
    
    To get a list of available modules, keywords, symbols, or topics, type
    "modules", "keywords", "symbols", or "topics".  Each module also comes
    with a one-line summary of what it does; to list the modules whose name
    or summary contain a given string such as "spam", type "modules spam".
    
    help> map
    Help on class map in module builtins:
    
    class map(object)
     |  map(func, *iterables) --> map object
     |  
     |  Make an iterator that computes the function using arguments from
     |  each of the iterables.  Stops when the shortest iterable is exhausted.
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
     |  ----------------------------------------------------------------------
     |  Static methods defined here:
     |  
     |  __new__(*args, **kwargs) from builtins.type
     |      Create and return a new object.  See help(type) for accurate signature.
    
    
    You are now leaving help and returning to the Python interpreter.
    If you want to ask for help on a particular object directly from the
    interpreter, you can type "help(object)".  Executing "help('string')"
    has the same effect as typing a particular string at the help> prompt.

