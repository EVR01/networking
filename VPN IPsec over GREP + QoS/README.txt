IPSEC/GRE VPN + QoS

In this lab I configured a VPN site-site over GRE, we use GRE
for this lab because the the multicast traffic that we have
is not compatible with ipsec since ipsec just processes unicast 
but not multicast ip packets, so I created a GRE interface tunnel
in order to transport the multicast traffic over it. I also applied the
crypto map to the tunnel interface in order to encrypt this traffic.


In the second part of this lab I match the traffic from the 192.168.10.0/24 
network in orther to give priority to this traffic, this network is simulatin
the network that is supose to handle the multicast traffic, since we want
to give priority to this traffic whenever there is cogestion we applied the
Qos policy to the class-map called MULTICAST.
