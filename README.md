# RKE2 Quickstart

## Ubuntu

```shell
# Ubuntu instructions 
# stop the software firewall
systemctl stop ufw
systemctl disable ufw

# get updates, install nfs, and apply
apt update
apt install nfs-common -y  
apt upgrade -y

# clean up
apt autoremove -y
```

## RKE2 Server Node

```shell
# On server linode
curl -sfL https://get.rke2.io | INSTALL_RKE2_TYPE=server sh - 

# start and enable for restarts - 
systemctl enable rke2-server.service 
systemctl start rke2-server.service
```

```shell
systemctl status rke2-server
```

```shell
cat /var/lib/rancher/rke2/server/node-token
```

```shell
# simlink all the things - kubectl
ln -s $(find /var/lib/rancher/rke2/data/ -name kubectl) /usr/local/bin/kubectl

# add kubectl conf
export KUBECONFIG=/etc/rancher/rke2/rke2.yaml 

# check node status
kubectl  get nodes
```

## RKE2 Agent Nodes

```shell
hostnamectl set-hostname agent1 #agent2, agent3 etc, etc. 
```

```shell
# we add INSTALL_RKE2_TYPE=agent
curl -sfL https://get.rke2.io | INSTALL_RKE2_TYPE=agent sh -  

# create config file
mkdir -p /etc/rancher/rke2/ 

# change the ip to reflect your rancher1 ip
echo "server: https://$RANCHER1_IP:9345" > /etc/rancher/rke2/config.yaml

# change the Token to the one from rancher1 /var/lib/rancher/rke2/server/node-token 
echo "token: $TOKEN" >> /etc/rancher/rke2/config.yaml
```

```shell
vim etc/rancher/rke2/config.yaml
```

```shell
# enable and start
systemctl enable rke2-agent.service
systemctl start rke2-agent.service
```

### journalctl

```shell
 journalctl -u rke2-server -f
```

```shell
 journalctl -u rke2-agent -f
```
