
<font size="8">**Installation notes<br> R730 AIO DX cloud with one worker**</font>

<style>
pre {
  color: blue;
}
</style>

````
147.11.90.20    : Dell R730 DX Floating
147.11.90.21    : Dell R730 Controller-0   (iDRAC 147.11.88.52 root/windriver)
147.11.90.22    : Dell R730 Controller-1   (iDRAC 147.11.88.53 root/windriver)
                : SuperMicro               (BMC 147.11.88.50 ADMIN/ADMIN)
````

---
#### eno2 1G interface on r730s is used for OAM
#### eno1 1G interface is used for Management and Cluster networks


---
### Installation  Overview  


Before creating the custom ISO, one change was made in stx-iso-utils.sh script:  

````
$ diff -u stx-iso-utils.sh.org stx-iso-utils.sh  
--- stx-iso-utils.sh.org      2020-07-31 10:36:50.271257796 -0700  
+++ stx-iso-utils.sh    2020-07-31 10:39:13.106568428 -0700  
@@ -13,6 +13,12 @@
declare MNTDIR=
declare WORKDIR=

+declare LOG_TAG=$(basename $0)
+
+function log_error {
+    logger -i -s -t ${LOG_TAG} -- "$@"
+}
+
function common_cleanup {
     unmount_efiboot_img
````
### Create custom addon file, generate the custom ISO, copy it to Virtual Media folder on PXE server /var/www/html, and start Serial over Lan console  

````
cat <<EOF > dell-r730-ks-addon.cfg
####start ks-addon.cfg
OAM_DEV=eno2
LOCAL=eno1

cat << EOF > /etc/sysconfig/network-scripts/ifcfg-$OAM_DEV
DEVICE=$OAM_DEV
BOOTPROTO=none
ONBOOT=yes
IPADDR=147.11.90.21
PREFIX=22
GATEWAY=147.11.88.1
DNS1=147.11.57.133
DNS2=147.11.57.128
EOF

cat << EOF > /etc/sysconfig/network-scripts/ifcfg-$LOCAL
DEVICE=$LOCAL
BOOTPROTO=none
EOF

####end ks-addon.cfg
EOF
````
Update ISO with custom parameters previously defined 
````
$ sudo ./update-iso.sh -i wind-river-cloud-platform-host-installer-20.06-38-PATCH_0001.iso -o wind-river-cloud-platform-host-installer-20.06-38-PATCH_0001-dellR730.iso -d 4 -t 3 -a dell-r730-ks-addon.cfg
````

Copy Custom ISO to /var/www/html Virtual Medial folder 
````
$ cp wind-river-cloud-platform-host-installer-20.06-38-PATCH_0001-dellR730.iso /var/www/html/.
````

Start Serial Over Lan session using IPMI tool 
````
$ ipmitool -I lanplus -U root -P windriver -H 147.11.88.53 sol activate
````

Note:  

	Call the Dell Redfish python files commands to boot from the custom ISO  

Retrive Dell RedFish Python script and Prepare the environment
````
$ mkdir dell; cd dell
$ git clone https://github.com/dell/iDRAC-Redfish-Scripting
$ cd iDRAC-Redfish-Scripting/Redfish\ Python
````
Unmount any mounted Virtual Media:`
````
$ python3 InsertEjectVirtualMediaREDFISH.py -ip 147.11.88.53 -u root -p windriver -o 2 -d 1
````
Mount previously created Custom ISO:
````
$ python3 InsertEjectVirtualMediaREDFISH.py -ip 147.11.88.53 -u root -p windriver -o 1 -d 1 -i http://147.11.88.5/wind-river-cloud-platform-host-installer-20.06-38-PATCH_0001-dellR730.iso
````
Force Next One Time Boot from Virtual Media and restart the server:
````
$ python3  SetNextOneTimeBootVirtualMediaDeviceOemREDFISH.py -ip  147.11.88.53 -u root -p windriver -d 1 -r y
````

In case after ISO has been installed IP configuration and routing needs to be adjusted, from SOL or Virtual console on controller-0 change IP address and default GW as shown here:

````
$ sudo bash
ip addr add 147.11.90.21/22 dev eno1
ip link set eno1 up
ip route add default via 147.11.88.1 dev eno1
exit
````

Note:  

Followng Installation steps are also described in StrlingX AIO DX installation procedure:  

https://docs.starlingx.io/deploy_install_guides/r4_release/virtual/aio_duplex.html


#### Create localhost.yaml file with system mode(duplex), IP configuration, Container registry access and object information
````
cat <<EOF > localhost.yml
system_mode: duplex
timezone: UTC

dns_servers:
  - 147.11.57.133
  - 147.11.57.128

name: "wwt-cloud"
description: "WRCP R730 AIO DX Cloud"
location: "alameda-ca-usa"
contact: "stephen.gooch@windriver.com"
management_subnet: 192.168.5.0/24
management_start_address: 192.168.5.2
management_end_address: 192.168.5.50
cluster_host_subnet: 192.168.6.0/24
cluster_pod_subnet: 172.16.0.0/16
cluster_service_subnet: 10.96.0.0/12
external_oam_subnet: 147.11.88.0/22
external_oam_gateway_address: 147.11.88.1
external_oam_floating_address: 147.11.90.20
external_oam_node_0_address: 147.11.90.21
external_oam_node_1_address: 147.11.90.22

docker_registries:
 defaults:
    username: AKIAZDKOSK7ZMIIWRKP4
    password: SBM13C23dbR1zGyWj6RRjyWcLPtVNDK/iBPoV2UY

additional_local_registry_images:
  - docker.io/wind-river/cloud-platform-deployment-manager:WRCP_20.06
  - gcr.io/kubebuilder/kube-rbac-proxy:v0.4.0
  - docker.io/wind-river/cmk:WRCP.20.01-v1.3.1-15-ge3df769-1
  - docker.io/starlingx/rvmc:stx.4.0-v1.0.0
  - quay.io/external_storage/rbd-provisioner:v2.1.1-k8s1.11
  - docker.io/starlingx/ceph-config-helper:v1.15.0

admin_password: <admin password>
ansible_become_pass: <sysadmin password>

EOF
````
Bootrap Controller-0
````
ansible-playbook /usr/share/ansible/stx-ansible/playbooks/bootstrap.yml
````

---

# Configure System Controller-0 and unlock it  

````
source /etc/platform/openrc
system host-if-list controller-0
system interface-network-list controller-0

OAM_IF=eno2
MGMT_IF=eno1
system host-if-modify controller-0 lo -c none

IFNET_UUIDS=$(system interface-network-list controller-0 | awk '{if ($6=="lo") print $4;}')
for UUID in $IFNET_UUIDS; do     system interface-network-remove ${UUID}; done

system host-if-modify controller-0 $OAM_IF -c platform
system interface-network-assign controller-0 $OAM_IF oam
system host-if-modify controller-0 $MGMT_IF -c platform
system interface-network-assign controller-0 $MGMT_IF mgmt
system interface-network-assign controller-0 $MGMT_IF cluster-host

system ntp-modify ntpservers=ntp-1.wrs.com 

system storage-backend-add ceph --confirmed
system host-disk-list controller-0
system host-disk-list controller-0 | awk '/\/dev\/sdb/{print $2}' | xargs -i system host-stor-add controller-0 {}
system host-stor-list controller-0

`````
Unlock controller-0

````
system host-unlock controller-0
`````
Install Licence
````
system license-install wrslicense-wrcp-2006-Dec-31-2025.lic
````
### Add controller-1  

use MAC address of PXE/MGMT interface to add controller-1 host  
On contoller-0:
````
system host-add -p controller -m ec:f4:bb:db:98:40 -o text -c ttyS0,115200 (Use MAC address of your PXE interface)
`````
initiate PXE boot on controller-1 node using IPMI tool or any other means  
`````
ipmitool -I lanplus -U <username> -P <password> -H <BMC IP> power off
ipmitool -I lanplus -U <username> -P <password> -H <BMC IP> chassis bootparam set bootflag force_pxe
ipmitool -I lanplus -U <username> -P <password> -H <BMC IP> power on
`````

Once Controller-1 completed PXE boot from Controller-0 it will become "available"  

Check availability with "system host-list" command  
On controller-0:
````
$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | available    |
+----+--------------+-------------+----------------+-------------+--------------+
````

````
OAM_IF=eno2
system host-if-modify controller-1 $OAM_IF -c platform
system interface-network-assign controller-1 $OAM_IF oam
system interface-network-assign controller-1 mgmt0 cluster-host
````

````
$ system host-if-list controller-1
+--------------------------------------+-------+----------+----------+---------+-----------+----------+-------------+------------+
| uuid                                 | name  | class    | type     | vlan id | ports     | uses i/f | used by i/f | attributes |
+--------------------------------------+-------+----------+----------+---------+-----------+----------+-------------+------------+
| 261d934f-192f-4656-9bb0-5945d86cf109 | eno2  | platform | ethernet | None    | [u'eno2'] | []       | []          | MTU=1500   |
| 26614c98-ffd4-443a-809e-9e7e4e4e86b1 | mgmt0 | platform | ethernet | None    | [u'eno1'] | []       | []          | MTU=1500   |
+--------------------------------------+-------+----------+----------+---------+-----------+----------+-------------+------------+
````
````
$ system interface-network-list controller-1
+--------------+--------------------------------------+--------+--------------+
| hostname     | uuid                                 | ifname | network_name |
+--------------+--------------------------------------+--------+--------------+
| controller-1 | 7c5341a9-6464-4f0c-8440-761e913bb2fe | mgmt0  | mgmt         |
| controller-1 | c8cafdba-7b5e-41ff-b6ee-2c0e52b05734 | eno2   | oam          |
| controller-1 | e50a4091-0d47-49bc-8e83-109815081a5a | mgmt0  | cluster-host |
+--------------+--------------------------------------+--------+--------------+
````

````
system host-disk-list controller-1
+--------------------------------------+-------------+------------+-------------+----------+---------------+--------------+----------------------------------+-------------------------------------------------+
| uuid                                 | device_node | device_num | device_type | size_gib | available_gib | rpm          | serial_id                        | device_path                                     |
+--------------------------------------+-------------+------------+-------------+----------+---------------+--------------+----------------------------------+-------------------------------------------------+
| 23d2e82e-55a0-4c97-9c71-afca293fbf2e | /dev/sda    | 2048       | HDD         | 223.0    | 5.918         | Undetermined | 007241e6946b34bc260082202e20844a | /dev/disk/by-path/pci-0000:03:00.0-scsi-0:2:0:0 |
| 1683a891-487c-41ff-87b7-af5e831d8f99 | /dev/sdb    | 2064       | HDD         | 223.0    | 0.0           | Undetermined | 00ad2d41a46c35bc260082202e20844a | /dev/disk/by-path/pci-0000:03:00.0-scsi-0:2:1:0 |
+--------------------------------------+-------------+------------+-------------+----------+---------------+--------------+----------------------------------+-------------------------------------------------+
````
````
system host-disk-list controller-1 | awk '/\/dev\/sdb/{print $2}' | xargs -i system host-stor-add controller-1 {}
````
````
$ system host-stor-list controller-1
+--------------------------------------+----------+-------+------------+--------------------------------------+-------------------------------------------------------+--------------+------------------+-----------+
| uuid                                 | function | osdid | state      | idisk_uuid                           | journal_path                                          | journal_node | journal_size_gib | tier_name |
+--------------------------------------+----------+-------+------------+--------------------------------------+-------------------------------------------------------+--------------+------------------+-----------+
| cd960f74-2759-4c8e-b1c1-ed6de2dab6cb | osd      | 1     | configured | 1683a891-487c-41ff-87b7-af5e831d8f99 | /dev/disk/by-path/pci-0000:03:00.0-scsi-0:2:1:0-part2 | /dev/sdb2    | 1                | storage   |
+--------------------------------------+----------+-------+------------+--------------------------------------+-------------------------------------------------------+--------------+------------------+-----------+
````

````
system host-unlock controller-1
````

### Add worker-0 (former compute-10)  

IPMI MGT http://147.11.88.50 ADMIN/ADMIN  

691	g11	MAC Address	00:25:90:FD:DA:10	MAC Address	00:25:90:FD:DA:10	compute-10  

10G swicth interface qe9 and xe35  

`````
system host-add -p worker -n worker-0 -m  00:25:90:fd:da:10 -o text -c ttyS0,115200

ipmitool -I lanplus -U ADMIN -P ADMIN -H 147.11.88.50 sol activate

system interface-network-assign worker-0 mgmt0 cluster-host
`````
Validate Interfaces properly created 
`````
$ system host-if-list worker-0
+--------------------------------------+-------+----------+----------+---------+---------------+----------+-------------+------------+
| uuid                                 | name  | class    | type     | vlan id | ports         | uses i/f | used by i/f | attributes |
+--------------------------------------+-------+----------+----------+---------+---------------+----------+-------------+------------+
| 4a51a822-8b02-4cc2-9dcb-0ef1a2ce01c7 | mgmt0 | platform | ethernet | None    | [u'enp1s0f0'] | []       | []          | MTU=1500   |
+--------------------------------------+-------+----------+----------+---------+---------------+----------+-------------+------------+
`````

`````
$ system interface-network-list worker-0
+----------+--------------------------------------+--------+--------------+
| hostname | uuid                                 | ifname | network_name |
+----------+--------------------------------------+--------+--------------+
| worker-0 | 982af309-ccbf-4643-a34f-b3067aeef848 | mgmt0  | cluster-host |
| worker-0 | bb718e11-241a-439b-bf46-96767a8e166f | mgmt0  | mgmt         |
+----------+--------------------------------------+--------+--------------+
`````
Ulock worker node
````
system host-unlock worker-0
`````
````
$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | worker-0     | worker      | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
`````

==============================
Deployment Manager Installation
==============================
````
 ./deploy build -n deployment -s wwt-cloud --minimal-config
````
````
cat <<EOF > /home/sysadmin/dm-helm-overrides.yaml
manager:
  image:
    repository: registry.local:9001/docker.io/wind-river/cloud-platform-deployment-manager 
    tag: WRCP_20.06
    pullPolicy: IfNotPresent
rbacProxy:
    image: registry.local:9001/gcr.io/kubebuilder/kube-rbac-proxy:v0.4.0
EOF
````
````
cat <<EOF > /home/sysadmin/dm-playbook-overrides.yaml
---
deployment_config: /home/sysadmin/cloud-deployment.yaml
deployment_manager_overrides: /home/sysadmin/dm-helm-overrides.yaml
deployment_manager_chart: /home/sysadmin/wind-river-cloud-platform-deploymentmanager-2.0.5.tgz
ansible_become_pass: Li69nux*
EOF
````
````
cat <<EOF > /home/sysadmin/master-bootstrap-and-deploy-playbook.yaml
- import_playbook: /usr/share/ansible/stx-ansible/playbooks/bootstrap.yml
- import_playbook: /home/sysadmin/wind-river-cloud-platform-deployment-manager.yaml
EOF
````

````
cp  deployment-config.yaml cloud-deployment.yaml
````
````
Note:  
System name in cloud-deployment.yaml file shoul not have Capital letters
````
ansible-playbook /home/sysadmin/master-bootstrap-and-deploy-playbook.yaml -e "@/home/sysadmin/dm-playbook-overrides.yaml"
````











# redeploy
</span>
