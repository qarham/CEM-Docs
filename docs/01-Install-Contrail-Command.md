# ![alt text](images/CC-Logo.png) Installation of Contrail Command

Please follow below instructions for Contrail command installation and 

## Contrail Command 5.0.1 GA Procedure

For Contrail Command GA please follow following steps:

* Install Docker

```bash
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install -y docker-ce
systemctl start docker
 ```

* Setup insecure registry for internal docker registry. This step is not required for "hub.juniper.net"

```bash
vi /etc/docker/daemon.json

# Add following line to the file
{"insecure-registries": ["ci-repo.englab.juniper.net:5010"]}

# Svae changes & restart docker
systemctl restart docker

 ```

* Download reference "comman_servers.tml" and update the config as per your environment.

```bash
cd /opt
wget https://raw.githubusercontent.com/qarham/cfm-vagrant/master/docs/scripts/command_servers.yml

# Now please make changes in config file
vi command_servers.yml
 ```



* Internal Repo

```bash
# Use following command for internal registry
docker pull ci-repo.englab.juniper.net:5010/contrail-command-deployer:5.0-214

docker run -t --net host -v /opt/command_servers.yml:/command_servers.yml -d --privileged --name contrail_command_deployer ci-repo.englab.juniper.net:5010/contrail-command-deployer:5.0-214
 ```

* External Repo "hub.juniper.net"

```bash
# For external use following steps
docker login hub.juniper.net/contrail
# Provide username/password
# Once login pull Contrail Command image using
docker pull hub.juniper.net/contrail/contrail-command-deployer:5.0.1-0.214

# AFter that please use following command to bring contrail command up.  
docker run -t --net host -v /opt/command_servers.yml:/command_servers.yml -d --privileged --name contrail_command_deployer hub.juniper.net/contrail/contrail-command-deployer:5.0.1-0.214
 ```

***Note*** Reference [Contrail Comman Servers File](https://raw.githubusercontent.com/qarham/cfm-vagrant/master/docs/scripts/command_servers.yml)

 Now to check the progress of installation use "docker log" command

 ```bash
docker logs -f contrail_command_deployer
 ```

***Here is recorded screen session for Contrail Command Installation***

[![asciicast](https://asciinema.org/a/mXerf6Q7zyP1rV0xkS6UoMzVm.png)](https://asciinema.org/a/mXerf6Q7zyP1rV0xkS6UoMzVm)

## Contrail Command GUI Access

After installation please use following URL for Contrail Command UI access.

* https://192.168.2.10:9091
    * Username/Password: admin/contrail123

### References

* <https://github.com/Juniper/contrail-ansible-deployer/wiki>
* <https://github.com/Juniper/vqfx10k-vagrant>