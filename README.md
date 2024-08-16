# Install Rabbit-msq Server to Centos 8

### Steps

##### 1. Add repository for Erlang version 26.2.5.2-1.el8
##### 2. Install Erlang version 26.2.5.2-1.el8
##### 3. Add repository for Rabbitmq server version 3.13.4
##### 4. Install Rabbit mq server
##### 5. Download and add plugin for Rabbitmq server
##### 6. Enable plugins
##### 7. Add user for rabbitmq and set permisions


### 1. Add repository for Erlang version 26.2.5.2-1.el8
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

### 2. Install Erlang version 26.2.5.2-1.el8
~~~
dnf install erlang
~~~

### 3. Add repository for Rabbitmq server version 3.13.4
~~~
sudo tee /etc/yum.repos.d/rabbitmq_rabbitmq-server.repo <<EOF
[rabbitmq_rabbitmq-server]
name=rabbitmq_rabbitmq-server
baseurl=https://packagecloud.io/rabbitmq/rabbitmq-server/el/8/\$basearch
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=file:///usr/share/keyrings/rabbitmq-archive-keyring.gpg
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300
EOF

~~~

### 4. Install Rabbit mq server
~~~
dnf install rabbitmq-server-3.13.4
~~~
#### Start rabbitqm-server
~~~
systemctl start rabbitmq-server
~~~
#### Add autostart rabbitmq-server
~~~
systemctl enable rabbitmq-server
~~~
#### Check status rabbitmq-server
~~~
systemctl status rabbitmq-server
~~~

### 5. Download and add plugin for Rabbitmq server
#### Check list and status plugins
~~~
rabbitmq-plugins list
~~~
#### Download and add plugin for Rabbitmq server
~~~
wget https://github.com/rabbitmq/rabbitmq-delayed-message-exchange/releases/download/v3.13.0/rabbitmq_delayed_message_exchange-3.13.0.ez
mv rabbitmq_delayed_message_exchange-3.13.0.ez /usr/lib/rabbitmq/lib/rabbitmq_server-3.13.4/plugins/
~~~

### 6. Enable plugins
~~~
rabbitmq-plugins enable rabbitmq_shovel
rabbitmq-plugins enable rabbitmq_shovel_management
rabbitmq-plugins enable rabbitmq_delayed_message_exchange
systemctl restart rabbitmq-server
systemctl status rabbitmq-server
~~~
#### Check status plugins
~~~
rabbitmq-plugins list
~~~

### 7. Add user for rabbitmq and set permisions
#### Add user and set password
~~~
rabbitmqctl add_user USER "PASSWORD"
~~~

#### Add permissions for user
~~~
rabbitmqctl set_permissions -p / USER ".*" ".*" ".*"
rabbitmqctl set_user_tags USER administrator
~~~
