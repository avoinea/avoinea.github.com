=================================================
Installing Python 2.4 with-zlib on Debian squeeze
=================================================

Yap, it's not as easy as it is supposed to be.


1. First of all make sure you have installed **zlib-dev** library,
which in Debian/Ubuntu is called **zlib1-dev**:

.. code-block:: console

  $ sudo aptitude install zlib1-dev


2. Download Python from http://www.python.org/download/releases/2.4.6/

.. code-block:: console

  $ cd /tmp
  $ wget http://www.python.org/ftp/python/2.4.6/Python-2.4.6.tgz


3. Decompress it:

.. code-block:: console

  $ tar -zxvf Python-2.4.6.tgz


4. Compile it. STOP, I did that 10 times already with and without
`--with-zlib=/usr/include` option, nothing, only a lot of frustration. So here
is the key:

.. code-block:: console

  $ cd /lib
  $ sudo ln -s x86_64-linux-gnu/libz.so.1 libz.so


5. Now compile and install python:

.. code-block:: console

  $ cd /tmp/Python-2.4.6
  $ ./configure --prefix=/opt/python24
  $ make
  $ make install


6. Test if zlib is installed:

.. code-block:: console

  $ /opt/python24/bin/python -c "import zlib"


7. Enjoy :o)
