# Role: Users
Users role manages user accounts on the managed servers. It can create and remove user, user's group, and ssh key(s). It also maintains ssh keys on the ansible user .

## Usage
Main parts of the role are _users_ dictionary and _enabled_users_ list.

### _users_ dictionary
By default stored in role's defaults/main.yml contains definition of all users managed by this role. Each key in the dictionary represents a username and each value is a dictionary of user's attributes. 

#### attributes

| Attribute | Required | Default | Description |
| --- |---|---|---|
| name | yes | | Description of the user |
| sudo | no | | Whether the user is capable of sudo. If set, the user is also made member of the _wheel_ group and his ssh keys are enabled for the __ user |
| profile | no | | Additional settings in the user's profile, e.g. command aliases. |
| ssh_key | yes | | Ssh keys authorised to connect to this user's account. Multiple keys can be included, each listed on a new line. If user is sudo enabled, the keys will also be authorised to ssh as the __ user used as ansible_user |
| groups | no | ['wheel'] if user is sudo enabled. [] otherwise. | Additional groups the user is member of. If user is sudo enabled, the _wheel_ group is automatically appended to the list. |
| shell | no | bash | User's shell |
| password | no | _no password_ | User's password as a hash |
| uid | no | _automatically assigned_ | User's UID |

The example defines three users, _Osama_, _Mustafa_ and _Oracle_:
~~~
users:
  martin:
    name: Osama Mustafa
    sudo: yes
    profile: |
      alias ll='ls -lah'
    ssh_key: |
      ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDCIWzpWdaLM4h8nZG0ryP/7uboS4oc3pfaXf/AofOjThvr8tOO6gK20/P3kpmWRoNvS5iax1tEaBvszuDs3HY5wFwxv4/919oWkHowoBI9lrHb0/7iOTpLAN4/vKFVHJP25WqbsbQQ/Y09plG2VVXvFVoCnbpLUKM0e4m+SGcWjWf5NtUKBc7JyXCz7FiVmgtPqcOBbodop/Jhh6syoFGeoMopEW4Yv4y5OEVZFEym9odg3D/9OZ9pTTAXkizkr4S06DcGq0aRHo6mI8ybwzoKoClj4o1ojN54sZkzF9TK5ByEH3RNKLOspYioOeoH0ccb9+H510POwwIu+U3JwI8N pegmanm@Darkstar
      ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCmYl27LX2KHOBumJLtnq4qZkqCBs+UyggSRk+5BxO05D/GPhf9HSj6EhhBLrRT2XKCRq9XJf34Hi7qZXLcV9f/k9ye8rf1CcUu67DDAvOGNVJbVcwJ0V+KUq8smJvMT8bkao4zS8HHUA68Xs0J4Hu1MeUcUJean6sYdnWiqjWHbbTid45aKQbYe/0HP/hyqlbeDjJGDi09JAy1zwH+P4Pb24pHtl0uO6OEFHuotliCl+8D35O2+F2Lnh4f3y1zSxNnK6iZfZzhKUjCazZX0/JrDfGQ4M6TxOsYILUVIKMQFMR1SQv6wBXh/5x7AgZdvlYXrhIc5nytqureZ9A4nJ/Z martin.pegman@admins-MacBook-Pro.local
      ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILwa7sm7/MuNYBDUYi6EbBtHsz/GkK5p15heHM9D4SCp martin.pegman@Martins-MacBook-Pro.local
  chantal:
    name: Osama Oracle
    sudo: yes
    profile: |
      alias ll='ls -lahtr'
    ssh_key: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC1ANWkq5V9PU6DPSJB8leS58kWfB6VocyMJnaWBUjGTPraq2oYpqEll9EeOsbcKuMgTJ4d3P63WF4qbwN/XMI1Un2Y5pPGycUQFhqPHThxu3mSTYQsN4lEcmqBUt8MX52U5wRYe/8UvDwrfbN1hFS7ObwJuRMh7a+ygCuPO4QarUJ51hw7W1lswKiMJfzj8nLNDHRWDZFCcMj5fCFk0Qa+MneaXFiCM0uh3ix8QnlGUCPGiZjgZfxqqsFC/GOx0ZQF0hqMiqiAFh0ikQ2upZIRC4SxkQ7jeSc4CVLv0QqYGbl6HHvoiMpweY70rpqPBom1xdbDaCSnWYmaQSKMFIb2F6nnzL1XEC2/PjFPFIMTxSmo6gVV4mMWupQkkTdOIWKcaD3THS/YMy0ubJEXPFPaM/+PlD444+XC9Peas2/RvyJZFPigRhLo2EOcg2x6eY9EeIPM1wMqiOp3xmEVhKg/gBqQ4CJuWsyvwSdqKyh7CotzyL+k/HvPxgVbXW26uZomQOBQkcZo1H/J2dX14SzuDxh0P0/17Vi/nf5GUUGK+HfLf3up9CKgmYtiomyOABcjc05wesfkO5W1EgeWwMSIvUGixeyqU3jll4MvmPWXC1KsK0PLRQIBoFhHiizAhFD+sPHB34bvrzQclbj1WxAjh0PeHlY0jyKRMQ1UTeBo3w== chantal@pegasus.ae
  charlie:
    name: Osama Test
    sudo: yes
    profile: |
      alias ll='ls -lah'
    ssh_key: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDs97sev2/IYsKXZpkNTOdssBArwwkEHccB1775pG/url7pqlfWffQONyvnmhVo8yvvEo1FcYvf35ur4w/j3zUdjj2W5s2j+nENAd/kxpLAgH7Exczwf3voOPpq9WkqevHxt6Vg09b9MubFLnce+GtNFLgibq6L7cz7N3dsmcNqQKckQxfXP8ut6zBld0X7aMHnnsH9oAdFmOS21fjoQO4Tx1gfYiUyIX4fVb/EeIMmuGQNuVeUxK8/9yYUKnovDB9gkoGjy/JAFdVX5HX4NI94V/XYeMvOCds5v/es5OXfO0XlbnaVNEBxMG1bC2VXFu/LDaWyfL6/xWR2zJiovp902nhPTyfOdvm3RVS7dKoX3QERr+zLrxv27MZdUQao8L9SIY5mbPQmYIegn5j9H6Ve0nxGX4r0w4gCSqyglwOaTEECqmpfboWU50oeZlab68k2PYAOqyPEpGsi5hxtI/D3TduwwuOQgjg1JJiRs5UAjtElrF8KwV02v2oOVyQzVgI+wybv1i1QasancOvSsvrzM+YqjkTAivx3/RYcp0b9EezKGeyVTF6liqa5XLpamVdG/+p4ZDgZ8xAbOZ/X77xqymfqn4EmsHiXq2u7Ix8F3vxAuCPKYyU+o9neYNjAMWhdI+LNp/Akc0q6cClkB6l6I3yRli+arBDWV5d2WT61yw== curly@charlie-mac
~~~


### _enabled_users_ list
Usually stored in the inventory in group_vars/all/users.yml. Contains a list of usernames to be present on the particular environment / group / host (depending on where the list is defined). Each value in the list has to match a key from the _users_ dictionary. The role makes sure that all the users from the _enabled_users_ are present on the server and all users not in the list, but defined in the _users_ dictionary are removed.

Following example would make sure that users _Osama_ and _Mustafa_ are present, but the user _Oracle_ is not:
~~~
enabled_users:
  - Osama
  - Mustafa
~~~

