# FreeRADIUS

Primero deben vaciar los archivos:
```
/etc/init.d/freeradius stop
echo > /etc/freeradius/clients.conf
echo > /etc/freeradius/users
```
## /etc/freeradius/clients.conf
```
client localhost {
	ipaddr = 127.0.0.1
	secret		= testing123
	require_message_authenticator = no
	nastype     = other
}
client 192.168.0.0/16 {
	secret		= lacnic29
	shortname	= test-ipv6
}
```
## /etc/freeradius/users
```
lacnic29	Cleartext-Password := "panama"
		      Service-Type = Framed-User,
		      Framed-Protocol = PPP,
		      Framed-Compression = Van-Jacobsen-TCP-IP,

lacnic29-48fijo Cleartext-Password := "panama"
          Service-Type = Framed-User,
          Framed-Protocol = PPP,
          Framed-Compression = Van-Jacobsen-TCP-IP,
          Delegated-IPv6-Prefix = 2001:db8:7000::/48”,

DEFAULT	Framed-Protocol == PPP
	        Framed-Protocol = PPP,
	        Framed-Compression = Van-Jacobson-TCP-IP
```

Luego vuelven a arrancar el servicio

```
/etc/init.d/freeradius start
```

O ejecútalo en modo DEBUG:

```
freeradius -X
```
