# Postgresql databases

## Configuration needed before start clsutering the servers

******************************************************************
remote.sh
******************************************************************

provide ip address for servers,
SERVER >> remote
SERVERL >> local


create a trigger file "/var/lib/postgresql/n1" same as in recovery.conf
{ /var/lib/postgresql/9.5/main/recovery.conf }


"recovery.conf" has changed to "recovery.done" once the slave become writable.


create .pgpass and give 0660 permission under home of postgres user


remove "recovery.done" to "recovery.conf" for future failover


******************************************************************

remotefail.sh

******************************************************************

usermod -aG sudo postgres


visudo >>
postgres ALL=(ALL) NOPASSWD: ALL


"recovery.conf" has changed to "recovery.done" once the slave become writable.


create .pgpass and give 0660 permission under home of postgres user

