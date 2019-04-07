# KafkaTest

kafkaのテスト実行環境
```
■構成
Ansibleサーバ1台；AWS t2.micro
Kafka Broker3台：AWS t2.small

■使い方
<Ansibleサーバ>
１。Ansibleインストール (root)
# amazon-linux-extras install ansible2

２。gitインストール (root)
# yum install git

３。hosts修正 (root)
# vi /etc/hosts
　→ Kafka Broker3台分追加
# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost6 localhost6.localdomain6

xxx.xxx.xxx.xxx	kafka-broker01
xxx.xxx.xxx.xxx	kafka-broker02
xxx.xxx.xxx.xxx	kafka-broker03

４。ssh鍵作成 (ec2-user)
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ec2-user/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ec2-user/.ssh/id_rsa.
Your public key has been saved in /home/ec2-user/.ssh/id_rsa.pub.

<Kafka Broker3台>
５。ssh公開鍵配布 (ec2-user)
　→ id_rsa.pubをKafka Broker3台のec2-userに配布。
$ cat id_rsa.pub >> ~/.ssh/authorized_keys

<Ansibleサーバ>
６。Code取得 (ec2-user)
$ git clone https://github.com/RyoheiTojo/KafkaTest.git

７。OracleJDK配置 (ec2-user)
　jdk-8u201-linux-x64.rpmを取得
　https://www.oracle.com/technetwork/java/javase/downloads/index.html

　/home/ec2-user/KafkaTest/roles/jdk/filesに配置

８。Ansible実行 (ec2-user)
$ cd /home/ec2-user/KafkaTest
$ ansible-playbook -i inventory/production site.yml
```

kafkaのテスト実行アプリ開発環境
```
■構成
Ansibleサーバ1台；AWS t2.micro
開発用サーバ1台：AWS t2.micro

■使い方
<Ansibleサーバ>
１。Ansibleインストール (root)
# amazon-linux-extras install ansible2

２。gitインストール (root)
# yum install git

３。hosts修正 (root)
# vi /etc/hosts
　→ Kafka Broker3台分追加
# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost6 localhost6.localdomain6

xxx.xxx.xxx.xxx	developserv

４。ssh鍵作成 (ec2-user)
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ec2-user/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ec2-user/.ssh/id_rsa.
Your public key has been saved in /home/ec2-user/.ssh/id_rsa.pub.

<開発用サーバ1台>
５。ssh公開鍵配布 (ec2-user)
　→ id_rsa.pubを開発用サーバ1台のec2-userに配布。
$ cat id_rsa.pub >> ~/.ssh/authorized_keys

<Ansibleサーバ>
６。Code取得 (ec2-user)
$ git clone https://github.com/RyoheiTojo/KafkaTest.git

７。OracleJDK配置 (ec2-user)
　jdk-8u201-linux-x64.rpmを取得
　https://www.oracle.com/technetwork/java/javase/downloads/index.html

　/home/ec2-user/KafkaTest/roles/jdk/filesに配置

８。Mavenファイル配置 (ec2-user)
　apache-maven-3.6.0-bin.tar.gzを取得
  http://ftp.meisei-u.ac.jp/mirror/apache/dist/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz

　/home/ec2-user/KafkaTest/roles/maven/filesに配置

９。Ansible実行 (ec2-user)
$ cd /home/ec2-user/KafkaTest
$ ansible-playbook -i inventory/develop site.yml

```
