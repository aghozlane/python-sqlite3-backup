Sqlite3 backup function implementation for Python sqlite3 module
================================================================

Single function that allows to save any sqlite3 database one to another. You
can use this for example for loading and dumping memory database (`:memory:`)
into file (alternative to `iter dump`_ functionality).

See the `Sqlite3 C API docs`_ for more info.

.. _iter dump: http://docs.python.org/release/2.6/library/sqlite3.html#sqlite3.Connection.iterdump
.. _Sqlite3 C API docs: http://www.sqlite.org/c3ref/backup_finish.html


Build and installation
----------------------

Now you can build or install `sqlitebck` using distutils::

    $ python setup.py install


Tests
-----

Nothing big, just test basic functionality::

    $ python tests.py


Usage example
-------------

Basic usage example - memory database saved into file::
    
    >>> import sqlite3
    >>> conn = sqlite3.connect(':memory:')
    >>> curr = conn.cursor()
    
    # create table and put there some data
    >>> curr.execute('CREATE TABLE foo (bar INTEGER)')
    <sqlite3.Cursor object at 0xb73b2800>
    >>> curr.execute('INSERT INTO foo VALUES (123)')
    <sqlite3.Cursor object at 0xb73b2800>
    >>> curr.close()
    >>> conn.commit()
    >>> import sqlitebck
    
    # save in memory database (conn) into file
    >>> conn2 = sqlite3.connect('/tmp/in_memory_sqlite_db_save.db')
    >>> sqlitebck.copy(conn, conn2)
    >>> conn.close()
    >>> curr2 = conn2.cursor()
    
    # check if data is in file database ;)
    >>> curr2.execute('SELECT * FROM foo');
    <sqlite3.Cursor object at 0xb73b2860>
    >>> curr2.fetchall()
    [(123,)]

If you want to load file database into memory, just call::

    >>> sqlitebck.copy(conn2, conn)

