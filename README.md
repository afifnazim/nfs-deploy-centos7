# nfs-deploy-centos7
Documentation of how to deploy NFS servers in CentOS 7 machines

To deploy NFS services we will need 2 server. One will be our nfs-server and other will be the nfs-client. 
NFS server will work as the file hosting platform and client will be able to read, write and create new files in server side. 

## NFS Server Side Configurations: 

```
yum install nfs-utils nfs-utils-lib
```
