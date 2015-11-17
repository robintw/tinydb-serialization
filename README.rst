tinydb-serialization
^^^^^^^^^^^^^^^^^

|Build Status| |Coverage| |Version|

``tinydb-serialization`` provides serialization for objects that TinyDB
otherwise couldn't handle.

Usage
*****

Creating a Serializer
---------------------

In this example we implement a serializer for ``datetime`` objects:

.. code-block:: python

    from datetime import datetime
    from tinydb_serialization import Serializer

    class DateTimeSerializer(Serializer):
        OBJ_CLASS = datetime  # The class this serializer handles

        def encode(self, obj):
            return obj.strftime('%Y-%m-%dT%H:%M:%S')

        def decode(self, s):
            return datetime.strptime(s, '%Y-%m-%dT%H:%M:%S') 

Using a Serializer
------------------

You can use your serializer like this:

.. code-block:: python

    >>> from tinydb.storages import JSONStorage
    >>> from tinydb_serialization import SerializationMiddleware
    >>>
    >>> serialization = SerializationMiddleware()
    >>> serialization.register_serializer(DateTimeSerializer(), 'TinyDate')
    >>>
    >>> db = TinyDB('db.json', storage=serialization)
    >>> db.insert({'date': datetime(2000, 1, 1, 12, 0, 0)})
    >>> db.all()
    [{'date': datetime.datetime(2000, 1, 1, 12, 0)}] 

Changelog
*********

**v1.0.0** (2015-09-27)
-----------------------

- Initial release on PyPI

.. |Build Status| image:: http://img.shields.io/travis/msiemens/tinydb-serialization.svg?style=flat-square
   :target: https://travis-ci.org/msiemens/tinydb-serialization
.. |Coverage| image:: http://img.shields.io/coveralls/msiemens/tinydb-serialization.svg?style=flat-square
   :target: https://coveralls.io/r/msiemens/tinydb-serialization
.. |Version| image:: http://img.shields.io/pypi/v/tinydb-serialization.svg?style=flat-square
   :target: https://pypi.python.org/pypi/tinydb-serialization/
