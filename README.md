# PE Console RBAC APIs

Puppet Enterprise 3.7 introduced a new Role Based Access Control layer. This
enables you to manage the permissions of local users as well as those who are
created remotely, on a directory service in very granular detail.

This module exposes some of it to the Puppet DSL. Currently, it manages
users, roles, permissions, and groups.

## Usage

``` Puppet
rbac_user { 'testing account':
  ensure       => present,
  name         => 'testing',
  display_name => 'Just a testing account',
  email        => 'testing@puppetlabs.com',
  password     => 'puppetlabs',
  roles        => [ 1,2 ],
}

rbac_role { 'Viewers':
  ensure      => 'present',
  description => 'Viewers',
  permissions => [
  {
    'object_type' => 'nodes',
    'action' => 'view_data',
    'instance' => '*'
  },
  {
    'object_type' => 'console_page',
    'action' => 'view',
    'instance' => '*'
  }],
}

rbac_group { 'admins':
  ensure => 'present',
  roles  => ['Administrators'],
}

```

## Limitations

The API does not currently allow you to update existing users, other than to
revoke the account, or update the roles attached to the user. When you ensure an
`rbac_user` is absent, the record will not be removed, just marked as revoked.

For a node that is not a standalone master to manage RBAC users, its certname
must be listed in the Console node's RBAC certificate whitelist.

Contact
-------

education@puppetlabs.com
