# Docker Environment for Mac Silicon based on Parallels

Problem: Docker Desktop comes with a terrible performance, mainly caused by poor File I/O. Our previous solution with Docker Machine and Parallels does not work on Silicon because its based on x86 for Parallels. Additionaly docker-machine project seems to be dead or at leased unmaintained.

My goal was to build an ARM based Docker environment into Parallels for M1 (v17+).

### Install Parallels
Install Parallels Desktop for Mac Pro Edition: 
https://www.parallels.com/directdownload/pd17/image/?experience=enter_key. 
Note: you will need a license for Parallels.

### Setup a Docker machine with Vagrant and Ansible
Provisioning is based on Vagrant and Ansible. Vagrant needs Parallels provider to work with Parallels VM.

```
brew install vagrant ansible 
vagrant plugin install vagrant-parallels 
vagrant plugin install vagrant-hostmanager
```
Provisioning and Docker connection is based on SSH and assumes you have your public SSH key located in ```~/.ssh/id_rsa.pub```.

Before starting create a local copy of the configuration file. It contains some parameters needed by Vagrant and ansible. Change it after your needs. Machine is created with name "docker-vagrant", 8GB Ram and 4 CPU Cores bydefault. .
```
cp config.yml.dist config.yml
```

Now simply run ```vagrant up``` in the root folder. You will be asked for password for updating /etc/hosts file. 

### Additional host names
if you need additional hostnames to be added, add them to the config.yml and run 
```
vagrant hostmanager
```
### Increase Disk size
Default Disk size is 64GB. If you need a bigger disk, there is currently no other way then resizing the disk of the vagrant box base image, before you provision it.

In case you allready a vagrant environment based on that base image you will have to destroy your vagrant environment and the original box. 
```
vagrant destroy
vagrant box remove mpasternak/focal64-arm
```
Now we pull a fresh version of the box, and resize the disk. Path name of the box might differ in the future when you use other base images or you pull new Versions of the box. You will find out exploring ```~/.vagrant.d/boxes/```.

```
vagrant box add mpasternak/focal64-arm
cd ~/.vagrant.d/boxes/mpasternak-VAGRANTSLASH-focal64-arm/202110.0.0/parallels/Ubuntu\ Linux.pvm
prl_disk_tool resize --hdd ./Ubuntu\ Linux-0.hdd  --size 128G
```
### Run Docker
If you previously installed Docker Desktop all docker command line tools are installed on your machine. If not install from homebrew ```brew install docker```.

To attach the provisioned machine you need to set your environment:
```
export DOCKER_HOST=ssh://vagrant@docker-vagrant
```

You are ready to go. Having set the DOCKER_HOST in the environment, you can use docker and docker-compose like you did before.
### Solve problems with base images
Especialy older version are not available as arm images. Here are some links for Dockerfiles to solve those problems:
* MySQL 5.7: https://github.com/beercan1989/docker-arm-mysql
* t.b.c.





