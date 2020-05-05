## ansible-role-keepalived
ci-gateway
=========

Provision a Highly available gateway using `keepalived`, and `conntrackd`. This
allows us to have an Active/Backup setup for external IPs, and internal IPs. 

The internal balanced IPs are meant to serve as NAT gateways for IPv4 hosts on
the internal network. 

The external IPs are meant to host services and either proxy back to or port-forward to internal servicesp.


Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

- ansible-role-iptables (this is a soft dependency for the test setup)

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: ci_gateway, x: 42 }

Testing
-------

A [Vagrantfile](Vagrantfile) is provided for manual testing purposes. You may
run `vagrant up` to get a test environment with 2 "gateways" and 1 "client"

To test that the gateway is working you need to reconfigure the client to point
at the internal VIP as a default route:

```shell
[root@client ~]# ip route del default

[root@client ~]# ip route add default via 192.168.33.254 dev eth1
```

License
-------

MIT

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
