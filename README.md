![gluster-ant](https://github.com/user-attachments/assets/27d9dc0e-9828-447c-a3fb-82a7271c947b)
# What is GlusterFS ? 
- Gluster is a free and open source software scalable network filesystem. <br>
- it use Disk --> Brick --> Volume to serve client data <br>
- we can config it in two ways : <br>
1- Replicated HA <br>
2- Distributed Scale <br>
- in Replication HA Method , if you Write 1G Data to DB , Based on your Replicaset , for example 3 , You have 3 instabce from your data and of course your data occupied 3G on your servers . now if one of your server goes down , you can retrive your data from other nodes . <br>
- now if you have one big file , for example 150 GB , and your disks on linux server have just 100 GB per nodes , you can not write yor data file into your replicated cluster and you should use <b>DISTRIBUTED</b> mode . in this mode , for example if you have 3 server with 100 GB stroage for glusterfs , now you have totally 300GB for storing data . but in Replicated Method you just have 100GB Storage  . <b>BUT</b> if one of your server goes down in distributed method , you have lost you data . <br>
- we can use two protocol to communicate between client and cluster . one of them is nfs and another is Gluster Protocol . 
![gluster](https://github.com/user-attachments/assets/8daa636d-d1fe-44c0-a57d-8b1cb5f99653) <br>
# DIAGRAM :
<img width="543" height="420" alt="gluster (1)" src="https://github.com/user-attachments/assets/692b9051-17cb-4bb8-b0d1-6adf4a28dcc7" /> <br>
every server has two networks : <br>
1- for public access <br>
2- for disk replication <br>
server gl1 : <br>
<img width="791" height="491" alt="gl1" src="https://github.com/user-attachments/assets/3f2ff47c-d703-4663-8b09-4443a7cd6078" /> <br>
server gl2 : <br>
<img width="783" height="493" alt="gl2" src="https://github.com/user-attachments/assets/45514d26-05d8-41ff-a373-2bdbfaf145d5" /> <br>
server gl3: <br>
<img width="790" height="492" alt="gl3" src="https://github.com/user-attachments/assets/be82bb95-c1ae-4b18-8b5e-8954d33d122c" /> <br>
now we should use lvm to create lvm partions fore sdb and sdc on all servers . <br>
``` bash
# pvs
# pvcreate /dev/sdb
# vgs
# vgcreate glustervg /dev/sdb
# lvs
# lvcreate -l 100%VG -n glusterlv glustervg
# lvs
# lsblk -l
# mkdir /glustervolume
# vi /etc/fstab
/dev/glustervg/glusterlv /glustervolume ext4 deafults 0 0
# mount -a
```

now install glusterfs : <br>
``` bash
# apt update
# apt -y install glusterfs-server
# systemctl status glusterd
# systemctl enable glusterd
# systemctl start glusterd
```
no we should create storage pool . all nodes should see each other and can ping them with name 

on all nodes : <br>
``` bash
#vi /etc/hosts
192.168.1.201 gl1
192.168.1.202 gl2
192.168.1.203 gl3
```
just on first node :
``` bash
(node1)
# gluster peer probe gl2
# gluster peer probe gl3
# gluster peer status
# gluster pool list
```
now we create volume (brick)(on all nodes): <br>
``` bash
# mkdir glustervolume/vol1
```
on one node : <br>
```bash
# gluster volume create vol1 replica 3 gl1:/glustervolume/vol1 gl2:/glustervolume/vol1 gl3:/glustervolume/vol1
# gluster volume start vol1
# gluster volume status vol1
 ```






