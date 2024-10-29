# Jarkom-Modul-3-IT35-2024

| Nama          | NRP          |
| ------------- | ------------ |
| RM Novian Malcolm Bayuputra | 5027231035 |
| Nayyara Ashila | 5027231083 |


# Topologi

![Screenshot 2024-10-22 212315](https://github.com/user-attachments/assets/32a46951-f329-4af1-8066-edfe7f0c8a68)

# Setup Config

## Paradis (Router/DHCP Relay)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.234.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.234.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.234.3.1
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.234.4.1
	netmask 255.255.255.0
```

## Fritz (DNS Server)
```
auto eth0
iface eth0 inet static
	address 192.234.4.3
	netmask 255.255.255.0
	gateway 192.234.4.1
```

## Tybur (DHCP Server)
```
auto eth0
iface eth0 inet static
	address 192.234.4.2
	netmask 255.255.255.0
	gateway 192.234.4.1
```

## Warhammer (Database Server)
```
auto eth0
iface eth0 inet static
	address 192.234.3.2
	netmask 255.255.255.0
	gateway 192.234.3.1
```

## Beast (Load Balancer Laravel)
```
auto eth0
iface eth0 inet static
	address 192.234.3.3
	netmask 255.255.255.0
	gateway 192.234.3.1
```

## Colossal (Load Balancer PHP)
```
auto eth0
iface eth0 inet static
	address 192.234.3.4
	netmask 255.255.255.0
	gateway 192.234.3.1
```

## Annie (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 192.234.1.2
	netmask 255.255.255.0
	gateway 192.234.1.1
```

## Bertholdt (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 192.234.1.3
	netmask 255.255.255.0
	gateway 192.234.1.1
```

## Reiner (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 192.234.1.4
	netmask 255.255.255.0
	gateway 192.234.1.1
```

## Armin (PHP Worker)
``` 
auto eth0
iface eth0 inet static
	address 192.234.2.2
	netmask 255.255.255.0
	gateway 192.234.2.1
up echo 'nameserver 192.234.4.3' > /etc/resolv.conf  
```

## Eren (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 192.234.2.3
	netmask 255.255.255.0
	gateway 192.234.2.1
up echo 'nameserver 192.234.4.3' > /etc/resolv.conf   

```

## Mikasa (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 192.234.2.4
	netmask 255.255.255.0
	gateway 192.234.2.1
up echo 'nameserver 192.234.4.3' > /etc/resolv.conf 
```

## Erwin (Client)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 82:72:f8:e4:51:d0
```

## Zeke (Client)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 3a:ef:29:94:f1:8a
```

## Paradis (DHCP Relay)
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.234.0.0/16
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start
```

## Fritz (DNS Server)
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
```

## Tybur (DHCP Server)
```
echo 'nameserver 192.234.4.3' > /etc/resolv.conf   
apt-get update
apt install isc-dhcp-server -y
```

## Warhammer (Database Server)
```
echo 'nameserver 192.234.4.3' > /etc/resolv.conf  
apt-get update
apt-get install mariadb-server -y
service mysql start
```

## PHP Worker
```
apt-get update
apt-get install nginx -y
apt-get install wget unzip -y
apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-cli php7.3-zip -y
```

## Laravel Worker
```
echo 'nameserver 192.234.4.3' > /etc/resolv.conf   
apt-get update
apt-get install mariadb-client -y
```

## Colossal (Load Balancer PHP)
```
echo 'nameserver 192.234.4.3' > /etc/resolv.conf   
apt-get update
apt-get install bind9 -y
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y
service nginx start
```

## Client
```
apt-get update
apt install lynx -y
apt install htop -y
apt install apache2-utils -y
apt-get install jq -y
```

# Soal 0
Pulau Paradis telah menjadi tempat yang damai selama 1000 tahun, namun kedamaian tersebut tidak bertahan selamanya. Perang antara kaum Marley dan Eldia telah mencapai puncak. Kaum Marley yang dipimpin oleh Zeke, me-register domain name marley.yyy.com untuk worker Laravel mengarah pada Annie. Namun ternyata tidak hanya kaum Marley saja yang berinisiasi, kaum Eldia ternyata sudah mendaftarkan domain name eldia.yyy.com untuk worker PHP (0) mengarah pada Armin.

```
echo 'zone "marley.it35.com" {
    type master;
    file "/etc/bind/sites/marley.it35.com";
};
zone "eldia.it35.com" {
    type master;
    file "/etc/bind/sites/eldia.it35.com";
};' > /etc/bind/named.conf.local

mkdir -p /etc/bind/sites
cp /etc/bind/db.local /etc/bind/sites/marley.it35.com
cp /etc/bind/db.local /etc/bind/sites/eldia.it35.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     marley.it35.com. root.marley.it35.com. (
                        2024102301      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      marley.it35.com.
@       IN      A       192.234.1.2    ; IP Annie
www     IN      CNAME   marley.it35.com.' > /etc/bind/sites/marley.it35.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eldia.it35.com. root.eldia.it35.com. (
                            2024102301         ; Serial
                            604800              ; Refresh
                            86400              ; Retry
                            2419200              ; Expire
                            604800 )            ; Negative Cache TTL
;
@       IN      NS      eldia.it35.com.
@       IN      A       192.234.2.2    ; IP Armin
www     IN      CNAME   eldia.it35.com.' > /etc/bind/sites/eldia.it35.com

echo 'options {
    directory "/var/cache/bind";

    forwarders {
        192.168.122.1;
    };

    // dnssec-validation auto;

    allow-query { any; };
    auth-nxdomain no;    # conform to RFC1035
    listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

* Test Ping `eldia.it35.com`
  
![Screenshot 2024-10-29 075115](https://github.com/user-attachments/assets/420554aa-98e8-4b82-b2ec-a47d71a009af)


# Soal 1-5

>Nomor 2
* Konfigurasi pada Tybur (DHCP Server)
```
echo '
subnet 192.234.1.0 netmask 255.255.255.0 {
range 192.234.1.5 192.234.1.25;
range 192.234.1.50 192.234.1.100;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

>Nomor 3
* Konfigurasi pada Tybur (DHCP Server)
```
echo '
subnet 192.234.1.0 netmask 255.255.255.0 {
	range 192.234.1.5 192.234.1.25;
	range 192.234.1.50 192.234.1.100;
}

subnet 192.234.2.0 netmask 255.255.255.0 {
	range 192.234.2.09 192.234.2.27;
	range 192.234.2.81 192.234.2.243;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

>Nomor 4
* Konfigurasi pada Paradis (DHCP Relay)
  
```
echo '
SERVERS="192.234.4.2"
INTERFACES="eth1 eth2 eth3 eth4"
OPTIONS=""
' > /etc/default/isc-dhcp-relay

echo '
net.ipv4.ip_forward=1
' > /etc/sysctl.conf

service isc-dhcp-relay restart
```

* Konfigurasi pada Tybur (DHCP Server)
```
echo '
INTERFACESv4="eth0"
INTERFACESv6=""
' > /etc/default/isc-dhcp-server

echo '
subnet 192.234.1.0 netmask 255.255.255.0 {
	range 192.234.1.05 192.234.1.25;
	range 192.234.1.50 192.234.1.100;
	option routers 192.234.1.1;
	option broadcast-address 192.234.1.255;
	option domain-name-servers 192.234.4.3;
}

subnet 192.234.2.0 netmask 255.255.255.0 {
	range 192.234.2.09 192.234.2.27;
	range 192.234.2.81 192.234.2.243;
	option routers 192.234.2.1;
	option broadcast-address 192.234.1.255;
	option domain-name-servers 192.234.4.3;
}

subnet 192.234.3.0 netmask 255.255.255.0 {
	option routers 192.234.3.1;
}

subnet 192.234.4.0 netmask 255.255.255.0 {
	option routers 192.234.4.1;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

>Nomor 5
* Konfigurasi pada Tybur (DHCP Server)
```
echo '
INTERFACESv4="eth0"
INTERFACESv6=""
' > /etc/default/isc-dhcp-server

echo '
subnet 192.234.1.0 netmask 255.255.255.0 {
	range 192.234.1.5 192.234.1.25;
	range 192.234.1.50 192.234.1.100;
	option routers 192.234.1.1;
	option broadcast-address 192.234.1.255;
	option domain-name-servers 192.234.4.3;
	default-lease-time 360;
	max-lease-time 5220;
}

subnet 192.234.2.0 netmask 255.255.255.0 {
	range 192.234.2.9 192.234.2.27;
	range 192.234.2.81 192.234.2.243;
	option routers 192.234.2.1;
	option broadcast-address 192.234.1.255;
	option domain-name-servers 192.234.4.3;
	default-lease-time 1800;
	max-lease-time 5220;
}

subnet 192.234.3.0 netmask 255.255.255.0 {
	option routers 192.234.3.1;
}

subnet 192.234.4.0 netmask 255.255.255.0 {
	option routers 192.234.4.1;
}
' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

* Test Ping `eldia.it35.com`

![Screenshot 2024-10-27 013651](https://github.com/user-attachments/assets/75d31176-bfb5-46eb-8609-afe8515869eb)

# Soal 6
Armin berinisiasi untuk memerintahkan setiap worker PHP untuk melakukan konfigurasi virtual host untuk website berikut https://intip.in/BangsaEldia dengan menggunakan php 7.3 

* Konfigurasi pada PHP Worker
```
service nginx start
service php7.3-fpm start

mkdir -p /var/www/eldia.it35.com

wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1TvebIeMQjRjFURKVtA32lO9aL7U2msd6' -O /root/bangsaEldia.zip
unzip -o /root/bangsaEldia.zip -d /var/www/eldia.it35.com

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/eldia.it35.com
ln -s /etc/nginx/sites-available/eldia.it35.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo '
server {
  listen 80;
  listen [::]:80;

  root /var/www/eldia.it35.com;
  index index.php index.html index.htm;

  server_name eldia.it35.com;

  location / {
    try_files $uri $uri/ =404;
  }

  location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
  }

  location ~ /\.ht {
    deny all;
  }
}' > /etc/nginx/sites-available/eldia.it35.com

service nginx restart
```

* Test di Armin

![Screenshot 2024-10-27 024526](https://github.com/user-attachments/assets/d770bd2c-258c-41f2-a8a4-e7d6dbcbfbfe)

* Test di Eren

![Screenshot 2024-10-27 024054](https://github.com/user-attachments/assets/228678ef-47e9-46a1-9e4b-5e8baf0ca653)

* Test di Mikasa

![Screenshot 2024-10-27 023913](https://github.com/user-attachments/assets/15a2472b-d167-422b-b244-e1265d0b09ca)

# Soal 7

* Konfigurasi pada Colossal (Load Balancer PHP)
```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #    hash $request_uri consistent;
        #    least_conn;
        #    ip_hash;
    server 192.234.2.2;
    server 192.234.2.3;
    server 192.234.2.4;
}

server {
    listen 80;
    server_name eldia.it35.com www.eldia.it35.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        proxy_pass http://worker;
    }
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart

**Konfigurasi pada Fritz (DNS Server)**

echo 'zone "marley.it35.com" {
    type master;
    file "/etc/bind/sites/marley.it35.com";
};
zone "eldia.it35.com" {
    type master;
    file "/etc/bind/sites/eldia.it35.com";
};' > /etc/bind/named.conf.local

mkdir -p /etc/bind/sites
cp /etc/bind/db.local /etc/bind/sites/marley.it35.com
cp /etc/bind/db.local /etc/bind/sites/eldia.it35.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     marley.it35.com. root.marley.it35.com. (
                        2024102301      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      marley.it35.com.
@       IN      A       192.234.1.2    ; IP Annie
www     IN      CNAME   marley.it35.com.' > /etc/bind/sites/marley.it35.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eldia.it35.com. root.eldia.it35.com. (
                            2024102301         ; Serial
                            604800              ; Refresh
                            86400              ; Retry
                            2419200              ; Expire
                            604800 )            ; Negative Cache TTL
;
@       IN      NS      eldia.it35.com.
@       IN      A       192.234.3.4    ; IP Colossal
www     IN      CNAME   eldia.it35.com.' > /etc/bind/sites/eldia.it35.com

echo 'options {
    directory "/var/cache/bind";

    forwarders {
        192.168.122.1;
    };

    // dnssec-validation auto;

    allow-query { any; };
    auth-nxdomain no;    # conform to RFC1035
    listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

* Test di client

`ab -n 6000 -c 200 http://eldia.it35.com/`

![Screenshot 2024-10-27 110724](https://github.com/user-attachments/assets/6c8ff12d-8e32-43fc-9288-e96762337c14)


# Soal 8
Karena Erwin meminta “laporan kerja Armin”, maka dari itu buatlah analisis hasil testing dengan 1000 request dan 75 request/second untuk masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
a. Nama Algoritma Load Balancer
b. Report hasil testing pada Apache Benchmark
c. Grafik request per second untuk masing masing algoritma. 
d. Analisis (8)

```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #    hash $request_uri consistent;
        #    least_conn;
        #    ip_hash;
    server 192.234.2.2;
    server 192.234.2.3;
    server 192.234.2.4;
}

server {
    listen 80;
    server_name eldia.it35.com www.eldia.it35.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        proxy_pass http://worker;
    }
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

```
  	# hash $request_uri consistent;
        #    least_conn;
        #    ip_hash;
```

Ada 4 algoritma, round robin, hash, least connection, ip hash. Uncomment salah satu atau tidak sama sekali

- Round Robin
  
![Screenshot 2024-10-27 110814](https://github.com/user-attachments/assets/48938921-166e-4bae-9663-a61ec946b988)

- Hash

![Screenshot 2024-10-27 110855](https://github.com/user-attachments/assets/fee7c106-44f4-47c7-94f5-ae8082533b54)

- Least Connection

![Screenshot 2024-10-27 110942](https://github.com/user-attachments/assets/88dc13d2-c4f9-4e5f-aabc-a67009435f01)

- IP Hash

![Screenshot 2024-10-27 111027](https://github.com/user-attachments/assets/511cb9bc-acc8-484b-8b47-8bd8a6d8ff27)

### Grafik dan Analisis

![image](https://github.com/user-attachments/assets/2614d84c-af35-4246-9d96-1610f849b7bf)

Analisis: 
> Generic Hash memiliki performa terbaik dengan RPS tertinggi, diikuti oleh Weight Round-Robin. Round Robin berada di tengah, sementara IP Hash memiliki RPS terendah. Generic Hash paling optimal untuk beban tinggi, sedangkan IP Hash cocok untuk kestabilan sesi.

  
# Soal 9
Dengan menggunakan algoritma Least-Connection, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 1000 request dengan 10 request/second, kemudian tambahkan grafiknya pada “laporan kerja Armin”. (9)

```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #    hash $request_uri consistent;
            least_conn;
        #    ip_hash;
    server 192.234.2.2;
    server 192.234.2.3;
    server 192.234.2.4;
}

server {
    listen 80;
    server_name eldia.it35.com www.eldia.it35.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        proxy_pass http://worker;
    }
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

- 3 Worker

![Screenshot 2024-10-27 111337](https://github.com/user-attachments/assets/73ffabf7-7c7e-4309-8e77-fddcd3ebe052)

- 2 Worker

![Screenshot 2024-10-27 111456](https://github.com/user-attachments/assets/ec233dcb-c6c0-4ec2-a3be-0699ca7de729)

- 1 Worker

![Screenshot 2024-10-27 111537](https://github.com/user-attachments/assets/7b5ce8cd-a832-477e-b5da-bf4ac67ce8eb)

# Soal 10
Selanjutnya coba tambahkan keamanan dengan konfigurasi autentikasi di Colossal dengan dengan kombinasi username: “arminannie” dan password: “jrkmyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/supersecret/ (10)

* Konfigurasi pada Colossal (Load Balancer PHP)
```
mkdir /etc/nginx/supersecret
htpasswd -b -c /etc/nginx/supersecret/htpasswd arminannie jrkmit35

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #    hash $request_uri consistent;
        #    least_conn;
        #    ip_hash;
    server 192.234.2.2;
    server 192.234.2.3;
    server 192.234.2.4;
}

server {
    listen 80;
    server_name eldia.it35.com www.eldia.it35.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        proxy_pass http://worker;
    }

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/supersecret/htpasswd;
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

* Contoh jika username/password salah

![Screenshot 2024-10-27 113431](https://github.com/user-attachments/assets/021d45f4-f192-4a34-9b9c-0e76970ed9a4)

# Soal 11
Lalu buat untuk setiap request yang mengandung /titan akan di proxy passing menuju halaman https://attackontitan.fandom.com/wiki/Attack_on_Titan_Wiki (11) 
hint: (proxy_pass)

* Konfigurasi pada Colossal (Load Balancer PHP)
```
mkdir -p /etc/nginx/supersecret
htpasswd -b -c /etc/nginx/supersecret/htpasswd arminannie jrkmit35

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #    hash $request_uri consistent;
        #    least_conn;
        #    ip_hash;
    server 192.234.2.2;
    server 192.234.2.3;
    server 192.234.2.4;
}

server {
    listen 80;
    server_name eldia.it35.com www.eldia.it35.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        proxy_pass http://worker;
    }

    location /titan {
        proxy_pass http://attackontitan.fandom.com;
        proxy_set_header Host attackontitan.fandom.com;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/supersecret/htpasswd;
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

![Screenshot 2024-10-27 114029](https://github.com/user-attachments/assets/d1627138-1c39-4f54-9cad-a9046664af46)


# Soal 12
Selanjutnya Colossal ini hanya boleh diakses oleh client dengan IP [Prefix IP].1.77, [Prefix IP].1.88, [Prefix IP].2.144, dan [Prefix IP].2.156. (12) 
hint: (fixed in dulu clientnya)

* Konfigurasi pada Colossal (Load Balancer PHP)
```
mkdir -p /etc/nginx/supersecret
htpasswd -b -c /etc/nginx/supersecret/htpasswd arminannie jrkmit35

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
        #    hash $request_uri consistent;
        #    least_conn;
        #    ip_hash;
    server 192.234.2.2;
    server 192.234.2.3;
    server 192.234.2.4;
}

server {
    listen 80;
    server_name eldia.it35.com www.eldia.it35.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html index.php;

    server_name _;

    location / {
        allow 192.234.1.77;
        allow 192.234.1.88;
        allow 192.234.2.144;
        allow 192.234.2.156;
        deny all;
        proxy_pass http://worker;
    }

    location /titan {
        proxy_pass http://attackontitan.fandom.com;
        proxy_set_header Host attackontitan.fandom.com;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/supersecret/htpasswd;
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart

config pada tybur (zeke saja buat test)

echo 'host Zeke{
        hardware ethernet fa:61:fb:1a:8d:5b;
        fixed-address 192.246.1.77;
}
' >> /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart

```

* Contoh jika ip tidak sesuai yang diallow

![Screenshot 2024-10-27 115501](https://github.com/user-attachments/assets/3215b6b2-5ac2-41e8-b4aa-5a7c6571aaa0)

* Contoh jika ip sesuai yang diallow

![Screenshot 2024-10-27 115702](https://github.com/user-attachments/assets/c988e22c-f0da-42b1-bba7-29ca3c580ed9)

# Soal  13
Karena mengetahui bahwa ada keturunan marley yang mewarisi kekuatan titan, Zeke pun berinisiatif untuk menyimpan data data penting di Warhammer, dan semua data tersebut harus dapat diakses oleh anak buah kesayangannya, Annie, Reiner, dan Berthold.  (13)

* Konfigurasi pada Warhammer (Database)
```
apt-get update
apt-get install mariadb-server -y
service mysql start

echo '
[client-server]
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/
[mysqld]
skip-networking=0
skip-bind-address
' > /etc/mysql/my.cnf 

sed -i 's/127.0.0.1/0.0.0.0/g' /etc/mysql/mariadb.conf.d/50-server.cnf

service mysql restart
```

**Lalu jalankan step di bawah**

`mysql -u root -p`

(password perlu diisi langsung enter)

```
CREATE USER 'it35'@'%' IDENTIFIED BY 'it35';
CREATE USER 'it35'@'localhost' IDENTIFIED BY 'it35';
CREATE DATABASE dbit35;
GRANT ALL PRIVILEGES ON *.* TO 'it35'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'it35'@'localhost';
FLUSH PRIVILEGES;
```

**Cek di setiap worker**
`mysql --host=192.234.3.2 --port=3306 --user=it35 --password=it35 dbit35 -e "SHOW DATABASES;"`

![Screenshot 2024-10-27 121822](https://github.com/user-attachments/assets/5681ccfc-3aa3-4019-bacf-4bc80a0f432e)

![Screenshot 2024-10-27 121937](https://github.com/user-attachments/assets/121c2594-0973-4b9e-818c-bcb7dc119d31)

![Screenshot 2024-10-27 122118](https://github.com/user-attachments/assets/11ab7510-f65d-4df2-8146-5ae3bb5da03d)









