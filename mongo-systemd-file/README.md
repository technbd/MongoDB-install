


### Install the Compatibility Package: (For Rocky Linux)

Install the compatibility libraries for the required OpenSSL version:

```
openssl version
```


```
yum install compat-openssl10
```



#### Verify the Library Is Available:

```
ll /usr/lib64/libcrypto.so.10


lrwxrwxrwx 1 root root 19 Jun 28  2022 /usr/lib64/libcrypto.so.10 -> libcrypto.so.1.0.2o
```




### Creating mongodb service file: 

```
vim /etc/systemd/system/mongod.service


[Unit]
Description=MongoDB Database Server
After=network.target

[Service]
User=rcms
Group=rcms

ExecStart=/mongodb/6.0.17/mongodb-6.0.17/bin/mongod --config /mongodb/6.0.17/mongodb-6.0.17/mongod.conf

LimitNOFILE=64000

[Install]
WantedBy=multi-user.target
```


```
systemctl daemon-reload

systemctl start mongod
systemctl enable mongod

systemctl status mongod
```

