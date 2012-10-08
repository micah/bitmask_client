=========================================
The LEAP Encryption Access Project Client
=========================================

*your internet encryption toolkit*

Installation
=============

Base Dependencies
------------------
Leap client depends on these libraries:

* python 2.6 or 2.7
* qt4 libraries (see installing Qt section below)
* libgnutls

Python packages are listed in ``pkg/requirements.pip`` and ``pkg/test-requirements.pip``

Debian systems
--------------

* python-qt4
* python-gnutls == 1.1.9
* python-keyring
* python-crypto
* python setuptools
* python-nose, python-mock, python-coverage (if you want to run tests)

Under a debian-based system, you can run::

  apt-get install openvpn python-qt4 python-gnutls python-keyring python-crypto
  
For testing:

  python-nose python-mock python-coverage

For building the package you will need also::

  pyqt4-dev-tools libgnutls-dev python-setuptools python-all-dev

# **note**: Some libs like setuptools are needed as build-deps only.                  
# TODO we probably should separate what's needed as a global lib dependency, and what's a dependency that still can be retrieved using pip.

Install python dependencies with pip
-------------------------------------
Use pip (preferrable inside a virtualenv) to install all the required python packages::

  apt-get install python-pip
  pip install -r pkg/requirements.pip


Install PyQt
------------
If you attempt to install PyQt using pip, it will fail because PyQt4 does not use the standard setup.py mechanism.

You can:
* run pkg/postmkvenv.sh after creating your virtualenv. It will symlink to your global PyQt installation (recommended).
* install PyQt globally and make a virtualenv with --site-packages
* run pkg/install_pyqt.sh inside your virtualenv (with --no-site-packages)

When installing from the tarball, it uses configure.py which generates a Makefile::

  python configure.py
  make && make install


Install leap-client
-------------------

After getting the sources and installing all the dependencies, proceed to install leap-client package:

# need to run this if you are installing from the git source tree
# not needed if installing from a tarball::

  python setup.py branding

# run this if you have installed previous versions before::

  python setup.py clean

And finally, build and install leap-client::

  python setup.py install # as root if installing globally.


Running the App
-----------------

If you're running a branded build, the script name will have a infix that
depends on your build flavor. Look for it in ``/usr/local/bin``::

  % leap-springbok-client

In order to run the client in debug mode::

  % leap-springbok-client --debug --logfile /tmp/leap.log

To see all the available command line options::

  % leap-springbok-client --help


Development
==============

Hack
--------------

The LEAP client git repository is available at:
git://leap.se/leap_client 

Some steps to be run when setting a development environment for the first time.

# recommended: enable a **virtualenv** to isolate your libraries::

  % virtualenv .  # ensure your .gitignore knows about it
  % source bin/activate

# make sure you are in the development branch::

  (leap_client)% git checkout develop
  (leap_client)% pkg/postmkvenv.sh
  (leap_client)% python setup.py branding
  (leap_client)% python setup.py develop  

to avoid messing with the entry point and global versions installed,
it's recommended to run the app like this during development cycle::

  (leap_client)% cd src/leap 
  (leap_client)% python app.py --debug

Install testing dependencies
----------------------------

have a look at ``pkg/test-requirements.pip``
The ./run_tests.sh command should install all of them in your virtualenv for you.

Running tests
-------------

There is a convenience script at ``./run_tests.sh``

If you want to run specific tests, pass the (sub)module to nose::
  nosetests leap.util

or::
  nosetests leap.util.tests.test_leap_argparse

Hint: colorized output
----------------------
Install ``rednose`` locally and activate it, and give your eyes a rest :)::

  (leap_client)% pip install rednose
  (leap_client)% export NOSE_REDNOSE=1


Tox
---
For running testsuite against all the supported python versions (currently 2.6 and 2.7), run::

  tox -v


Compiling resource/ui files
-----------------------------

You should refresh resource/ui files every time you change an image or a resource/ui (.ui / .qc). From the root folder::

  make ui
  make resources

As there are some tests to guard against unwanted resource updates, you will have to update the resource hash in those failing tests.