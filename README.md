#Docker Environment for Mac Silicon based on Parallels

Problem: Docker Desktop comes with a terrible performance, mainly caused by poor File I/O. Previuous solution with Docker Machine and parallels does not work on Silicon because its base on x86 for parallels. 

Solution: Provisions an ARM based Docker environment into Parallels for M1 (v17+).

## Preconditions
### Parallels
Install Parallels Desktop for Mac Pro Edition: https://www.parallels.com/directdownload/pd17/image/?experience=enter_key. Note: you will need a license for Parallels.

### Setup Docker machine
Provisioning is based on Vagrant and Ansible. Vagrant needs parallels provider to work with Parallels. 

```
brew install vagrant ansible 
vagrant plugin install vagrant-parallels 
vagrant plugin install vagrant-hostmanager
```
Provisioning and Docker connection is based on SSH and assumes you have your public SSH key located in ```~/.ssh/id_rsa.pub```.

After cloning the repo simply run ```vagrant up``` in the root folder. You will be asked for password for updating /etc/hosts file. Default name for the docker host is "docker-vagrant" Machine is created with 8GB Ram and 4 CPU Cores. Disk size is 64GB.

if you need additional hostname to be added, uncomment the according line in Vagrantfile 
```
# uncomment this, if you need to add additional hostnames
# config.hostmanager.aliases = %w(example-host)
```
and run 
```
vagrant hostmanager
```

###Run Docker
If you previously installed Docker Desktop all dockker command line tools are installed on your machine. If not install from homebrew ```brew install docker```.

To attach the provisioned machine you need to set your environment:
```
export DOCKER_HOST=ssh://vagrant@docker-vagrant
```

You are ready to go.
###Solve problems with base images
Especialy older version are not available as arm images. Here are some links for Dockerfiles to solve those problems:
* MySQL 5.7: https://github.com/beercan1989/docker-arm-mysql
* t.b.c.

## Todos
* find out how to properly resize disk or provision a machine with bigger disk



