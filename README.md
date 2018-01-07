# Installation of a Apache Cloudstack cluster over Centos7 

### Remove the old guests
```
virsh destroy cloudstack1;virsh undefine cloudstack1;rm -rf /opt/cloudstack/guests/cloudstack1/
```

## If this is a reinstallation then have to clean the old SSH fingerprints
```
ssh-keygen -R 192.168.40.91 -f /home/fernando.hackbart/.ssh/known_hosts

```

## Create new guests
```
mkdir -p /opt/cloudstack/guests/cloudstack1/

virt-install \
 -n cloudstack1 \
 --description="Cloudstack 1" \
 --os-type=Linux \
 --os-variant=generic \
 --ram=2048 \
 --vcpus=1 \
 --graphics=vnc \
 --noautoconsole \
 --disk path=/opt/cloudstack/guests/cloudstack1/cloudstack1.img,bus=virtio,size=10 \
 --pxe \
 --boot=hd,network \
 --network=bridge:dcos-br0,model=virtio,mac=52:54:00:e2:87:91
```

## After the installation of the Linux boxes the same are shutdown, should start them
```
virsh start cloudstack1
```

## Just to add the new SSH fingerprints
```
ssh root@192.168.40.91 date
```

## Clone this Ansible deployment playbook
```
git clone https://github.com/fernandohackbart/ansible-cloudstack-centos.git
```

## Run the Ansible deployment playbook
```
cd ansible-cloudstack-centos
ansible-playbook -i hosts cloudstack-centos7.yml
```

## CloudStack console

http://192.168.40.91:8080/client

