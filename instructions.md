# Check if Distribution

dist = uname -a | cut -d' ' -f 3

if "arch" in dist:

# Install easy-rsa package.
	sudo pacman -S easy-rsa --noconfirm

#if "debian" in dist:
	sudo apt-get install easy-rsa -y

cd /etc/easy-rsa

echo "set_var EASYRSA_ALGO ec" >> /etc/easy-rsa/vars
echo "set_var EASYRSA_CURVE secp521r1" >> /etc/easy-rsa/vars
echo "set_var EASYRSA_DIGEST "sha512" >> /etc/easy-rsa/vars
echo "set_var EASYRSA_NS_SUPPORT "yes" >> /etc/easy-rsa/vars


export EASYRSA=$(pwd)
export EASYRSA_VARS_FILE=/etc/easy-rsa/vars

## ca.crt
#### This should be created in the client side

easyrsa init-pki

pass = input()
easyrsa build-ca

pass >>
pass >>
'.' >> 

### We get /etc/easy-rsa/pki/ca.crt

## servername.key
#### This should be created in the client side

easyrsa gen-req servername nopass
pass >>
pass >>
'.' >>

### We get key: /etc/easy-rsa/pki/private/servername.key

## Diffie-Hellman file dh.pem
openssl dhparam -out /etc/openvpn/server/dh.pem 2048

### We got /etc/openvpn/server/dh.pem 

## ta.key
 openvpn --genkey secret /etc/openvpn/server/ta.key

### we got /etc/openvpn/server/ta.key

## servername.crt

easyrsa sign-req server servername
pass >>
pass >>

### We got /etc/easy-rsa/pki/issued/servername.crt

cp /etc/easy-rsa/pki/issued/servername.crt /etc/openvpn/server
cp /etc/easy-rsa/pki/ca.crt /etc/openvpn/server
cp /etc/easy-rsa/pki/private/servername.key /etc/openvpn/server


## References

### https://wiki.archlinux.org/index.php/OpenVPN
### https://wiki.archlinux.org/index.php/Easy-RSA
### https://en.wikipedia.org/wiki/Public_key_infrastructure
### https://github.com/angristan/openvpn-install/blob/master/openvpn-install.sh
### http://sites.inka.de/bigred/devel/tcp-tcp.html


