Install the Canonical userdir-ldap package and configure it for use with
Canonical infra.

The default options should be fine for our normal use.  The only one
you may want to pay attention to is "users-to-migrate", which is a
space-separated list of usernames whose ssh keys will be copied from
~/.ssh/authorized_keys to /etc/ssh/user-authorized_keys.  By default
this is just the ubuntu user, as without this Juju will break, but
if you need to add others, this is the place to do it.

Units of the userdir-ldap charm can also be cascaded. Cascaded units
will take userdata from their upstream userdir-ldap units. To do this a
"server" application can be related to a "client" application via the
udprovide and udconsume relations. The "server" unit will attempt to
rsync userdata from userdb.internal for the related "client"
units. Note that userdb.internal may be configured to only allow
syncing of the "template-hostname" userdata. In this case, "client"
units will only be able to get userdata for "template-hostname" as
well. See the bundle in "./tests/functional/tests/bundles/bionic.yaml"
for an example.

Design note: with cascaded userdir-ldap units, user data is coming
into the "server" from userdb-host unit via two paths:

- rsynced via ud-replicate, where ud-replicate will process userdata
  for local consumption

- straight rsync, without postprocessing, where client units in turn
  will be able to ud-replicate from



