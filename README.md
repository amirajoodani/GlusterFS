![gluster-ant](https://github.com/user-attachments/assets/27d9dc0e-9828-447c-a3fb-82a7271c947b)
# What is GlusterFS ? 
Gluster is a free and open source software scalable network filesystem. <br>
it use Disk --> Brick --> Volume to serve client data <br>
we can config it in two ways : <br>
1- Replicated HA <br>
2- Distributed Scale <br>
in Replication HA Method , if you Write 1G Data to DB , Based on your Replicaset , for example 3 , You have 3 instabce from your data and of course your data occupied 3G on your servers . <br>
now if you have one big file , for example 150 GB , and your disks on linux server have just 100 GB per nodes , you can not write yor data file into your replicated cluster and you should use <b>DISTRIBUTED<b> mode . <br>
