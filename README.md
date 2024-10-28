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
	address 192.234.3.4
	netmask 255.255.255.0
	gateway 192.234.3.0
```

## Beast (Load Balancer Laravel)
```
auto eth0
iface eth0 inet static
	address 192.234.3.2
	netmask 255.255.255.0
	gateway 192.234.3.0
```

## Colossal (Load Balancer PHP)
```
auto eth0
iface eth0 inet static
	address 192.234.3.3
	netmask 255.255.255.0
	gateway 192.234.3.0
```

## Annie (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 192.234.1.2
	netmask 255.255.255.0
	gateway 192.234.1.0
```

## Bertholdt (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 192.234.1.3
	netmask 255.255.255.0
	gateway 192.234.1.0
```

## Reiner (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 192.234.1.4
	netmask 255.255.255.0
	gateway 192.234.1.0
```

## Armin (PHP Worker)
``` 
auto eth0
iface eth0 inet static
	address 192.234.2.2
	netmask 255.255.255.0
	gateway 192.234.2.0
```

## Eren (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 192.234.2.3
	netmask 255.255.255.0
	gateway 192.234.2.0
```

## Mikasa (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 192.234.2.4
	netmask 255.255.255.0
	gateway 192.234.2.0
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

* Fritz1.sh (DNS Server)

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

* tes ping `marley.it35.com`

![Screenshot 2024-10-27 013651](https://github.com/user-attachments/assets/75d31176-bfb5-46eb-8609-afe8515869eb)


# Soal 1-5

>Nomor 2
* konfigurasi pada Tybur (DHCP Server)
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

* tes ping `marley.it35.com`

![Screenshot 2024-10-27 013651](https://github.com/user-attachments/assets/75d31176-bfb5-46eb-8609-afe8515869eb)

# Soal 6
Armin berinisiasi untuk memerintahkan setiap worker PHP untuk melakukan konfigurasi virtual host untuk website berikut https://intip.in/BangsaEldia dengan menggunakan php 7.3 (6)

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

* Script untuk Armin, Eren, Mikasa
```
#!/bin/bash

# Update package list and install necessary packages
apt-get update
apt-get install lynx nginx wget unzip php7.3 php-fpm -y

# Start PHP-FPM and Nginx services
service php7.3-fpm start
service nginx start

# Create download directory
mkdir -p /var/www/html/download/

# Download the ZIP file from Google Drive
wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1yliJkxu-3XmgJ6Xb37pGc2Jht5NTO9oj' -O /var/www/html/download/bangsa-eldia.zip

# Unzip the downloaded file
unzip /var/www/html/download/bangsa-eldia.zip -d /var/www/html/download

# Move extracted files to the web root
mv /var/www/html/download/bangsa-eldia/modul-3/* /var/www/html/

# Clean up by removing the download directory
rm -rf /var/www/html/download/

# Configure Nginx
cat <<EOL > /etc/nginx/sites-available/it35.conf
server {
    listen 80;

    root /var/www/html;

    index index.php index.html index.htm;

    server_name _;

    location / {
        try_files \$uri \$uri/ /index.php?\$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
    }

    error_log /var/log/nginx/it35_error.log;
    access_log /var/log/nginx/it35_access.log;
}
EOL

# Enable the site configuration
ln -s /etc/nginx/sites-available/it35.conf /etc/nginx/sites-enabled/

# Remove the default Nginx site
rm /etc/nginx/sites-enabled/default

# Restart Nginx and PHP-FPM services
service nginx restart
service php7.3-fpm restart
```

* Test di Armin
`lynx 192.234.2.2`

![Screenshot 2024-10-27 024526](https://github.com/user-attachments/assets/d770bd2c-258c-41f2-a8a4-e7d6dbcbfbfe)

* Test di Eren
`lynx 192.234.2.3`

![Screenshot 2024-10-27 024054](https://github.com/user-attachments/assets/228678ef-47e9-46a1-9e4b-5e8baf0ca653)

* Test di Mikasa
`lynx 192.234.2.4`

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

`ab -n 6000 -c 200 http://192.234.3.3/`

![Screenshot 2024-10-27 031809](https://github.com/user-attachments/assets/48b9855a-3209-41a7-8319-2c43223aa9ab)
![Screenshot 2024-10-27 031710](https://github.com/user-attachments/assets/e379758d-4f3c-4d87-89d2-24d930c6d868)

# Soal 8
Karena Erwin meminta “laporan kerja Armin”, maka dari itu buatlah analisis hasil testing dengan 1000 request dan 75 request/second untuk masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
a. Nama Algoritma Load Balancer
b. Report hasil testing pada Apache Benchmark
c. Grafik request per second untuk masing masing algoritma. 
d. Analisis (8)

* Script Colossal8.sh
```
#!/bin/bash

# Konfigurasi Round Robin
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/round_robin

echo '
    upstream round-robin {
        server 192.234.2.2;
        server 192.234.2.3;
        server 192.234.2.4;
    }

    server {
        listen 81;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://round-robin;
        }
    }
' > /etc/nginx/sites-available/round_robin

# Konfigurasi Weighted Round Robin
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/weight_round_robin

echo '
    upstream weight_round-robin {
        server 192.234.2.2 weight=3;
        server 192.234.2.3 weight=2;
        server 192.234.2.4 weight=1;
    }

    server {
        listen 82;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://weight_round-robin;
        }
    }
' > /etc/nginx/sites-available/weight_round_robin

# Konfigurasi Generic Hash
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/generic_hash

echo '
    upstream generic_hash {
        hash $request_uri consistent;
        server 192.234.2.2 weight=3;
        server 192.234.2.3 weight=2;
        server 192.234.2.4 weight=1;
    }

    server {
        listen 83;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://generic_hash;
        }
    }
' > /etc/nginx/sites-available/generic_hash

# Konfigurasi IP Hash
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/ip_hash

echo '
    upstream ip_hash {
        ip_hash;
        server 192.234.2.2;
        server 192.234.2.3;
        server 192.234.2.4;
    }

    server {
        listen 84;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://ip_hash;
        }
    }
' > /etc/nginx/sites-available/ip_hash

# Konfigurasi Least Connections
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/least_connection

echo '
    upstream least_connection {
        least_conn;
        server 192.234.2.2;
        server 192.234.2.3;
        server 192.234.2.4;
    }

    server {
        listen 85;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://least_connection;
        }
    }
' > /etc/nginx/sites-available/least_connection

# Aktifkan semua konfigurasi load balancer
ln -sf /etc/nginx/sites-available/round_robin /etc/nginx/sites-enabled/
ln -sf /etc/nginx/sites-available/weight_round_robin /etc/nginx/sites-enabled/
ln -sf /etc/nginx/sites-available/generic_hash /etc/nginx/sites-enabled/
ln -sf /etc/nginx/sites-available/ip_hash /etc/nginx/sites-enabled/
ln -sf /etc/nginx/sites-available/least_connection /etc/nginx/sites-enabled/

# Restart Nginx
service nginx restart
```

```
  	# hash $request_uri consistent;
        #    least_conn;
        #    ip_hash;
```

Ada 4 algoritma, round robin, hash, least connection, ip hash. Uncomment salah satu atau tidak sama sekali

- Round Robin
  `ab -n 1000 -c 75 http://192.234.3.3:81/`
  
![Screenshot 2024-10-27 032826](https://github.com/user-attachments/assets/e4bd8e22-3435-4b42-a633-b1f6814f1acb)
![Screenshot 2024-10-27 032837](https://github.com/user-attachments/assets/5e549817-1db4-4247-a3b8-66f14e18f19a)

- Weight Round-Robin
  `ab -n 1000 -c 75 http://192.234.3.3:82/`
  
![Screenshot 2024-10-27 032929](https://github.com/user-attachments/assets/a7eb14f8-7609-4158-b9b7-b2174adf4436)
![Screenshot 2024-10-27 033005](https://github.com/user-attachments/assets/a31f6fcc-34af-4cb3-aa14-6e12b1ffc44e)

- Generic Hash
  `ab -n 1000 -c 75 http://192.234.3.3:83/`

![Screenshot 2024-10-27 033059](https://github.com/user-attachments/assets/2212b0d9-fef5-4a2b-a03c-32192f72330a)
![Screenshot 2024-10-27 033109](https://github.com/user-attachments/assets/5c491869-4e29-4f6c-be20-b1e1831c497c)

- IP Hash
  `ab -n 1000 -c 75 http://192.234.3.3:84/`
  
![Screenshot 2024-10-27 033147](https://github.com/user-attachments/assets/b07c55bd-2f32-4c3f-928e-4035b4e3c5bf)
![Screenshot 2024-10-27 033200](https://github.com/user-attachments/assets/10d3d1fb-5b91-46ae-b0e7-b81137f6ffcc)

- Least Connection
  `ab -n 1000 -c 75 http://192.234.3.3:85/`

![Screenshot 2024-10-27 033244](https://github.com/user-attachments/assets/f908de9c-f2e0-47ed-9743-adfbbef180a1)
![Screenshot 2024-10-27 033235](https://github.com/user-attachments/assets/ada4383c-fbed-4f5d-8b61-bff758757f24)

### Grafik dan Analisis

![image](https://github.com/user-attachments/assets/2614d84c-af35-4246-9d96-1610f849b7bf)

Analisis: 
> Generic Hash memiliki performa terbaik dengan RPS tertinggi, diikuti oleh Weight Round-Robin. Round Robin berada di tengah, sementara IP Hash memiliki RPS terendah. Generic Hash paling optimal untuk beban tinggi, sedangkan IP Hash cocok untuk kestabilan sesi.

  
# Soal 9
Dengan menggunakan algoritma Least-Connection, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 1000 request dengan 10 request/second, kemudian tambahkan grafiknya pada “laporan kerja Armin”. (9)

* Script Colossal9.sh
```
# Konfigurasi Least Connections
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/least_connection

echo '
    upstream least_connection {
        least_conn;
        server 192.234.2.2;
        server 192.234.2.3;
        server 192.234.2.4;
    }

    server {
        listen 85;
        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://least_connection;
        }
    }
' > /etc/nginx/sites-available/least_connection

ln -sf /etc/nginx/sites-available/least_connection /etc/nginx/sites-enabled/

# Restart Nginx
service nginx restart
```

* Grafik dan Analisa
  
![image](https://github.com/user-attachments/assets/b21c436d-e4a8-48aa-a6ab-2f8c186ca652)

Analisa:
>Dari hasil uji coba, Generic Hash memiliki performa terbaik dengan RPS tertinggi (696.55), diikuti oleh Weight Round-Robin (625.65), Round Robin (486.31), dan IP Hash yang terendah (435.29). Generic Hash paling optimal untuk penyeimbangan beban, sementara IP Hash lebih cocok untuk kebutuhan penanganan sesi yang stabil.


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









