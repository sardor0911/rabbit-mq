# Install Rabbit-msq Server to Centos 8

## Steps

###1. Add repository for Erlang version 26.2.5.2-1.el8
###2. Install Erlang version 26.2.5.2-1.el8
###3. Add repository for Rabbitmq server version 3.13.4
###4. Install Rabbit mq server
###5. Download and add plugin for Rabbitmq server
###6. Enable plugins
###7. Add user for rabbitmq and set permisions


## To install we need Erlang version 26.2.5.2-1.el8
### Install Erlang version 26.2.5.2-1.el8
### Add repository for Erlang
~~~
sudo tee /etc/yum.repos.d/rabbitmq_erlang.repo <<EOF
[rabbitmq_erlang]
name=rabbitmq_erlang
baseurl=https://packagecloud.io/rabbitmq/erlang/el/8/\$basearch
repo_gpgcheck=0
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/rabbitmq/erlang/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300

[rabbitmq_erlang-source]
name=rabbitmq_erlang-source
baseurl=https://packagecloud.io/rabbitmq/erlang/el/8/SRPMS
repo_gpgcheck=0
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/rabbitmq/erlang/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300
EOF

~~~
