========
Licenses
========

OpenIO releases its Open Source code under the term of variant of the GNU
Public License, a variant depending on the code.

* `OpenIO SDS`_:

  * The LGPLv3_ is used for all the code destined to be included in third-party applications. This is gathered under the core/ and metautils/ directories.
  * The AGPLv3_ is used elsewhere, and is always related to daemons or CLI tools.

* `OpenIO Swift`_: Apache License, Version 2
* `OpenIO Java`_ client: LGPLv3 (GNU Lesser Public License, version 3)
* `OpenIO SDS, the puppet templates`_: Apache License, Version 2

Compile-time dependencies
~~~~~~~~~~~~~~~~~~~~~~~~~

Build our C codebase relies on:

* asn1c_: BSD License
* cmake_: BSD License
* flex_: BSD License
* json-c_: BSD License
* bison_: GPLv3_ (with a grant for generated code)
* gcc_: GPLv3_
* GNU make_: GPLv3_
* glib2_: LGPLv2_
* libcurl_: MIT/X derivative License

Building and testing our Java codebase depends on:

* gradle_: Apache License, Version 2
* jetty_: Apache License, Version 2
* junit_: Eclipse Public License, Version 1
* mockito_: MIT License

Building and testing our Python code requires:

* python-coverage_: Apache software License, version 2
* python-fixtures_: Apache software License, version 2
* python-pbr_: Apache License, Version 2
* python-flake8_: MIT License
* python-pip_: MIT License
* python-tox_: MIT License
* python-mock_: BSD License
* python-nose_: LGPLv3_


Delivery Dependencies
~~~~~~~~~~~~~~~~~~~~~

* puppet_: Apache License, Version 2


Runtime Dependencies
~~~~~~~~~~~~~~~~~~~~

The codebase written in C depends on:

* apr_: Apache License, Version 2.0
* httpd_ (and its shipped modules): Apache License, Version 2.0
* zookeeper_ (client): Apache License, Version 2.0
* libattr_: LGPLv2_
* zeromq3_: LGPLv2_
* liberasurecode_: BSD License
* lzo_: GPLv2_
* python_: BeOpen license
* sqlite_: Public Domain (without support)

The Java codebase depends on:

* gson_: Apache License, Version 2
* log4j_: Apache License, Version 2

OpenIO SDS also requires some side services to support it, all published under
the terms of a public license:

* beanstalkd_: MIT License
* gridinit_: AGPLv3
* zookeeper_ (server): Apache License, Version 2.0

Our runtime also requires the following python dependencies:

* python-cliff_: Apache License, Version 2
* python-requests_: Apache License, Version 2
* python-eventlet_: MIT License
* python-gunicorn_: MIT License
* python-redis_: MIT License
* python-setuptools_: MIT License
* python-simplejson_: MIT License
* pyxattr_: MIT License
* PyYAML_: MIT License
* python-futures_: BSD License
* python-plyvel_: BSD License
* python-pyeclib_: BSD License
* python-werkzeug_: BSD License

.. _apr: http://apr.apache.org
.. _asn1c: https://github.com/open-io/asn1c
.. _beanstalkd: https://github.com/kr/beanstalkd
.. _bison: https://www.gnu.org/software/bison/
.. _cmake: https://cmake.org/
.. _flex: https://github.com/westes/flex
.. _gcc: https://gcc.gnu.org/
.. _glib2: https://developer.gnome.org/glib/
.. _GNU make: https://www.gnu.org/software/make/
.. _gradle: https://gradle.org/
.. _gridinit: https://github.com/open-io/gridinit
.. _gson: https://github.com/google/gson
.. _httpd: http://httpd.apache.org
.. _jetty: http://www.eclipse.org/jetty/
.. _json-c: https://github.com/json-c/json-c
.. _junit: http://junit.org/junit4/
.. _libattr: http://savannah.nongnu.org/projects/attr
.. _libcurl: https://curl.haxx.se/libcurl/
.. _liberasurecode: https://github.com/openstack/liberasurecode
.. _log4j: https://logging.apache.org/log4j/2.x/
.. _lzo: http://www.oberhumer.com/opensource/lzo/
.. _make: https://www.gnu.org/software/make/
.. _mockito: https://github.com/mockito/mockito
.. _OpenIO Java: https://github.com/open-io/oio-api-java
.. _OpenIO SDS: https://github.com/open-io/oio-sds
.. _OpenIO SDS, the puppet templates: https://github.com/open-io/puppet-openiosds
.. _OpenIO Swift: https://github.com/open-io/oio-swift
.. _puppet: https://puppet.com/
.. _python-cliff: https://pypi.python.org/pypi/simplejson
.. _python-coverage: https://pypi.python.org/pypi/coverage
.. _python-eventlet: https://pypi.python.org/pypi/eventlet
.. _python-fixtures: https://pypi.python.org/pypi/fixtures
.. _python-flake8: https://pypi.python.org/pypi/flake8
.. _python-futures: https://pypi.python.org/pypi/futures
.. _python-gunicorn: https://pypi.python.org/pypi/gunicorn
.. _python: https://docs.python.org/3/license.html
.. _python-mock: https://pypi.python.org/pypi/mock
.. _python-nose: https://pypi.python.org/pypi/nose
.. _python-pbr: https://pypi.python.org/pypi/pbr
.. _python-pip: https://pypi.python.org/pypi/pip
.. _python-plyvel: https://pypi.python.org/pypi/plyvel
.. _python-pyeclib: https://pypi.python.org/pypi/PyECLib
.. _python-redis: https://pypi.python.org/pypi/redis
.. _python-requests: https://pypi.python.org/pypi/requests
.. _python-setuptools: https://pypi.python.org/pypi/setuptools
.. _python-simplejson: https://pypi.python.org/pypi/simplejson
.. _python-tox: https://pypi.python.org/pypi/tox
.. _python-werkzeug: https://pypi.python.org/pypi/Werkzeug
.. _pyxattr: https://pypi.python.org/pypi/xattr
.. _pyyaml: https://pypi.python.org/pypi/PyYAML
.. _sqlite: http://sqlite.org/
.. _templates: https://github.com/open-io/puppet-openiosds
.. _zeromq3: http://zeromq.org/
.. _zookeeper: http://zookeeperapr.apache.org

.. _AGPLv3: https://www.gnu.org/licenses/agpl.html
.. _AGPLv2: https://www.gnu.org/licenses/old-licenses/agpl-2.1.html
.. _LGPLv3: https://www.gnu.org/licenses/lgpl.html
.. _LGPLv2: https://www.gnu.org/licenses/old-licenses/lgpl-2.1.html
.. _GPLv3: https://www.gnu.org/licenses/gpl.html
.. _GPLv2: https://www.gnu.org/licenses/old-licenses/gpl-2.1.html
