
In this file I'll walk you thought the freeradius server configuration
and also the client which in this case is a cisco router

First we need to install de freeradius, in our case since we are
using ubuntu we'll use the command:

apt install freeradius 

Once we have the freeradius install we need to configure two files,
the clients.conf in which we will specify which IP adresess are allow
to connect with our server and the users file in which we will specify
the users that are allow to access the devices configured with the radius server.


In our case we will use vim to access the file
and allow the 192.168.122.0/24 network
to access our server.

vim /etc/freeradius/3.0/clients.conf 


client R1 {
 ipaddr = 192.168.122.0
 netmask = 24
 secret = erickr@dius
 nastype = cisco
}

In the users file we will configure the users we need, in my case I setup the eviloria which uses
cleartext password (not recommended) and the user jperez which uses an encrypted MD5 password.

vim /etc/freeradius/3.0/users

eviloria Cleartext-Password := "cisco"

jperez Crypt-Password:= "$1$QJvhWm73$i8ARt7x.9QudhkrUlOZNt0"


The jperez password is configured in MD5, we can use the command radcrypt for that,
in fact we are using the password cisco but encrypted in MD5

root@UbuntuDockerGuest-1:/etc/freeradius/3.0# radcrypt -m cisco
$1$QJvhWm73$i8ARt7x.9QudhkrUlOZNt0

After we finish our configuration in the server then we have to go the de router and set up de configuration,
so that the router can connect with the radius server, the complete configuration is in file R1, but here 
goes the basic configuration

aaa new-model
aaa authentication login RADIUSERICK group radius local
aaa authorization exec ERICK_RADIUS group radius local

radius-server host 192.168.122.118 auth-port 1812 acct-port 1813 key erickr@dius

line vty 0 4
 authorization exec ERICK_RADIUS
 login authentication RADIUSERICK

Then we just have to test our connection using telnet or ssh.

NOTE: in order to the changes to take effect we have to restart the freeradius service, we can use
the next command for that:

service freeradius restart
 