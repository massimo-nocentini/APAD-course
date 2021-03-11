Jupyter notebooks
#################


IPython: Beyond Normal Python
=============================

There are many options for development environments for Python, and I’m
often asked which one I use in my own work. My answer sometimes
surprises people: my preferred environment is
`IPython <http://ipython.org/>`__ plus a text editor (in my case, Emacs
or Atom depending on my mood). IPython (short for *Interactive Python*)
was started in 2001 by Fernando Perez as an enhanced Python interpreter,
and has since grown into a project aiming to provide, in Perez’s words,
“Tools for the entire life cycle of research computing.” If Python is
the engine of our data science task, you might think of IPython as the
interactive control panel.

As well as being a useful interactive interface to Python, IPython also
provides a number of useful syntactic additions to the language; we’ll
cover the most useful of these additions here. In addition, IPython is
closely tied with the `Jupyter project <http://jupyter.org>`__, which
provides a browser-based notebook that is useful for development,
collaboration, sharing, and even publication of data science results.
The IPython notebook is actually a special case of the broader Jupyter
notebook structure, which encompasses notebooks for Julia, R, and other
programming languages. As an example of the usefulness of the notebook
format, look no further than the page you are reading: the entire
manuscript for this book was composed as a set of IPython notebooks.

IPython is about using Python effectively for interactive scientific and
data-intensive computing. This chapter will start by stepping through
some of the IPython features that are useful to the practice of data
science, focusing especially on the syntax it offers beyond the standard
features of Python. Next, we will go into a bit more depth on some of
the more useful “magic commands” that can speed-up common tasks in
creating and using data science code. Finally, we will touch on some of
the features of the notebook that make it useful in understanding data
and sharing results.

Shell or Notebook?
------------------

There are two primary means of using IPython that we’ll discuss in this
chapter: the IPython shell and the IPython notebook. The bulk of the
material in this chapter is relevant to both, and the examples will
switch between them depending on what is most convenient. In the few
sections that are relevant to just one or the other, we will explicitly
state that fact. Before we start, some words on how to launch the
IPython shell and IPython notebook.

Launching the IPython Shell
~~~~~~~~~~~~~~~~~~~~~~~~~~~

This chapter, like most of this book, is not designed to be absorbed
passively. I recommend that as you read through it, you follow along and
experiment with the tools and syntax we cover: the muscle-memory you
build through doing this will be far more useful than the simple act of
reading about it. Start by launching the IPython interpreter by typing
**``ipython``** on the command-line; alternatively, if you’ve installed
a distribution like Anaconda or EPD, there may be a launcher specific to
your system (we’ll discuss this more fully in `Help and Documentation in
IPython <01.01-Help-And-Documentation.ipynb>`__).

Once you do this, you should see a prompt like the following:

::

   IPython 4.0.1 -- An enhanced Interactive Python.
   ?         -> Introduction and overview of IPython's features.
   %quickref -> Quick reference.
   help      -> Python's own help system.
   object?   -> Details about 'object', use 'object??' for extra details.
   In [1]:

With that, you’re ready to follow along.

Launching the Jupyter Notebook
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Jupyter notebook is a browser-based graphical interface to the
IPython shell, and builds on it a rich set of dynamic display
capabilities. As well as executing Python/IPython statements, the
notebook allows the user to include formatted text, static and dynamic
visualizations, mathematical equations, JavaScript widgets, and much
more. Furthermore, these documents can be saved in a way that lets other
people open them and execute the code on their own systems.

Though the IPython notebook is viewed and edited through your web
browser window, it must connect to a running Python process in order to
execute code. This process (known as a “kernel”) can be started by
running the following command in your system shell:

::

   $ jupyter notebook

This command will launch a local web server that will be visible to your
browser. It immediately spits out a log showing what it is doing; that
log will look something like this:

::

   $ jupyter notebook
   [NotebookApp] Serving notebooks from local directory: /Users/jakevdp/PythonDataScienceHandbook
   [NotebookApp] 0 active kernels 
   [NotebookApp] The IPython Notebook is running at: http://localhost:8888/
   [NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).

Upon issuing the command, your default browser should automatically open
and navigate to the listed local URL; the exact address will depend on
your system. If the browser does not open automatically, you can open a
window and manually open this address (*http://localhost:8888/* in this
example).

Help and Documentation in IPython
=================================

If you read no other section in this chapter, read this one: I find the
tools discussed here to be the most transformative contributions of
IPython to my daily workflow.

When a technologically-minded person is asked to help a friend, family
member, or colleague with a computer problem, most of the time it’s less
a matter of knowing the answer as much as knowing how to quickly find an
unknown answer.

In data science it’s the same: searchable web resources such as online
documentation, mailing-list threads, and StackOverflow answers contain a
wealth of information, even (especially?) if it is a topic you’ve found
yourself searching before.

*Being an effective practitioner of data science is less about
memorizing the tool or command you should use for every possible
situation, and more about learning to effectively find the information
you don’t know, whether through a web search engine or another means.*

One of the most useful functions of IPython/Jupyter is to shorten the
gap between the user and the type of documentation and search that will
help them do their work effectively.

While web searches still play a role in answering complicated questions,
an amazing amount of information can be found through IPython alone.
Some examples of the questions IPython can help answer in a few
keystrokes:

-  How do I call this function? What arguments and options does it have?
-  What does the source code of this Python object look like?
-  What is in this package I imported? What attributes or methods does
   this object have?

Here we’ll discuss IPython’s tools to quickly access this information,
namely the ``?`` character to explore documentation, the ``??``
characters to explore source code, and the Tab key for auto-completion.

Accessing Documentation with ``?``
----------------------------------

The Python language and its data science ecosystem is built with the
user in mind, and one big part of that is access to documentation. Every
Python object contains the reference to a string, known as a *doc
string*, which in most cases will contain a concise summary of the
object and how to use it. Python has a built-in ``help()`` function that
can access this information and prints the results. For example, to see
the documentation of the built-in ``len`` function, you can do the
following:

.. code:: ipython3

    help(len)


.. parsed-literal::

    Help on built-in function len in module builtins:
    
    len(obj, /)
        Return the number of items in a container.
    


This notation works for just about anything, including object methods:

.. code:: ipython3

    L = [1, 2, 3]
    help(L.insert)


.. parsed-literal::

    Help on built-in function insert:
    
    insert(index, object, /) method of builtins.list instance
        Insert object before index.
    


or even objects themselves, with the documentation from their type:

.. code:: ipython3

    help(L)


.. parsed-literal::

    Help on list object:
    
    class list(object)
     |  list(iterable=(), /)
     |  
     |  Built-in mutable sequence.
     |  
     |  If no argument is given, the constructor creates a new empty list.
     |  The argument must be an iterable if specified.
     |  
     |  Methods defined here:
     |  
     |  __add__(self, value, /)
     |      Return self+value.
     |  
     |  __contains__(self, key, /)
     |      Return key in self.
     |  
     |  __delitem__(self, key, /)
     |      Delete self[key].
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
     |  __getitem__(...)
     |      x.__getitem__(y) <==> x[y]
     |  
     |  __gt__(self, value, /)
     |      Return self>value.
     |  
     |  __iadd__(self, value, /)
     |      Implement self+=value.
     |  
     |  __imul__(self, value, /)
     |      Implement self*=value.
     |  
     |  __init__(self, /, *args, **kwargs)
     |      Initialize self.  See help(type(self)) for accurate signature.
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
     |  __reversed__(self, /)
     |      Return a reverse iterator over the list.
     |  
     |  __rmul__(self, value, /)
     |      Return value*self.
     |  
     |  __setitem__(self, key, value, /)
     |      Set self[key] to value.
     |  
     |  __sizeof__(self, /)
     |      Return the size of the list in memory, in bytes.
     |  
     |  append(self, object, /)
     |      Append object to the end of the list.
     |  
     |  clear(self, /)
     |      Remove all items from list.
     |  
     |  copy(self, /)
     |      Return a shallow copy of the list.
     |  
     |  count(self, value, /)
     |      Return number of occurrences of value.
     |  
     |  extend(self, iterable, /)
     |      Extend list by appending elements from the iterable.
     |  
     |  index(self, value, start=0, stop=9223372036854775807, /)
     |      Return first index of value.
     |      
     |      Raises ValueError if the value is not present.
     |  
     |  insert(self, index, object, /)
     |      Insert object before index.
     |  
     |  pop(self, index=-1, /)
     |      Remove and return item at index (default last).
     |      
     |      Raises IndexError if list is empty or index is out of range.
     |  
     |  remove(self, value, /)
     |      Remove first occurrence of value.
     |      
     |      Raises ValueError if the value is not present.
     |  
     |  reverse(self, /)
     |      Reverse *IN PLACE*.
     |  
     |  sort(self, /, *, key=None, reverse=False)
     |      Sort the list in ascending order and return None.
     |      
     |      The sort is in-place (i.e. the list itself is modified) and stable (i.e. the
     |      order of two equal elements is maintained).
     |      
     |      If a key function is given, apply it once to each list item and sort them,
     |      ascending or descending, according to their function values.
     |      
     |      The reverse flag can be set to sort in descending order.
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
     |  
     |  ----------------------------------------------------------------------
     |  Data and other attributes defined here:
     |  
     |  __hash__ = None
    


Importantly, this will even work for functions or other objects you
create yourself! Here we’ll define a small function with a docstring:

.. code:: ipython3

    def square(a):
        """Return the square of a."""
        return a ** 2

Note that to create a docstring for our function, we simply placed a
string literal in the first line. Because doc strings are usually
multiple lines, by convention we used Python’s triple-quote notation for
multi-line strings.

.. code:: ipython3

    help(square)


.. parsed-literal::

    Help on function square in module __main__:
    
    square(a)
        Return the square of a.
    


This quick access to documentation via docstrings is one reason you
should get in the habit of always adding such inline documentation to
the code you write!

Accessing Source Code with ``??``
---------------------------------

Because the Python language is so easily readable, another level of
insight can usually be gained by reading the source code of the object
you’re curious about. IPython provides a shortcut to the source code
with the double question mark (``??``):

.. code:: ipython

   In [8]: square??
   Type:        function
   String form: <function square at 0x103713cb0>
   Definition:  square(a)
   Source:
   def square(a):
       "Return the square of a"
       return a ** 2

For simple functions like this, the double question-mark can give quick
insight into the under-the-hood details.

If you play with this much, you’ll notice that sometimes the ``??``
suffix doesn’t display any source code: this is generally because the
object in question is not implemented in Python, but in C or some other
compiled extension language. If this is the case, the ``??`` suffix
gives the same output as the ``?`` suffix. You’ll find this particularly
with many of Python’s built-in objects and types, for example ``len``
from above:

.. code:: ipython

   In [9]: len??
   Type:        builtin_function_or_method
   String form: <built-in function len>
   Namespace:   Python builtin
   Docstring:
   len(object) -> integer

   Return the number of items of a sequence or mapping.

Using ``?`` and/or ``??`` gives a powerful and quick interface for
finding information about what any Python function or module does.

Exploring Modules with Tab-Completion
-------------------------------------

IPython’s other useful interface is the use of the tab key for
auto-completion and exploration of the contents of objects, modules, and
name-spaces. In the examples that follow, we’ll use ``<TAB>`` to
indicate when the Tab key should be pressed.

Tab-completion of object contents
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Every Python object has various attributes and methods associated with
it. Like with the ``help`` function discussed before, Python has a
built-in ``dir`` function that returns a list of these, but the
tab-completion interface is much easier to use in practice. To see a
list of all available attributes of an object, you can type the name of
the object followed by a period (“``.``”) character and the Tab key:

.. code:: ipython

   In [10]: L.<TAB>
   L.append   L.copy     L.extend   L.insert   L.remove   L.sort     
   L.clear    L.count    L.index    L.pop      L.reverse  

To narrow-down the list, you can type the first character or several
characters of the name, and the Tab key will find the matching
attributes and methods:

.. code:: ipython

   In [10]: L.c<TAB>
   L.clear  L.copy   L.count  

   In [10]: L.co<TAB>
   L.copy   L.count 

If there is only a single option, pressing the Tab key will complete the
line for you. For example, the following will instantly be replaced with
``L.count``:

.. code:: ipython

   In [10]: L.cou<TAB>

Though Python has no strictly-enforced distinction between
public/external attributes and private/internal attributes, by
convention a preceding underscore is used to denote such methods. For
clarity, these private methods and special methods are omitted from the
list by default, but it’s possible to list them by explicitly typing the
underscore:

.. code:: ipython

   In [10]: L._<TAB>
   L.__add__           L.__gt__            L.__reduce__
   L.__class__         L.__hash__          L.__reduce_ex__

For brevity, we’ve only shown the first couple lines of the output. Most
of these are Python’s special double-underscore methods (often nicknamed
“dunder” methods).

Tab completion when importing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Tab completion is also useful when importing objects from packages. Here
we’ll use it to find all possible imports in the ``itertools`` package
that start with ``co``:

::

   In [10]: from itertools import co<TAB>
   combinations                   compress
   combinations_with_replacement  count

Similarly, you can use tab-completion to see which imports are available
on your system (this will change depending on which third-party scripts
and modules are visible to your Python session):

::

   In [10]: import <TAB>
   Display all 399 possibilities? (y or n)
   Crypto              dis                 py_compile
   Cython              distutils           pyclbr
   ...                 ...                 ...
   difflib             pwd                 zmq

   In [10]: import h<TAB>
   hashlib             hmac                http         
   heapq               html                husl         

(Note that for brevity, I did not print here all 399 importable packages
and modules on my system.)

Beyond tab completion: wildcard matching
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Tab completion is useful if you know the first few characters of the
object or attribute you’re looking for, but is little help if you’d like
to match characters at the middle or end of the word. For this use-case,
IPython provides a means of wildcard matching for names using the ``*``
character.

For example, we can use this to list every object in the namespace that
ends with ``Warning``:

.. code:: ipython

   In [10]: *Warning?
   BytesWarning                  RuntimeWarning
   DeprecationWarning            SyntaxWarning
   FutureWarning                 UnicodeWarning
   ImportWarning                 UserWarning
   PendingDeprecationWarning     Warning
   ResourceWarning

Notice that the ``*`` character matches any string, including the empty
string.

Similarly, suppose we are looking for a string method that contains the
word ``find`` somewhere in its name. We can search for it this way:

.. code:: ipython

   In [10]: str.*find*?
   str.find
   str.rfind

I find this type of flexible wildcard search can be very useful for
finding a particular command when getting to know a new package or
reacquainting myself with a familiar one.

IPython Magic Commands
======================

The previous two sections showed how IPython lets you use and explore
Python efficiently and interactively. Here we’ll begin discussing some
of the enhancements that IPython adds on top of the normal Python
syntax. These are known in IPython as *magic commands*, and are prefixed
by the ``%`` character. These magic commands are designed to succinctly
solve various common problems in standard data analysis. Magic commands
come in two flavors: *line magics*, which are denoted by a single ``%``
prefix and operate on a single line of input, and *cell magics*, which
are denoted by a double ``%%`` prefix and operate on multiple lines of
input. We’ll demonstrate and discuss a few brief examples here, and come
back to more focused discussion of several useful magic commands later
in the chapter.

Running External Code: ``%run``
-------------------------------

As you begin developing more extensive code, you will likely find
yourself working in both IPython for interactive exploration, as well as
a text editor to store code that you want to reuse. Rather than running
this code in a new window, it can be convenient to run it within your
IPython session. This can be done with the ``%run`` magic.

For example, imagine you’ve created a ``myscript.py`` file with the
following contents:

.. code:: bash

    %%bash
    
    cat my-script.py


.. parsed-literal::

    
    def square(x):
        """square a number"""
        return x ** 2
    
    for N in range(1, 4):
        print(N, "squared is", square(N))


You can execute this from your IPython session as follows:

.. code:: ipython3

    %run my-script.py


.. parsed-literal::

    1 squared is 1
    2 squared is 4
    3 squared is 9


Note also that after you’ve run this script, any functions defined
within it are available for use in your IPython session:

.. code:: ipython3

    square(5)




.. parsed-literal::

    25



There are several options to fine-tune how your code is run; you can see
the documentation in the normal way, by typing **``%run?``** in the
IPython interpreter.

Timing Code Execution: ``%timeit``
----------------------------------

Another example of a useful magic function is ``%timeit``, which will
automatically determine the execution time of the single-line Python
statement that follows it. For example, we may want to check the
performance of a list comprehension:

.. code:: ipython3

    %timeit L = [n ** 2 for n in range(1000)]


.. parsed-literal::

    277 µs ± 4.2 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)


The benefit of ``%timeit`` is that for short commands it will
automatically perform multiple runs in order to attain more robust
results.

For multi line statements, adding a second ``%`` sign will turn this
into a cell magic that can handle multiple lines of input. For example,
here’s the equivalent construction with a ``for``-loop:

.. code:: ipython3

    %%timeit
    L = []
    for n in range(1000):
        L.append(n ** 2)


.. parsed-literal::

    299 µs ± 3.05 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)


We can immediately see that list comprehensions are about 10% faster
than the equivalent ``for``-loop construction in this case.

Help on Magic Functions: ``?``, ``%magic``, and ``%lsmagic``
------------------------------------------------------------

Like normal Python functions, IPython magic functions have docstrings,
and this useful documentation can be accessed in the standard manner.
So, for example, to read the documentation of the ``%timeit`` magic
simply type this:

.. code:: ipython

   In [10]: %timeit?

Documentation for other functions can be accessed similarly. To access a
general description of available magic functions, including some
examples, you can type this:

.. code:: ipython

   In [11]: %magic

For a quick and simple list of all available magic functions, type this:

.. code:: ipython

   In [12]: %lsmagic

Finally, I’ll mention that it is quite straightforward to define your
own magic functions if you wish.

Input and Output History
========================

Previously we saw that the IPython shell allows you to access previous
commands with the up and down arrow keys, or equivalently the
Ctrl-p/Ctrl-n shortcuts. Additionally, in both the shell and the
notebook, IPython exposes several ways to obtain the output of previous
commands, as well as string versions of the commands themselves. We’ll
explore those here.

IPython’s ``In`` and ``Out`` Objects
------------------------------------

By now I imagine you’re quite familiar with the ``In [1]:``/``Out[1]:``
style prompts used by IPython. But it turns out that these are not just
pretty decoration: they give a clue as to how you can access previous
inputs and outputs in your current session. Imagine you start a session
that looks like this:

.. code:: ipython3

    import math
    
    math.sin(2)




.. parsed-literal::

    0.9092974268256817



.. code:: ipython3

    math.cos(2)




.. parsed-literal::

    -0.4161468365471424



We’ve imported the built-in ``math`` package, then computed the sine and
the cosine of the number 2. These inputs and outputs are displayed in
the shell with ``In``/``Out`` labels, but there’s more–IPython actually
creates some Python variables called ``In`` and ``Out`` that are
automatically updated to reflect this history:

.. code:: ipython3

    print(str(In)[:1000])


.. parsed-literal::

    ['', 'help(len)', 'L = [1, 2, 3]\nhelp(L.insert)', 'help(L)', 'def square(a):\n    """Return the square of a."""\n    return a ** 2', 'help(square)', "get_ipython().run_cell_magic('bash', '', '\\ncat my-script.py\\n')", "get_ipython().run_line_magic('run', 'my-script.py')", 'square(5)', "get_ipython().run_line_magic('timeit', 'L = [n ** 2 for n in range(1000)]')", "get_ipython().run_cell_magic('timeit', '', 'L = []\\nfor n in range(1000):\\n    L.append(n ** 2)\\n')", 'import math\n\nmath.sin(2)', 'math.cos(2)', 'print(str(In)[:1000])']


.. code:: ipython3

    print(str(Out)[:1000])


.. parsed-literal::

    {8: 25, 11: 0.9092974268256817, 12: -0.4161468365471424}


The ``In`` object is a list, which keeps track of the commands in order
(the first item in the list is a place-holder so that ``In[1]`` can
refer to the first command):

.. code:: ipython3

    print(In[1])
    import math


.. parsed-literal::

    help(len)


The ``Out`` object is not a list but a dictionary mapping input numbers
to their outputs (if any):

.. code:: ipython3

    print(Out)


.. parsed-literal::

    {8: 25, 11: 0.9092974268256817, 12: -0.4161468365471424}


Note that not all operations have outputs: for example, ``import``
statements and ``print`` statements don’t affect the output. The latter
may be surprising, but makes sense if you consider that ``print`` is a
function that returns ``None``; for brevity, any command that returns
``None`` is not added to ``Out``.

Where this can be useful is if you want to interact with past results.
For example, let’s check the sum of ``sin(2) ** 2`` and ``cos(2) ** 2``
using the previously-computed results:

.. code:: ipython3

    Out[8] ** 2 + Out[12] ** 2




.. parsed-literal::

    625.1731781895681



The result is ``1.0`` as we’d expect from the well-known trigonometric
identity. In this case, using these previous results probably is not
necessary, but it can become very handy if you execute a very expensive
computation and want to reuse the result!

Underscore Shortcuts and Previous Outputs
-----------------------------------------

The standard Python shell contains just one simple shortcut for
accessing previous output; the variable ``_`` (i.e., a single
underscore) is kept updated with the previous output; this works in
IPython as well:

.. code:: ipython3

    print(_)


.. parsed-literal::

    625.1731781895681


But IPython takes this a bit further—you can use a double underscore to
access the second-to-last output, and a triple underscore to access the
third-to-last output (skipping any commands with no output):

.. code:: ipython3

    print(__)
    
    print(___)



.. parsed-literal::

    -0.4161468365471424
    0.9092974268256817


IPython stops there: more than three underscores starts to get a bit
hard to count, and at that point it’s easier to refer to the output by
line number.

There is one more shortcut we should mention, however–a shorthand for
``Out[X]`` is ``_X`` (i.e., a single underscore followed by the line
number):

.. code:: ipython3

    Out




.. parsed-literal::

    {8: 25, 11: 0.9092974268256817, 12: -0.4161468365471424, 17: 625.1731781895681}



Suppressing Output
------------------

Sometimes you might wish to suppress the output of a statement (this is
perhaps most common with the plotting commands that we’ll explore in
`Introduction to
Matplotlib <04.00-Introduction-To-Matplotlib.ipynb>`__). Or maybe the
command you’re executing produces a result that you’d prefer not like to
store in your output history, perhaps so that it can be deallocated when
other references are removed. The easiest way to suppress the output of
a command is to add a semicolon to the end omf the line:

.. code:: ipython3

    math.sin(2) + math.cos(2);

Note that the result is computed silently, and the output is neither
displayed on the screen or stored in the ``Out`` dictionary:

.. code:: ipython3

    14 in Out




.. parsed-literal::

    False



Related Magic Commands
----------------------

For accessing a batch of previous inputs at once, the ``%history`` magic
command is very helpful. Here is how you can print the first four
inputs:

.. code:: ipython3

     %history -n 1-3


.. parsed-literal::

       1: help(len)
       2:
    L = [1, 2, 3]
    help(L.insert)
       3: help(L)


As usual, you can type ``%history?`` for more information and a
description of options available. Other similar magic commands are
``%rerun`` (which will re-execute some portion of the command history)
and ``%save`` (which saves some set of the command history to a file).
For more information, I suggest exploring these using the ``?`` help
functionality discussed in `Help and Documentation in
IPython <01.01-Help-And-Documentation.ipynb>`__.

IPython and Shell Commands
==========================

When working interactively with the standard Python interpreter, one of
the frustrations is the need to switch between multiple windows to
access Python tools and system command-line tools. IPython bridges this
gap, and gives you a syntax for executing shell commands directly from
within the IPython terminal. The magic happens with the exclamation
point: anything appearing after ``!`` on a line will be executed not by
the Python kernel, but by the system command-line.

The following assumes you’re on a Unix-like system, such as Linux or Mac
OSX. Some of the examples that follow will fail on Windows, which uses a
different type of shell by default (though with the 2016 announcement of
native Bash shells on Windows, soon this may no longer be an issue!). If
you’re unfamiliar with shell commands, I’d suggest reviewing the `Shell
Tutorial <http://swcarpentry.github.io/shell-novice/>`__ put together by
the always excellent Software Carpentry Foundation.

Quick Introduction to the Shell
-------------------------------

A full intro to using the shell/terminal/command-line is well beyond the
scope of this chapter, but for the uninitiated we will offer a quick
introduction here. The shell is a way to interact textually with your
computer. Ever since the mid 1980s, when Microsoft and Apple introduced
the first versions of their now ubiquitous graphical operating systems,
most computer users have interacted with their operating system through
familiar clicking of menus and drag-and-drop movements. But operating
systems existed long before these graphical user interfaces, and were
primarily controlled through sequences of text input: at the prompt, the
user would type a command, and the computer would do what the user told
it to. Those early prompt systems are the precursors of the shells and
terminals that most active data scientists still use today.

Someone unfamiliar with the shell might ask why you would bother with
this, when many results can be accomplished by simply clicking on icons
and menus. A shell user might reply with another question: why hunt
icons and click menus when you can accomplish things much more easily by
typing? While it might sound like a typical tech preference impasse,
when moving beyond basic tasks it quickly becomes clear that the shell
offers much more control of advanced tasks, though admittedly the
learning curve can intimidate the average computer user.

As an example, here is a sample of a Linux/OSX shell session where a
user explores, creates, and modifies directories and files on their
system (``osx:~ $`` is the prompt, and everything after the ``$`` sign
is the typed command; text that is preceded by a ``#`` is meant just as
description, rather than something you would actually type in):

.. code:: bash

   osx:~ $ echo "hello world"             # echo is like Python's print function
   hello world

   osx:~ $ pwd                            # pwd = print working directory
   /home/jake                             # this is the "path" that we're sitting in

   osx:~ $ ls                             # ls = list working directory contents
   notebooks  projects 

   osx:~ $ cd projects/                   # cd = change directory

   osx:projects $ pwd
   /home/jake/projects

.. code:: bash

   osx:projects $ ls
   datasci_book   mpld3   myproject.txt

   osx:projects $ mkdir myproject          # mkdir = make new directory

   osx:projects $ cd myproject/

   osx:myproject $ mv ../myproject.txt ./  # mv = move file. Here we're moving the
                                           # file myproject.txt from one directory
                                           # up (../) to the current directory (./)
   osx:myproject $ ls
   myproject.txt

Notice that all of this is just a compact way to do familiar operations
(navigating a directory structure, creating a directory, moving a file,
etc.) by typing commands rather than clicking icons and menus. Note that
with just a few commands (``pwd``, ``ls``, ``cd``, ``mkdir``, and
``cp``) you can do many of the most common file operations. It’s when
you go beyond these basics that the shell approach becomes really
powerful.

Shell Commands in IPython
-------------------------

Any command that works at the command-line can be used in IPython by
prefixing it with the ``!`` character. For example, the ``ls``, ``pwd``,
and ``echo`` commands can be run as follows:

.. code:: ipython3

    !ls


.. parsed-literal::

    Makefile                      matplotlib.slides.html
    [34mdata[m[m                          mprun_demo.py
    file.dot                      my-script.py
    generators.ipynb              my_figure.png
    generators.slides.html        networkx.ipynb
    gotchas.ipynb                 networkx.slides.html
    gotchas.slides.html           notebooks.ipynb
    grid.edgelist                 numpy.ipynb
    [34mimages[m[m                        numpy.slides.html
    introduction.ipynb            pandas.ipynb
    introduction.slides.html      pandas.slides.html
    jupyter-notebooks.ipynb       path.png
    jupyter-notebooks.slides.html path.to.file
    marathon-data.csv             requirements.txt
    matplotlib.ipynb              roget_dat.txt.gz


.. code:: ipython3

    !pwd


.. parsed-literal::

    /Users/mn/Developer/working-copies/pythons/APAD-course/ipynbs


.. code:: ipython3

    !echo "printing from the shell"


.. parsed-literal::

    printing from the shell


Passing Values to and from the Shell
------------------------------------

Shell commands can not only be called from IPython, but can also be made
to interact with the IPython namespace. For example, you can save the
output of any shell command to a Python list using the assignment
operator:

.. code:: ipython3

    contents = !ls
    contents




.. parsed-literal::

    ['Makefile',
     'data',
     'file.dot',
     'generators.ipynb',
     'generators.slides.html',
     'gotchas.ipynb',
     'gotchas.slides.html',
     'grid.edgelist',
     'images',
     'introduction.ipynb',
     'introduction.slides.html',
     'jupyter-notebooks.ipynb',
     'jupyter-notebooks.slides.html',
     'marathon-data.csv',
     'matplotlib.ipynb',
     'matplotlib.slides.html',
     'mprun_demo.py',
     'my-script.py',
     'my_figure.png',
     'networkx.ipynb',
     'networkx.slides.html',
     'notebooks.ipynb',
     'numpy.ipynb',
     'numpy.slides.html',
     'pandas.ipynb',
     'pandas.slides.html',
     'path.png',
     'path.to.file',
     'requirements.txt',
     'roget_dat.txt.gz']



.. code:: ipython3

    directory = !pwd
    directory




.. parsed-literal::

    ['/Users/mn/Developer/working-copies/pythons/APAD-course/ipynbs']



Note that these results are not returned as lists, but as a special
shell return type defined in IPython:

.. code:: ipython3

    type(directory)




.. parsed-literal::

    IPython.utils.text.SList



This looks and acts a lot like a Python list, but has additional
functionality, such as the ``grep`` and ``fields`` methods and the
``s``, ``n``, and ``p`` properties that allow you to search, filter, and
display the results in convenient ways. For more information on these,
you can use IPython’s built-in help features.

Communication in the other direction–passing Python variables into the
shell–is possible using the ``{varname}`` syntax:

.. code:: ipython3

    message = "hello from Python"
    !echo {message}


.. parsed-literal::

    hello from Python


The curly braces contain the variable name, which is replaced by the
variable’s contents in the shell command.

Errors and Debugging
====================

Code development and data analysis always require a bit of trial and
error, and IPython contains tools to streamline this process. This
section will briefly cover some options for controlling Python’s
exception reporting, followed by exploring tools for debugging errors in
code.

Controlling Exceptions: ``%xmode``
----------------------------------

Most of the time when a Python script fails, it will raise an Exception.
When the interpreter hits one of these exceptions, information about the
cause of the error can be found in the *traceback*, which can be
accessed from within Python. With the ``%xmode`` magic function, IPython
allows you to control the amount of information printed when the
exception is raised. Consider the following code:

.. code:: ipython3

    def func1(a, b):
        return a / b
    
    def func2(x):
        a = x
        b = x - 1
        return func1(a, b)


.. code:: ipython3

    func2(1)


::


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-32-7cb498ea7ed1> in <module>
    ----> 1 func2(1)
    

    <ipython-input-31-586ccabd0db3> in func2(x)
          5     a = x
          6     b = x - 1
    ----> 7     return func1(a, b)
    

    <ipython-input-31-586ccabd0db3> in func1(a, b)
          1 def func1(a, b):
    ----> 2     return a / b
          3 
          4 def func2(x):
          5     a = x


    ZeroDivisionError: division by zero


Calling ``func2`` results in an error, and reading the printed trace
lets us see exactly what happened. By default, this trace includes
several lines showing the context of each step that led to the error.
Using the ``%xmode`` magic function (short for *Exception mode*), we can
change what information is printed.

``%xmode`` takes a single argument, the mode, and there are three
possibilities: ``Plain``, ``Context``, and ``Verbose``. The default is
``Context``, and gives output like that just shown before. ``Plain`` is
more compact and gives less information:

.. code:: ipython3

    %xmode Plain


.. parsed-literal::

    Exception reporting mode: Plain


.. code:: ipython3

    func2(1)


::


    Traceback (most recent call last):


      File "<ipython-input-34-7cb498ea7ed1>", line 1, in <module>
        func2(1)


      File "<ipython-input-31-586ccabd0db3>", line 7, in func2
        return func1(a, b)


      File "<ipython-input-31-586ccabd0db3>", line 2, in func1
        return a / b


    ZeroDivisionError: division by zero



The ``Verbose`` mode adds some extra information, including the
arguments to any functions that are called:

.. code:: ipython3

    %xmode Verbose


.. parsed-literal::

    Exception reporting mode: Verbose


.. code:: ipython3

    func2(1)


::


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-36-7cb498ea7ed1> in <module>
    ----> 1 func2(1)
            global func2 = <function func2 at 0x7fd878f41670>


    <ipython-input-31-586ccabd0db3> in func2(x=1)
          5     a = x
          6     b = x - 1
    ----> 7     return func1(a, b)
            global func1 = <function func1 at 0x7fd878f41af0>
            a = 1
            b = 0


    <ipython-input-31-586ccabd0db3> in func1(a=1, b=0)
          1 def func1(a, b):
    ----> 2     return a / b
            a = 1
            b = 0
          3 
          4 def func2(x):
          5     a = x


    ZeroDivisionError: division by zero


This extra information can help narrow-in on why the exception is being
raised. So why not use the ``Verbose`` mode all the time? As code gets
complicated, this kind of traceback can get extremely long. Depending on
the context, sometimes the brevity of ``Default`` mode is easier to work
with.

Debugging: When Reading Tracebacks Is Not Enough
------------------------------------------------

The standard Python tool for interactive debugging is ``pdb``, the
Python debugger. This debugger lets the user step through the code line
by line in order to see what might be causing a more difficult error.
The IPython-enhanced version of this is ``ipdb``, the IPython debugger.

There are many ways to launch and use both these debuggers; we won’t
cover them fully here. Refer to the online documentation of these two
utilities to learn more.

In IPython, perhaps the most convenient interface to debugging is the
``%debug`` magic command. If you call it after hitting an exception, it
will automatically open an interactive debugging prompt at the point of
the exception. The ``ipdb`` prompt lets you explore the current state of
the stack, explore the available variables, and even run Python
commands!

Let’s look at the most recent exception, then do some basic tasks–print
the values of ``a`` and ``b``, and type ``quit`` to quit the debugging
session:

.. code:: ipython3

    %debug


.. parsed-literal::

    > [0;32m<ipython-input-31-586ccabd0db3>[0m(2)[0;36mfunc1[0;34m()[0m
    [0;32m      1 [0;31m[0;32mdef[0m [0mfunc1[0m[0;34m([0m[0ma[0m[0;34m,[0m [0mb[0m[0;34m)[0m[0;34m:[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m----> 2 [0;31m    [0;32mreturn[0m [0ma[0m [0;34m/[0m [0mb[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m      3 [0;31m[0;34m[0m[0m
    [0m[0;32m      4 [0;31m[0;32mdef[0m [0mfunc2[0m[0;34m([0m[0mx[0m[0;34m)[0m[0;34m:[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m      5 [0;31m    [0ma[0m [0;34m=[0m [0mx[0m[0;34m[0m[0;34m[0m[0m
    [0m
    ipdb> continue


The interactive debugger allows much more than this, though–we can even
step up and down through the stack and explore the values of variables
there:

.. code:: ipython3

    %debug


.. parsed-literal::

    > [0;32m<ipython-input-31-586ccabd0db3>[0m(2)[0;36mfunc1[0;34m()[0m
    [0;32m      1 [0;31m[0;32mdef[0m [0mfunc1[0m[0;34m([0m[0ma[0m[0;34m,[0m [0mb[0m[0;34m)[0m[0;34m:[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m----> 2 [0;31m    [0;32mreturn[0m [0ma[0m [0;34m/[0m [0mb[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m      3 [0;31m[0;34m[0m[0m
    [0m[0;32m      4 [0;31m[0;32mdef[0m [0mfunc2[0m[0;34m([0m[0mx[0m[0;34m)[0m[0;34m:[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m      5 [0;31m    [0ma[0m [0;34m=[0m [0mx[0m[0;34m[0m[0;34m[0m[0m
    [0m
    ipdb> continue


This allows you to quickly find out not only what caused the error, but
what function calls led up to the error.

If you’d like the debugger to launch automatically whenever an exception
is raised, you can use the ``%pdb`` magic function to turn on this
automatic behavior:

.. code:: ipython3

    %xmode Plain
    %pdb on
    func2(1)


.. parsed-literal::

    Exception reporting mode: Plain
    Automatic pdb calling has been turned ON


::


    Traceback (most recent call last):


      File "<ipython-input-39-f80f6b5cecf3>", line 3, in <module>
        func2(1)


      File "<ipython-input-31-586ccabd0db3>", line 7, in func2
        return func1(a, b)


      File "<ipython-input-31-586ccabd0db3>", line 2, in func1
        return a / b


    ZeroDivisionError: division by zero



.. parsed-literal::

    > [0;32m<ipython-input-31-586ccabd0db3>[0m(2)[0;36mfunc1[0;34m()[0m
    [0;32m      1 [0;31m[0;32mdef[0m [0mfunc1[0m[0;34m([0m[0ma[0m[0;34m,[0m [0mb[0m[0;34m)[0m[0;34m:[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m----> 2 [0;31m    [0;32mreturn[0m [0ma[0m [0;34m/[0m [0mb[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m      3 [0;31m[0;34m[0m[0m
    [0m[0;32m      4 [0;31m[0;32mdef[0m [0mfunc2[0m[0;34m([0m[0mx[0m[0;34m)[0m[0;34m:[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m      5 [0;31m    [0ma[0m [0;34m=[0m [0mx[0m[0;34m[0m[0;34m[0m[0m
    [0m
    ipdb> continue


Finally, if you have a script that you’d like to run from the beginning
in interactive mode, you can run it with the command ``%run -d``, and
use the ``next`` command to step through the lines of code
interactively.

Partial list of debugging commands
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are many more available commands for interactive debugging than
we’ve listed here; the following table contains a description of some of
the more common and useful ones:

==============
===========================================================
Command        Description
==============
===========================================================
``list``       Show the current location in the file
``h(elp)``     Show a list of commands, or find help on a specific command
``q(uit)``     Quit the debugger and the program
``c(ontinue)`` Quit the debugger, continue in the program
``n(ext)``     Go to the next step of the program
``<enter>``    Repeat the previous command
``p(rint)``    Print variables
``s(tep)``     Step into a subroutine
``r(eturn)``   Return out of a subroutine
==============
===========================================================

For more information, use the ``help`` command in the debugger, or take
a look at ``ipdb``\ ’s `online
documentation <https://github.com/gotcha/ipdb>`__.

Profiling and Timing Code
=========================

In the process of developing code and creating data processing
pipelines, there are often trade-offs you can make between various
implementations. Early in developing your algorithm, it can be
counterproductive to worry about such things. As Donald Knuth famously
quipped, “We should forget about small efficiencies, say about 97% of
the time: premature optimization is the root of all evil.”

But once you have your code working, it can be useful to dig into its
efficiency a bit. Sometimes it’s useful to check the execution time of a
given command or set of commands; other times it’s useful to dig into a
multiline process and determine where the bottleneck lies in some
complicated series of operations. IPython provides access to a wide
array of functionality for this kind of timing and profiling of code.
Here we’ll discuss the following IPython magic commands:

-  ``%time``: Time the execution of a single statement
-  ``%timeit``: Time repeated execution of a single statement for more
   accuracy
-  ``%prun``: Run code with the profiler
-  ``%lprun``: Run code with the line-by-line profiler
-  ``%memit``: Measure the memory use of a single statement
-  ``%mprun``: Run code with the line-by-line memory profiler

The last four commands are not bundled with IPython–you’ll need to get
the ``line_profiler`` and ``memory_profiler`` extensions, which we will
discuss in the following sections.

Timing Code Snippets: ``%timeit`` and ``%time``
-----------------------------------------------

We saw the ``%timeit`` line-magic and ``%%timeit`` cell-magic in the
introduction to magic functions in `IPython Magic
Commands <01.03-Magic-Commands.ipynb>`__; it can be used to time the
repeated execution of snippets of code:

.. code:: ipython3

    %timeit sum(range(100))


.. parsed-literal::

    1.11 µs ± 4.94 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)


Note that because this operation is so fast, ``%timeit`` automatically
does a large number of repetitions.

For slower commands, ``%timeit`` will automatically adjust and perform
fewer repetitions:

.. code:: ipython3

    %%timeit
    total = 0
    for i in range(1000):
        for j in range(1000):
            total += i * (-1) ** j


.. parsed-literal::

    325 ms ± 5.44 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)


Sometimes repeating an operation is not the best option. For example, if
we have a list that we’d like to sort, we might be misled by a repeated
operation. Sorting a pre-sorted list is much faster than sorting an
unsorted list, so the repetition will skew the result:

.. code:: ipython3

    import random
    L = [random.random() for i in range(100000)]
    %timeit L.sort()


.. parsed-literal::

    444 µs ± 10.6 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)


For this, the ``%time`` magic function may be a better choice. It also
is a good choice for longer-running commands, when short, system-related
delays are unlikely to affect the result. Let’s time the sorting of an
unsorted and a presorted list:

.. code:: ipython3

    import random
    L = [random.random() for i in range(100000)]
    print("sorting an unsorted list:")
    %time L.sort()


.. parsed-literal::

    sorting an unsorted list:
    CPU times: user 22.1 ms, sys: 78 µs, total: 22.1 ms
    Wall time: 22.1 ms


.. code:: ipython3

    print("sorting an already sorted list:")
    %time L.sort()


.. parsed-literal::

    sorting an already sorted list:
    CPU times: user 1.29 ms, sys: 1 µs, total: 1.29 ms
    Wall time: 1.3 ms


Notice how much faster the presorted list is to sort, but notice also
how much longer the timing takes with ``%time`` versus ``%timeit``, even
for the presorted list! This is a result of the fact that ``%timeit``
does some clever things under the hood to prevent system calls from
interfering with the timing. For example, it prevents cleanup of unused
Python objects (known as *garbage collection*) which might otherwise
affect the timing. For this reason, ``%timeit`` results are usually
noticeably faster than ``%time`` results.

For ``%time`` as with ``%timeit``, using the double-percent-sign cell
magic syntax allows timing of multiline scripts:

.. code:: ipython3

    %%time
    total = 0
    for i in range(1000):
        for j in range(1000):
            total += i * (-1) ** j


.. parsed-literal::

    CPU times: user 418 ms, sys: 2.57 ms, total: 421 ms
    Wall time: 420 ms


For more information on ``%time`` and ``%timeit``, as well as their
available options, use the IPython help functionality (i.e., type
``%time?`` at the IPython prompt).

Profiling Full Scripts: ``%prun``
---------------------------------

A program is made of many single statements, and sometimes timing these
statements in context is more important than timing them on their own.
Python contains a built-in code profiler (which you can read about in
the Python documentation), but IPython offers a much more convenient way
to use this profiler, in the form of the magic function ``%prun``.

By way of example, we’ll define a simple function that does some
calculations:

.. code:: ipython3

    def sum_of_lists(N):
        total = 0
        for i in range(5):
            L = [j ^ (j >> i) for j in range(N)]
            total += sum(L)
        return total

Now we can call ``%prun`` with a function call to see the profiled
results:

.. code:: ipython3

    %prun sum_of_lists(1000000)


.. parsed-literal::

     

::

   14 function calls in 0.705 seconds

      Ordered by: internal time

      ncalls  tottime  percall  cumtime  percall filename:lineno(function)
           5    0.614    0.123    0.614    0.123 <ipython-input-95-f105717832a2>:4(<listcomp>)
           5    0.043    0.009    0.043    0.009 {built-in method builtins.sum}
           1    0.036    0.036    0.693    0.693 <ipython-input-95-f105717832a2>:1(sum_of_lists)
           1    0.012    0.012    0.705    0.705 <string>:1(<module>)
           1    0.000    0.000    0.705    0.705 {built-in method builtins.exec}
           1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}

The result is a table that indicates, in order of total time on each
function call, where the execution is spending the most time. In this
case, the bulk of execution time is in the list comprehension inside
``sum_of_lists``. From here, we could start thinking about what changes
we might make to improve the performance in the algorithm.

For more information on ``%prun``, as well as its available options, use
the IPython help functionality (i.e., type ``%prun?`` at the IPython
prompt).

Line-By-Line Profiling with ``%lprun``
--------------------------------------

The function-by-function profiling of ``%prun`` is useful, but sometimes
it’s more convenient to have a line-by-line profile report. This is not
built into Python or IPython, but there is a ``line_profiler`` package
available for installation that can do this. Start by using Python’s
packaging tool, ``pip``, to install the ``line_profiler`` package:

::

   $ pip install line_profiler

Next, you can use IPython to load the ``line_profiler`` IPython
extension, offered as part of this package:

.. code:: ipython3

    %load_ext line_profiler

Now the ``%lprun`` command will do a line-by-line profiling of any
function–in this case, we need to tell it explicitly which functions
we’re interested in profiling:

.. code:: ipython3

    %lprun -f sum_of_lists sum_of_lists(5000)

\``\` Timer unit: 1e-06 s

Total time: 0.006239 s File:
/home/mn/Developer/working-copies/pythons/on-python/UniFiCourseSpring2020/mprun_demo.py
Function: sum_of_lists at line 1

Line # Hits Time Per Hit % Time Line Contents
=============================================

::

    1                                           def sum_of_lists(N):
    2         1          2.0      2.0      0.0      total = 0
    3         6          6.0      1.0      0.1      for i in range(5):
    4         5       5869.0   1173.8     94.1          L = [j ^ (j >> i) for j in range(N)]
    5         5        218.0     43.6      3.5          total += sum(L)
    6         5        144.0     28.8      2.3          del L # remove reference to L
    7         1          0.0      0.0      0.0      return total```

The result is a table that indicates, in order of total time on each
function call, where the execution is spending the most time. In this
case, the bulk of execution time is in the list comprehension inside
``sum_of_lists``. From here, we could start thinking about what changes
we might make to improve the performance in the algorithm.

For more information on ``%prun``, as well as its available options, use
the IPython help functionality (i.e., type ``%prun?`` at the IPython
prompt).

Profiling Memory Use: ``%memit`` and ``%mprun``
-----------------------------------------------

Another aspect of profiling is the amount of memory an operation uses.
This can be evaluated with another IPython extension, the
``memory_profiler``. As with the ``line_profiler``, we start by
``pip``-installing the extension:

::

   $ pip install memory_profiler

Then we can use IPython to load the extension:

.. code:: ipython3

    %load_ext memory_profiler

The memory profiler extension contains two useful magic functions: the
``%memit`` magic (which offers a memory-measuring equivalent of
``%timeit``) and the ``%mprun`` function (which offers a
memory-measuring equivalent of ``%lprun``). The ``%memit`` function can
be used rather simply:

.. code:: ipython3

    %memit sum_of_lists(1000000)


.. parsed-literal::

    peak memory: 129.71 MiB, increment: 72.76 MiB


We see that this function uses about 100 MB of memory.

For a line-by-line description of memory use, we can use the ``%mprun``
magic. Unfortunately, this magic works only for functions defined in
separate modules rather than the notebook itself, so we’ll start by
using the ``%%file`` magic to create a simple module called
``mprun_demo.py``, which contains our ``sum_of_lists`` function, with
one addition that will make our memory profiling results more clear:

.. code:: ipython3

    %%file mprun_demo.py
    def sum_of_lists(N):
        total = 0
        for i in range(5):
            L = [j ^ (j >> i) for j in range(N)]
            total += sum(L)
            del L # remove reference to L
        return total


.. parsed-literal::

    Overwriting mprun_demo.py


We can now import the new version of this function and run the memory
line profiler:

.. code:: ipython3

    from mprun_demo import sum_of_lists
    %mprun -f sum_of_lists sum_of_lists(1000000)


.. parsed-literal::

    *** KeyboardInterrupt exception caught in code being profiled.


\``\` Filename:
/home/mn/Developer/working-copies/pythons/on-python/UniFiCourseSpring2020/mprun_demo.py

Line # Mem usage Increment Line Contents
========================================

::

    1     85.2 MiB     85.2 MiB   def sum_of_lists(N):
    2     85.2 MiB      0.0 MiB       total = 0
    3     85.2 MiB      0.0 MiB       for i in range(5):
    4    112.2 MiB      0.2 MiB           L = [j ^ (j >> i) for j in range(N)]
    5    112.2 MiB      0.0 MiB           total += sum(L)
    6     85.2 MiB      0.0 MiB           del L # remove reference to L
    7                                 return total```

Here the ``Increment`` column tells us how much each line affects the
total memory budget: observe that when we create and delete the list
``L``, we are adding about 25 MB of memory usage. This is on top of the
background memory usage from the Python interpreter itself.

For more information on ``%memit`` and ``%mprun``, as well as their
available options, use the IPython help functionality (i.e., type
``%memit?`` at the IPython prompt).
