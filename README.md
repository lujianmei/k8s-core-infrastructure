# k8s-core-infrastructure

Using Coreos + Ansible + Kubernetes to create a base infrastructure for production use.

## Install ##
`
git clone https://github.com/lujianmei/k8s-core-infrastructure ~/.tmp/
cd vagrant
vagrant up
`
It will start 6 coreos server, you could absolute change the $num_instances if you do not want to start up 6 server. 

## Change ssh key ##

### Automatic insert into iso ###
Change the ~vagrant/Vangrantfile~ there is a variable named: "insert_ssh_key_into_iso", you may change it to true, and make sure your current working environment is Linux, which the insert_ssh_keys_into_iso.sh can be run.

### Manually insert ###
`
vagrant ssh core-0$num # to login into virtual server one by one
sudo passwd core  # to change the password
`
Then change password value in the copy_ssh_keys.sh file, execute:

`
./copy_ssh_keys.sh
`

Or if you in MacOS, the script can not be ok, you need to change the ~copy_ssh_keys_onmac.sh~ and execute:

`
./copy_ssh_keys_onmac.sh
`

## Initial the python environment and startup coreos ##
`
cd k8s-core-infrastructure
ansible-galaxy install -r requirements.yml -p ./roles
ansible-playbook -i inventory/staging/hosts bootstrap-coreos.yml -D
`
Which will initial all coreos servers
Also, you will need to add all coreos virtual servers hostname and their ip address into you hosts file of current working environment.

## Initial kubernetes environment ##
`
ansible-playbook -i inventory/staging/hosts bootstrap-kubernetes.yml -D -e k8s_role_path="./roles/"
`
Run to install all kubernetes servers
