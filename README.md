# MikroTik-OVPN

/certificate
add name=ca-template common-name=netnam.vn days-valid=3650 key-size=2048 key-usage=crl-sign,key-cert-sign
add name=server-template common-name=*.netnam.vn days-valid=3650 key-size=2048 key-usage=digital-signature,key-encipherment,tls-server
add name=client-template common-name=client.netnam.vn days-valid=3650 key-size=2048 key-usage=tls-client

/certificate
sign ca-template name=ca-certificate
sign server-template name=server-certificate ca=ca-certificate
sign client-template name=client-certificate ca=ca-certificate


/certificate
export-certificate ca-certificate export-passphrase=""
export-certificate client-certificate export-passphrase=12345678

/ip
pool add name="vpn-pool" ranges=192.168.88.0-192.168.88.250

/ppp
profile add name="vpn-profile" use-encryption=yes local-address=192.168.88.254 dns-server=192.168.88.254 remote-address=vpn-pool
secret add name=vpn profile=vpn-profile password=password

/interface ovpn-server server
set default-profile=vpn-profile certificate=server-certificate require-client-certificate=yes auth=sha1 cipher=aes128,aes192,aes256 enabled=yes

https://www.medo64.com/2016/12/simple-openvpn-server-on-mikrotik/

##Config files

client

dev tun

proto tcp

remote 10.20.32.102 1194

resolv-retry infinite

nobind

persist-key

persist-tun

ca ca.crt

cert client.crt

key client.key

remote-cert-tls server

cipher AES-128-CBC

auth SHA1

auth-user-pass

redirect-gateway def1

verb 3
