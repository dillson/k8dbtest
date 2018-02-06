# k8dbtest
Testing for Kubernetes DB application

Deploy MySQL Test setup


Test MySQL setup
You can use mysql-client to send some data to the master (mysql-0.mysql)

kubectl run mysql-client --image=mysql:5.7 -i --rm --restart=Never --\
  mysql -h mysql-0.mysql <<EOF
CREATE DATABASE test;
CREATE TABLE test.messages (message VARCHAR(250));
INSERT INTO test.messages VALUES ('hello, from mysql-client');
EOF
You can run the following to test if slaves (mysql-read) received the data

$ kubectl run mysql-client --image=mysql:5.7 -it --rm --restart=Never --\
  mysql -h mysql-read -e "SELECT * FROM test.messages"
This should display an output like this:

+--------------------------+
| message                  |
+--------------------------+
| hello, from mysql-client |
+--------------------------+
