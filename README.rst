OpenStack Tempest testing memo
==============================

1. Try quickstart of Tempest
2. Try non-admin operation
3. Triage the launchpad bugs

Deploy Tempest
--------------

1. Install ubuntu 16.04 on virtualbox. (10 min)::

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

Then we need to make minimum setting at least::

  $ vi etc/tempest.conf
    + [auth]
    + use_dynamic_credentials = False
    + test_accounts_file = /home/<username>/cloud-01/etc/accounts.yaml
    +
    + [identity]
    + auth_version = v2
    + uri = http://<Keystone IP-address>/v2.0/
    +
    + [identity-feature-enabled]
    + api_v3 = false

  $ vi etc/accounts.yaml (The following is just for devstack env as sample)
    + - username: 'demo'
    +   tenant_name: 'demo'
    +   password: 'secret'

At the above setting, Keystone v3 API is disabled because the dev community
is still using v2 API even though that is deprecated.

