=========
fountains
=========

Python library for generating and concisely specifying reproducible pseudorandom binary data for unit testing.

|pypi| |travis| |coveralls|

.. |pypi| image:: https://badge.fury.io/py/fountains.svg
   :target: https://badge.fury.io/py/fountains
   :alt: PyPI version and link.

.. |travis| image:: https://travis-ci.com/reity/fountains.svg?branch=master
   :target: https://travis-ci.com/reity/fountains

.. |coveralls| image:: https://coveralls.io/repos/github/reity/fountains/badge.svg?branch=master
   :target: https://coveralls.io/github/reity/fountains?branch=master

Purpose
-------
This library makes it possible to generate pseudorandom binary test data in a reproducible way, as well as to embed concise specifications of correct function behavior on that test data. This enables the construction of functional tests within unit testing suites that fit within one-line definitions but still test a function's behavior against a large number of inputs. More background information about this library's purpose, design, and implementation can be found in a `related article <https://github.com/reity/article-specifications-for-distinguishing-functions>`_.

Package Installation and Usage
------------------------------
The package is available on `PyPI <https://pypi.org/project/fountains/>`_::

    python -m pip install fountains

The library can be imported in the usual ways::

    import fountains
    from fountains import fountains

Examples
^^^^^^^^
An object of the `fountains` class can be used to generate pseudorandom binary test data::

    >>> [bs.hex() for bs in fountains(length=3, limit=4)]
    ['e3b0c4', 'ce1bc4', '2ed5b5', '781f5a']

Supplying a function as a parameter to a `fountains` object makes it possible to generate a concise (but necessarily incomplete) specification for that function's behavior on a stream of pseudorandom inputs::

    >>> add = lambda bs: bytes([(bs[0] + bs[1] + bs[2]) % 256])
    >>> bits = list(fountains(3, 8, function=add))
    >>> bits
    [0, 0, 1, 1, 1, 0, 1, 0]
    
When converted to a hexadecimal string, this specification encodes partial information about 4 distinct input-output test cases in every character::
    
    >>> from bitlist import bitlist
    >>> bitlist(bits).hex()
    '3a' # Partial outputs from 8 distinct tests.

Supplying the specification generated in the manner above as an additional parameter makes it possible to test the function's behavior::

    >>> list(fountains(3, 8, function=add, bits='3a'))
    [True, True, True, True, True, True, True, True]

Each individual boolean value in the above represents the result of an individual test case. A different function might not satisfy the same partial specification::

    >>> mul = lambda bs: bytes([(bs[0] * bs[1] * bs[2]) % 256])
    >>> list(fountains(3, 8, function=mul, bits='3a'))
    [True, False, True, True, False, True, False, True]

Each boolean value in the outputs of the last two code blocks above may be a false negative (i.e., `True` may mean that the function satisfies the specification only in a portion of its output for the corresponding input) but is *never a false positive signal of incorrect behavior* (i.e., `False` indicates the function does not satisfy the specification for the corresponding input-output pair).

Testing and Conventions
-----------------------
All unit tests are executed and their coverage is measured when using `nose <https://nose.readthedocs.io/>`_ (see ``setup.cfg`` for configuration details)::

    python -m pip install nose coverage
    nosetests

Alternatively, all unit tests are included in the module itself and can be executed using `doctest <https://docs.python.org/3/library/doctest.html>`_::

    python fountains/fountains.py -v

Style conventions are enforced using `Pylint <https://www.pylint.org/>`_::

    python -m pip install nose coverage
    pylint fountains

Contributions
-------------
In order to contribute to the source code, open an issue or submit a pull request on the `GitHub page <https://github.com/reity/fountains>`_ for this library.

Versioning
----------
Beginning with version 0.2.0, the version number format for this library and the changes to the library associated with version number increments conform with `Semantic Versioning 2.0.0 <https://semver.org/#semantic-versioning-200>`_.
