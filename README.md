# nfs-deploy-centos7
Documentation of how to deploy NFS servers in CentOS 7 machines

To deploy NFS services we will need 2 machines. One will be our nfs-server and other will be the nfs-client. 
NFS server will work as the file hosting platform and client will be able to read, write and create new files in server side. 

## NFS Server Side Configurations: 

Installing nfs packages in the server side -
```
yum install nfs-utils nfs-utils-lib
```

Creating a new directory for nfs share - 
```
mkdir /nfs-share
```

Change the directory permissions - 
```
chmod -R 755 /nfs-share
chown nfsnobody:nfsnobody /nfs-share
```

Start services in the server machine - 
```
systemctl start nfs-server
systemctl enable nfs-server
```

Now we need to configure the exports file to enable sharing for the clients. So we will open the exports file and add the line -  
```
vi /etc/exports
/nfs-share 192.168.1.250(rw,sync,no_root_squash)
```
In the above command, change your the IP address to your clients IP or we can use * to allow all users from this server. We can also allow specific block of IPs for any specific share - 
```
/nfs-share *(rw,sync,no_root_squash) ## Allow all IPs
/nfs-share 192.168.1.1/24(rw,sync,no_root_squash) ## Allow specific block
```

Some other options we can use in “/etc/exports” file for file sharing is as follows.

<b>ro:</b> With the help of this option we can provide read only access to the shared files i.e client will only be able to read. <br>
<b>rw:</b> This option allows the client server to both read and write access within the shared directory. <br>
<b>sync:</b> Sync confirms requests to the shared directory only once the changes have been committed. <br>
<b>no_subtree_check:</b> This option prevents the subtree checking. When a shared directory is the subdirectory of a larger file system, nfs performs scans of every directory above it, in order to verify its permissions and details. Disabling the subtree check may increase the reliability of NFS, but reduce security. <br>
<b>no_root_squash:</b> This phrase allows root to connect to the designated directory.


We will have to change the firewall settings to allow nfs traffic in to the nfs-server - 
```
firewall-cmd --permanent --zone=public --add-service=nfs
firewall-cmd --permanent --zone=public --add-service=mountd
firewall-cmd --permanent --zone=public --add-service=rpc-bind
firewall-cmd --reload
```


## NFS Client Side Configurations: 

Installing nfs packages in the server side -
```
yum install nfs-utils
```

Create a mount point for the nfs share mount - 
```
mkdir /mnt/nfs
```

Mount the nfs-server using below commands - 
```
mount -t nfs 192.168.1.200:/nfs-share /mnt/nfs
```
In the above command ```-t``` is the type, which in our case is nfs and IP address in the server's IP. Please change the IP address as per your nfs-server IP.
