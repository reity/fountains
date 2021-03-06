=========
fountains
=========

Python library for generating and embedding in a compact way random but reproducible data for unit testing.

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
This library makes it possible to generate random test data in a reproducible way. It also makes it possible to include very concise (i.e., one-line) test definitions that nevertheless test a function on a large number of inputs.

Package Installation and Usage
------------------------------
The package is available on PyPI::

    python -m pip install fountains

The library can be imported in the usual ways::

    import fountains
    from fountains import fountains

Testing and Conventions
-----------------------
All unit tests are executed and their coverage is measured when using `nose <https://nose.readthedocs.io/>`_ (see ``setup.cfg`` for configution details)::

    nosetests

Alternatively, all unit tests are included in the module itself and can be executed using `doctest <https://docs.python.org/3/library/doctest.html>`_::

    python fountains/fountains.py -v

Style conventions are enforced using `Pylint <https://www.pylint.org/>`_::

    pylint fountains

Contributions
-------------
In order to contribute to the source code, open an issue or submit a pull request on the GitHub page for this library.

Versioning
----------
Beginning with version 0.2.0, the version number format for this library and the changes to the library associated with version number increments conform with `Semantic Versioning 2.0.0 <https://semver.org/#semantic-versioning-200>`_.
