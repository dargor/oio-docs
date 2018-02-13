============
Installation
============

Initialize
==========

.. only:: ubuntu or debian

  .. include:: ../../install-common/source/initialize_debian.rst

.. only:: centos

  .. include:: ../../install-common/source/initialize_centos.rst

Repositories Configuration
==========================

.. only:: centos

  .. include:: ../../install-common/source/packages_configuration_centos.rst

.. only:: ubuntu

  .. include:: ../../install-common/source/packages_configuration_ubuntu.rst

.. only:: debian

  .. include:: ../../install-common/source/packages_configuration_debian.rst

3. Install the OpenStack repository

   .. only:: centos

      .. code-block:: bash

        sudo bash
        yum -y install epel-release centos-release-openstack-pike

   .. only:: ubuntu

      .. code-block:: bash

        sudo bash
        add-apt-repository cloud-archive:pike -y
        apt-get update



Installation
============

We will use the OpenStack modules to install and configure OpenStack KeyStone.

Install the module:

   .. only:: debian

      .. code-block:: bash

        puppet module install openstack-keystone

   .. only:: ubuntu

      .. code-block:: bash

        curl -L https://github.com/openstack/puppet-keystone/archive/stable/pike.tar.gz | tar xzf - -C /etc/puppet/modules/ ; mv /etc/puppet/modules/puppet-keystone-stable-pike /etc/puppet/modules/keystone
        curl -L https://github.com/openstack/puppet-openstacklib/archive/stable/pike.tar.gz | tar xzf - -C /etc/puppet/modules/ ; mv /etc/puppet/modules/puppet-openstacklib-stable-pike /etc/puppet/modules/openstacklib
        curl -L https://github.com/openstack/puppet-oslo/archive/stable/pike.tar.gz | tar xzf - -C /etc/puppet/modules/ ; mv /etc/puppet/modules/puppet-oslo-stable-pike /etc/puppet/modules/oslo
        for module in puppetlabs/apache puppetlabs/inifile puppetlabs/stdlib ; do puppet module install $module ; done

   .. only:: centos

      .. code-block:: bash

        yum install -y puppet curl
        curl -L https://github.com/openstack/puppet-keystone/archive/stable/pike.tar.gz | tar xzf - -C /etc/puppet/modules/ ; mv /etc/puppet/modules/puppet-keystone-stable-pike /etc/puppet/modules/keystone
        curl -L https://github.com/openstack/puppet-openstacklib/archive/stable/pike.tar.gz | tar xzf - -C /etc/puppet/modules/ ; mv /etc/puppet/modules/puppet-openstacklib-stable-pike /etc/puppet/modules/openstacklib
        curl -L https://github.com/openstack/puppet-oslo/archive/stable/pike.tar.gz | tar xzf - -C /etc/puppet/modules/ ; mv /etc/puppet/modules/puppet-oslo-stable-pike /etc/puppet/modules/oslo
        for module in puppetlabs/apache puppetlabs/inifile puppetlabs/stdlib ; do puppet module install $module ; done


Puppet Manifest
===============

Here is an example manifest you can tune to your own settings:

- `OPENIO_PROXY_URL` should point to an oioproxy service. `6006` is the default port, so you can just change the `OIO_SERVER` to another server where OpenIO is installed.
- `admin_token` is used for KeyStone administrative purpose only, to secure your installation, modify it.
- To secure your installation, modify the password fields `SWIFT_PASS` and `DEMO_PASS`.

In a file called ``~/openio.pp``:

  .. code-block:: puppet

    $openio_proxy_url = "http://OPENIO_PROXY_URL:6006"
    $admin_token = 'KEYSTONE_ADMIN_UUID'
    $swift_passwd = 'SWIFT_PASS'
    $admin_passwd = 'ADMIN_PASS'
    $demo_passwd = 'DEMO_PASS'
    $region = 'RegionOne'

    # Deploy Openstack Keystone
    class { 'keystone':
      admin_token         => $admin_token,
      admin_password      => $admin_passwd,
      database_connection => 'sqlite:////var/lib/keystone/keystone.db',
      service_name => 'httpd',
    }

    # Use Apache httpd service with mod_wsgi
    class { 'keystone::wsgi::apache':
      ssl => false,
    }

    # Adds the admin credential to keystone.
    class { 'keystone::roles::admin':
      email               => 'test@openio.io',
      password            => $admin_passwd,
      admin               => 'admin',
      admin_tenant        => 'admin',
      admin_user_domain   => 'admin',
      admin_project_domain => 'admin',
    }

    # Installs the service user endpoint.
    class { 'keystone::endpoint':
      public_url   => "http://${ipaddress}:5000",
      admin_url    => "http://${ipaddress}:35357",
      internal_url => "http://${ipaddress}:5000",
      region       => $region,
    }

    # Openstack Swift service credentials
    keystone_user { 'swift':
      ensure   => present,
      enabled  => true,
      password => $swift_passwd,
    }
    keystone_user_role { 'swift@services':
      roles  => ['admin'],
      ensure => present
    }
    keystone_service { 'openio-swift':
      ensure      => present,
      type        => 'object-store',
      description => 'OpenIO SDS swift proxy',
    }
    keystone_endpoint { 'localhost-1/openio-swift':
      ensure       => present,
      type         => 'object-store',
      public_url   => "http://${ipaddress}:6007/v1.0/AUTH_%(tenant_id)s",
      admin_url    => "http://${ipaddress}:6007/v1.0/AUTH_%(tenant_id)s",
      internal_url => "http://${ipaddress}:6007/v1.0/AUTH_%(tenant_id)s",
    }

    # Demo account credentials
    keystone_tenant { 'demo':
      ensure  => present,
      enabled => true,
    }
    keystone_user { 'demo':
      ensure  => present,
      enabled => true,
      password => $demo_passwd,
    }
    keystone_role { '_member_':
      ensure => present,
    }
    keystone_user_role { 'demo@demo':
      roles  => ['admin','_member_'],
      ensure => present
    }

    # Deploy OpenIO Swift/S3 gateway
    class {'openiosds':}
    openiosds::namespace {'OPENIO':
        ns => 'OPENIO',
    }
    openiosds::oioswift {'oioswift-0':
      ns                 => 'OPENIO',
      ipaddress          => '0.0.0.0',
      sds_proxy_url      => $openio_proxy_url,
      password     => $swift_passwd,
      memcache_servers   => "${ipaddress}:6019",
      region_name        => $region,
      middleware_swift3 => {'location' => $region},
      project_name => 'services',
    }
    openiosds::memcached {'memcached-0':
      ns => 'OPENIO',
    }

  .. note::
    The `demo` user will be created for testing purpose, following the example of the OpenStack Keystone documentation.


Package Installation and Service Configuration
==============================================

Now let's run Puppet, it install the packages and configure the services.
Apply the manifest:

   .. code-block:: bash

        puppet apply --no-stringify_facts ~/openio.pp

This step may take a few minutes. Please be patient as it downloads and installs all necessary packages.
Once completed, all services will be installed and running using OpenIO GridInit init system.

Test run
==============================================

Just to make sure our setup works, we should test it using the swift CLI:

   .. only:: centos

      .. code-block:: bash

        yum -y install python-swiftclient

   .. only:: ubuntu

      .. code-block:: bash

        apt-get install -y python-swiftclient

Now create a credentials file for the demo account (change accordingly if you have changed the default configuration)

   .. code-block:: bash

        cat <<EOF >> ~/.demo_keystonerc
        export OS_IDENTITY_API_VERSION="3"
        export OS_AUTH_URL="http://127.0.0.1:5000/v3"
        export OS_USER_DOMAIN_ID="default"
        export OS_PROJECT_DOMAIN_ID="default"
        export OS_PROJECT_NAME="demo"
        export OS_USERNAME="demo"
        export OS_PASSWORD="DEMO_PASS"
        EOF

Then source the file and try to get the stats from Swift:

   .. code-block:: bash

        source ~/.demo_keystonerc
        swift stat
            Account: AUTH_908fa1a46c8d44ae8db4580305b1cd9c
            Containers: 0
            Objects: 0
            Bytes: 0
            X-Timestamp: 1518478586.21649
            X-Trans-Id: tx79ef74dbaf9940d0ac876-005a8224fa
            Content-Type: text/plain; charset=utf-8
            X-Openstack-Request-Id: tx79ef74dbaf9940d0ac876-005a8224fa

If you get a similar output, then your Swift gateway is up and running.
