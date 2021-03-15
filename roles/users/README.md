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
      Your-key
  chantal:
    name: Osama Oracle
    sudo: yes
    profile: |
      alias ll='ls -lahtr'
    ssh_key: Your-key
  charlie:
    name: Osama Test
    sudo: yes
    profile: |
      alias ll='ls -lah'
    ssh_key: Your-key
~~~


### _enabled_users_ list
Usually stored in the inventory in group_vars/all/users.yml. Contains a list of usernames to be present on the particular environment / group / host (depending on where the list is defined). Each value in the list has to match a key from the _users_ dictionary. The role makes sure that all the users from the _enabled_users_ are present on the server and all users not in the list, but defined in the _users_ dictionary are removed.

Following example would make sure that users _Osama_ and _Mustafa_ are present, but the user _Oracle_ is not:
~~~
enabled_users:
  - Osama
  - Mustafa
~~~

