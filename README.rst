OpenStack Tempest testing memo
==============================

1. Try quickstart of Tempest
2. Try non-admin operation
3. Triage the launchpad bugs

Deploy Tempest
--------------

1. Install ubuntu 16.04 on virtualbox. (10 min)
   768MB memory
   OpenSSH server
   VertualBox -> Settings -> Network ->
   Port Forwarding -> Host Port 2223, Guest Port 22
2. Teraterm Macro::

  hostname = '127.0.0.1:2223'
  username = 'oomichi'
  password = 'XXX'

  COMMAND = hostname
  strconcat COMMAND '/ssh /auth=password /2 /user=' 
  strconcat COMMAND username
  strconcat COMMAND ' /passwd='
  strconcat COMMAND password
  connect COMMAND
  end

1. Try quickstart of Tempest
----------------------------
Run some commands based on the quickstart::

  $ git clone http://git.openstack.org/openstack/tempest
  $ sudo apt-get update
  $ sudo apt-get install python-pip
  $ sudo apt-get install -y libffi-dev
  $ pip install tempest/
  $ export PATH="$PATH:$HOME/.local/bin"
  $ tempest init cloud-01
  $ cd cloud-01

Then if running tempest, it shows errors like::

  {0} setUpClass (tempest.scenario.test_volume_boot_pattern.TestVolumeBootPatternV2) [0.000000s] ... FAILED
  Captured traceback:
  ~~~~~~~~~~~~~~~~~~~
    Traceback (most recent call last):
      File "/home/oomichi/.local/lib/python2.7/site-packages/tempest/test.py", line 274, in setUpClass
        six.reraise(etype, value, trace)
      File "/home/oomichi/.local/lib/python2.7/site-packages/tempest/test.py", line 262, in setUpClass
        cls.setup_credentials()
      File "/home/oomichi/.local/lib/python2.7/site-packages/tempest/test.py", line 362, in setup_credentials
        credential_type=credentials_type)
      File "/home/oomichi/.local/lib/python2.7/site-packages/tempest/test.py", line 520, in get_client_manager
        cred_provider = cls._get_credentials_provider()
      File "/home/oomichi/.local/lib/python2.7/site-packages/tempest/test.py", line 498, in _get_credentials_provider
        identity_version=cls.get_identity_version())
      File "/home/oomichi/.local/lib/python2.7/site-packages/tempest/common/credentials_factory.py", line 77, in get_credentials_provider
        fill_in=True, identity_version=identity_version)
      File "/home/oomichi/.local/lib/python2.7/site-packages/tempest/common/credentials_factory.py", line 185, in get_configured_admin_credentials
        identity_version=identity_version, **params)
      File "/home/oomichi/.local/lib/python2.7/site-packages/tempest/common/credentials_factory.py", line 218, in get_credentials
        **params)
      File "/home/oomichi/.local/lib/python2.7/site-packages/tempest/lib/auth.py", line 644, in get_credentials
        http_timeout=http_timeout)
      File "/home/oomichi/.local/lib/python2.7/site-packages/tempest/lib/auth.py", line 265, in __init__
        super(KeystoneAuthProvider, self).__init__(credentials, scope)
      File "/home/oomichi/.local/lib/python2.7/site-packages/tempest/lib/auth.py", line 93, in __init__
        raise exceptions.InvalidCredentials(message)
    tempest.lib.exceptions.InvalidCredentials: Invalid Credentials
    Details: Credentials are: {'username': None, 'project_name': None, 'user_id': None, 'tenant_name': None, 'tenant_id': None, 'project_id': None} Password is not defined.

We need to set minimum config related to credentials at least.


