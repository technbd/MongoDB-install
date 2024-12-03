
## Install MongoDB Community Edition on Centos 7: 

This command installs `mongodb-org`, a meta-package that includes the following:

- mongodb-org-server : The standard MongoDB daemon, and relevant init scripts and configurations
- mongodb-org-mongos : The MongoDB Shard daemon
- mongodb-org-shell : The MongoDB shell, used to interact with MongoDB via the command line
- mongodb-org-tools : Contains a few basic tools to restore, import, and export data, as well as other diverse functions.


### Install MongoDB 6:
Create a `/etc/yum.repos.d/mongodb-org-6.0.repo` file so that you can install MongoDB directly using `yum`:

```
vim /etc/yum.repos.d/mongodb-org-6.0.repo


[mongodb-org-6.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/7/mongodb-org/6.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://pgp.mongodb.com/server-6.0.asc
```


```
yum install -y mongodb-org
```


```
mongod --version

db version v6.0.17
Build Info: {
    "version": "6.0.17",
    "gitVersion": "1b0ca02043c6d35d5cfdc91e21fc00a05d901539",
    "openSSLVersion": "OpenSSL 1.0.1e-fips 11 Feb 2013",
    "modules": [],
    "allocator": "tcmalloc",
    "environment": {
        "distmod": "rhel70",
        "distarch": "x86_64",
        "target_arch": "x86_64"
    }
}
```



### Install MongoDB 7:

Create a `/etc/yum.repos.d/mongodb-org-7.0.repo` file so that you can install MongoDB directly using `yum`:

```
vim /etc/yum.repos.d/mongodb-org-7.0.repo


[mongodb-org-7.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/7/mongodb-org/7.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://pgp.mongodb.com/server-7.0.asc
```


```
yum install -y mongodb-org
```


```
mongod --version
```


```
systemctl start mongod
systemctl status mongod
```


```
ll /var/log/mongodb
ll /var/lib/mongo
```


###

By default, MongoDB is configured to connect without any authentication. For security purposes, it is a good idea to secure MongoDB with a password. The authorization option enables **role-based access control** for your databases. If no value is specified, any user will have the ability to modify any database. RBAC which limits users’ access to database resources and actions, is enabled via the authorization option. Each user will have access to every database and be able to perform any operation if this option is disabled. 

We strongly recommend uncommenting the `security` section and adding the following:

```
vim /etc/mongod.conf

# network interfaces
net:
  port: 27017
  #bindIp: 127.0.0.1  # Enter 0.0.0.0,:: to bind to all IPv4 and IPv6 addresses or, alternatively, use the net.bindIpAll setting.
  bindIp: 0.0.0.0


security:
  authorization: enabled
```


```
systemctl restart mongod
```




### 

Start a `mongosh` session on the same host machine as the mongod. You can run `mongosh` without any command-line options to connect to a mongod that is running on your localhost with default port 27017.

```
mongosh --version

2.3.1
```


```
mongosh

Current Mongosh Log ID: 66dea85b1b0326b459964032
Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.3.1
Using MongoDB:          6.0.17
Using Mongosh:          2.3.1
...
...
```


```
test> db.version()
6.0.17
```



---
---



## Install MongoDB on Rocky 8: 



### Install MongoDB 6:
Create a `/etc/yum.repos.d/mongodb-org-6.0.repo` file so that you can install MongoDB directly using `yum`:

```
vim /etc/yum.repos.d/mongodb-org-6.0.repo

[mongodb-org-6.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/8/mongodb-org/6.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://pgp.mongodb.com/server-6.0.asc
```


```
yum install -y mongodb-org -y 
```


```
systemctl start mongod
systemctl status mongod
```


```
mongod --version

db version v6.0.17
Build Info: {
    "version": "6.0.17",
    "gitVersion": "1b0ca02043c6d35d5cfdc91e21fc00a05d901539",
    "openSSLVersion": "OpenSSL 1.1.1k  FIPS 25 Mar 2021",
    "modules": [],
    "allocator": "tcmalloc",
    "environment": {
        "distmod": "rhel80",
        "distarch": "x86_64",
        "target_arch": "x86_64"
    }
}
```


```
mongosh

Current Mongosh Log ID: 66deaeb31ce8502073964032
Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.3.1
Using MongoDB:          6.0.17
Using Mongosh:          2.3.1
```




### Install MongoDB 7:

Create a `/etc/yum.repos.d/mongodb-org-7.0.repo` file so that you can install MongoDB directly using `yum`:

```
vim /etc/yum.repos.d/mongodb-org-7.0.repo


[mongodb-org-7.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/8/mongodb-org/7.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://pgp.mongodb.com/server-7.0.asc
```


```
yum install -y mongodb-org
```


```
mongod --version

db version v7.0.14
Build Info: {
    "version": "7.0.14",
    "gitVersion": "ce59cfc6a3c5e5c067dca0d30697edd68d4f5188",
    "openSSLVersion": "OpenSSL 1.1.1k  FIPS 25 Mar 2021",
    "modules": [],
    "allocator": "tcmalloc",
    "environment": {
        "distmod": "rhel80",
        "distarch": "x86_64",
        "target_arch": "x86_64"
    }
}
```

### Install MongoDB v4.4:

Create a `mongodb.repo` file so that you can install MongoDB directly using `yum`:

```
vim /etc/yum.repos.d/mongodb.repo

[mongodb-org-4.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/7/mongodb-org/4.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.4.asc
```


```
yum install -y mongodb-org
```


```
systemctl start mongod
```




### Links:
- [Install MongoDB Community Edition](https://www.mongodb.com/docs/v6.0/tutorial/install-mongodb-on-red-hat/)
- [Install MongoDB Community using .tgz Tarball](https://www.mongodb.com/docs/v6.0/tutorial/install-mongodb-on-red-hat-tarball/)
- [Installing MongoDB on CentOS 7](https://www.linode.com/docs/guides/install-mongodb-on-centos-7/)
- [Install MongoDB](https://operavps.com/docs/install-mongodb-on-centos/)
- [Install MongoDB on Rocky](https://www.tecmint.com/install-mongodb-on-rocky-linux-and-almalinux/)
- [Install MongoDB on Rocky](https://www.atlantic.net/dedicated-server-hosting/how-to-install-and-use-mongodb-on-rocky-linux-8/)





