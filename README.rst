OpenStack Tempest testing memo
==============================

1. Try quickstart of Tempest
2. Triage the launchpad bugs

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
After that, it is nice to run the simplest test as the first step like::

  $ tempest run --regex tempest.api.compute.flavors.test_flavors.FlavorsV2TestJSON.test_list_flavors_limit_results
  [..]
  ======
  Totals
  ======
  Ran: 1 tests in 5.0000 sec.
   - Passed: 1
   - Skipped: 0
   - Expected Fail: 0
   - Unexpected Success: 0
   - Failed: 0
  Sum of execute time for each test: 0.1557 sec.

  ==============
  Worker Balance
  ==============
   - Worker 0 (1 tests) => 0:00:00.155677
  $

The reason why the test is the simplest is every cloud should contain one
flavor at least and the test just checks the existence. We don't need to
specify any other items on the above minimum configuration.

