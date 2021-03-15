# Provisioning

Ansible scripts for provisioning  instances

## Usage
### Prerequisites

You need to either have Docker installed to be able to run ansible (>=2.8.0) runner image (unless you want to install Ansible and all its dependencies locally)
You also need to have:
- ssh access to the provisioned instance
- azure credentials in ~/.azure/credentials

~~~
$ sudo docker run --rm -tiv $(pwd):/workdir -v /home/alesh/.azure:/root/.azure -v $SSH_AUTH_SOCK:/tmp/ssh-agent -e "SSH_AUTH_SOCK=/tmp/ssh-agent"  docker.graphaware.com/internal/ansible ansible-playbook playbook.yml --check --limit tag_ansible_enabled
~~~

This is to allow you for example , change anything in your instance using ansible , in case new employee join the company and wants to add the SSH key for him/Her
All you have to do , edit the SSH and it will run automatcially depends on your timeline. 