# Configuración Básica BNG IPv6 Mikrotik
```
/ip pool
add name=PPPoEv4 ranges=100.64.8.0/22
/ipv6 pool
add name=PPPoEv6-PD prefix=2001:db8:8000::/33 prefix-length=48
/ppp profile
set *0 remote-address=PPPoEv4
add change-tcp-mss=yes dhcpv6-pd-pool=PPPoEv6-PD dns-server=8.8.8.8,9.9.9.9 local-address=100.64.0.1 \
    name=PPPoEv6 only-one=no remote-address=PPPoEv4 use-compression=no use-encryption=no use-mpls=no \
    use-upnp=no wins-server=127.0.0.1
/interface pppoe-server server
add authentication=pap default-profile=PPPoEv6 disabled=no interface=ether2 max-sessions=1024
/ip address
add address=192.168.33.10/24 interface=ether1 network=192.168.33.0 comment="WAN"
/ip firewall nat
add action=masquerade chain=srcnat src-address=100.64.0.0/10
/ip route
add check-gateway=arp distance=1 gateway=192.168.33.2 comment="default gateway"
/ipv6 address
add address=2001:db8::beba interface=ether1
/ppp aaa
set interim-update=5m use-radius=yes
/radius
add address=192.168.33.100 secret=lacnic29 service=ppp
/routing ospf-v3 interface
add area=backbone
/system identity
set name=BNG-MKT
/user aaa
set use-radius=yes
```
