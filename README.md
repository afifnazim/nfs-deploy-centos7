# nfs-deploy-centos7
Documentation of how to deploy NFS servers in CentOS 7 machines

To deploy NFS services we will need 2 server. One will be our nfs-server and other will be the nfs-client. 
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
/nfs-share 192.168.1.250(rw,sync,no_root_squash) ###Change the IP address as per your nfs-server IP address###
```

