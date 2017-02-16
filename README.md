#Docker swarm cluster

This demonstration will cover

- Provisioning multiple instances with vagrant
- Deploying a cluster which is capable of managing docker images
- Deploy a docker image to the cluster 
- Demonstrate cluster configuration during runtime with ansible (post_provision_example)
- Demonstrate a mechanism for deploying applications (Jenkins server with docker swarm plugin)

###Installation

```
pip install virtualenv
virtualenv env
source env/bin/activate
pip install -r requirements.txt
vagrant up
```

###Dependencies

```
python with pip
vagrant with vagrant-address (0.3.1) vagrant-hostmanager (1.8.5) vagrant-hosts (2.8.0)
```


###Process

`Vagrantfile` will provision ubuntu hosts.
`Ansible` playbooks are automatically kicked off as part of the provisioning step in vagrant.
`Docker swarm` is formed after provisioning by using clever DNS resolution between containers.

###Technologies

```
ansible -> Orchestration
vagrant -> Provisioning
docker  -> Deployment/Provisioning
```