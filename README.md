![image](https://github.com/user-attachments/assets/4ded567f-4c1a-4560-8e06-cd7b25b94ae6)# Install Rabbit-msq Server to Centos 8

### Steps

##### 1. Add repository for Erlang version 26.2.5.2-1.el8
##### 2. Download GPG-ключ 
##### 3. Install Erlang version 26.2.5.* 
##### 4. Add repository for Rabbitmq server version 3.13.7
##### 5. Install Rabbit mq server
##### 6. Download and add plugin for Rabbitmq server
##### 7. Enable plugins
##### 8. Add user for rabbitmq and set permisions


### 1. Add repository for Erlang version 26.2.5.*
~~~
sudo tee /etc/yum.repos.d/rabbitmq_erlang.repo <<EOF
[rabbitmq_erlang]
name=rabbitmq_erlang
baseurl=https://packagecloud.io/rabbitmq/erlang/el/8/$basearch
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
### 2. Download GPG-ключ 
~~~
curl -fsSL https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.rpm.sh | sudo bash
~~~
### 3. Install Erlang version 26.2.5.*
~~~
dnf install erlang
~~~

### 4. Add repository for Rabbitmq server version 3.13.7
~~~
[rabbitmq_rabbitmq-server]
name=rabbitmq_rabbitmq-server
baseurl=https://packagecloud.io/rabbitmq/rabbitmq-server/el/8/$basearch
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/rabbitmq/rabbitmq-server/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300

[rabbitmq_rabbitmq-server-source]
name=rabbitmq_rabbitmq-server-source
baseurl=https://packagecloud.io/rabbitmq/rabbitmq-server/el/8/SRPMS
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/rabbitmq/rabbitmq-server/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300
EOF
~~~

### 5. Install Rabbit mq server
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

### 6. Download and add plugin for Rabbitmq server
#### Check list and status plugins
~~~
rabbitmq-plugins list
~~~
#### Download and add plugin for Rabbitmq server
~~~
wget https://github.com/rabbitmq/rabbitmq-delayed-message-exchange/releases/download/v3.13.0/rabbitmq_delayed_message_exchange-3.13.0.ez
~~~
#### Move plugins to folder Rabbitmq-server
~~~
mv rabbitmq_delayed_message_exchange-3.13.0.ez /usr/lib/rabbitmq/lib/rabbitmq_server-3.13.7/plugins/
~~~

### 7. Enable plugins
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
#### Result
![image](https://github.com/user-attachments/assets/10c07c36-0aa2-49c9-a43d-3f856a06c286)

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
