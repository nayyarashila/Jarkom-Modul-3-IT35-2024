# Jarkom-Modul-3-IT35-2024

## Topologi

![Screenshot 2024-10-22 212315](https://github.com/user-attachments/assets/32a46951-f329-4af1-8066-edfe7f0c8a68)

## Setup Config

# paradis
```
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.234.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.234.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.234.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.234.4.0
	netmask 255.255.255.0

iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.234.0.0/16
```

# Fritz
```
auto eth0
iface eth0 inet static
	address 192.234.4.2
	netmask 255.255.255.0
	gateway 192.234.4.0
```

# Tybur
```
auto eth0
iface eth0 inet static
	address 192.234.4.3
	netmask 255.255.255.0
	gateway 192.234.4.0
```

# Warhammer
```
auto eth0
iface eth0 inet static
	address 192.234.3.4
	netmask 255.255.255.0
	gateway 192.234.3.0
```

# Beast
```
auto eth0
iface eth0 inet static
	address 192.234.3.2
	netmask 255.255.255.0
	gateway 192.234.3.0
```

# Colossal
```
auto eth0
iface eth0 inet static
	address 192.234.3.3
	netmask 255.255.255.0
	gateway 192.234.3.0
```

# Annie
```
auto eth0
iface eth0 inet static
	address 192.234.1.2
	netmask 255.255.255.0
	gateway 192.234.1.0
```

# Bertholdt
```
auto eth0
iface eth0 inet static
	address 192.234.1.3
	netmask 255.255.255.0
	gateway 192.234.1.0
```

# Reiner
```
auto eth0
iface eth0 inet static
	address 192.234.1.4
	netmask 255.255.255.0
	gateway 192.234.1.0
```

# Armin
```
auto eth0
iface eth0 inet static
	address 192.234.2.2
	netmask 255.255.255.0
	gateway 192.234.2.0
```

# Eren
```
auto eth0
iface eth0 inet static
	address 192.234.2.3
	netmask 255.255.255.0
	gateway 192.234.2.0
```

# Mikasa
```
auto eth0
iface eth0 inet static
	address 192.234.2.4
	netmask 255.255.255.0
	gateway 192.234.2.0
```

# Erwin & Zeke
```
auto eth0
iface eth0 inet dhcp
```

## Soal 0
Pulau Paradis telah menjadi tempat yang damai selama 1000 tahun, namun kedamaian tersebut tidak bertahan selamanya. Perang antara kaum Marley dan Eldia telah mencapai puncak. Kaum Marley yang dipimpin oleh Zeke, me-register domain name marley.yyy.com untuk worker Laravel mengarah pada Annie. Namun ternyata tidak hanya kaum Marley saja yang berinisiasi, kaum Eldia ternyata sudah mendaftarkan domain name eldia.yyy.com untuk worker PHP (0) mengarah pada Armin.

* Fritz1.sh

```
  apt-get update
apt-get install bind9 -y

forward="options {
directory \"/var/cache/bind\";
forwarders {
  	   192.168.122.1;
};

allow-query{any;};
listen-on-v6 { any; };
};
"
echo "$forward" > /etc/bind/named.conf.options

echo "zone \"marley.it35.com\" {
	type master;
	file \"/etc/bind/jarkom/marley.it35.com\";
};

zone \"eldia.it35.com\" {
	type master;
	file \"/etc/bind/jarkom/eldia.it35.com\";
};
" > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

riegel="
;
;BIND data file for local loopback interface
;
\$TTL    604800
@    IN    SOA    marley.it35.com. root.marley.it35.com. (
        2        ; Serial
                604800        ; Refresh
                86400        ; Retry
                2419200        ; Expire
                604800 )    ; Negative Cache TTL
;                   
@    IN    NS    marley.it25.com.
@       IN    A    192.234.1.2
"
echo "$riegel" > /etc/bind/jarkom/marley.it35.com

granz="
;
;BIND data file for local loopback interface
;
\$TTL    604800
@    IN    SOA    eldia.it35.com. root.eldia.it35.com. (
        2        ; Serial
                604800        ; Refresh
                86400        ; Retry
                2419200        ; Expire
                604800 )    ; Negative Cache TTL
;                   
@    IN    NS    eldia.it25.com.
@       IN    A    192.234.2.2
"
echo "$granz" > /etc/bind/jarkom/eldia.it35.com

service bind9 restart
```

* tes ping `marley.it35.com`

![Screenshot 2024-10-27 013651](https://github.com/user-attachments/assets/75d31176-bfb5-46eb-8609-afe8515869eb)


## Soal 1-5

* paradis1.sh
```
apt-get update
apt install isc-dhcp-relay -y

service isc-dhcp-relay start 

echo '# Defaults for isc-dhcp-relay initscript

# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="192.234.4.3" 

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' > /etc/default/isc-dhcp-relay


echo net.ipv4.ip_forward=1 > /etc/sysctl.conf

service isc-dhcp-relay restart
```

* Tybur1.sh
```
apt-get update
apt-get install isc-dhcp-server -y

echo 'INTERFACESv4="eth0"
INTERFACESv6=""
' > /etc/default/isc-dhcp-server

subnet="option domain-name \"example.org\";
option domain-name-servers ns1.example.org, ns2.example.org;

default-lease-time 600;
max-lease-time 7200;

ddns-update-style-none;

subnet 192.234.1.0 netmask 255.255.255.0 {
    range 192.234.1.5 192.234.1.25;
    range 192.234.1.50 12.234.1.100;
    option routers 192.234.1.1;
    option broadcast-address 192.234.1.255;
    option domain-name-servers 192.234.4.2;
    default-lease-time 360;
    max-lease-time 5220;
}

subnet 192.234.2.0 netmask 255.255.255.0 {
    range 192.234.2.9 192.234.2.27;
    range 192.234.2.81 192.234.2.243;
    option routers 192.234.2.1;
    option broadcast-address 192.234.2.255;
    option domain-name-servers 192.234.4.2;
    default-lease-time 1800;
    max-lease-time 5220;
}

subnet 192.234.3.0 netmask 255.255.255.0 {
}

subnet 192.234.4.0 netmask 255.255.255.0 {
}

"
echo "$subnet" > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

* tes ping `marley.it35.com`

![Screenshot 2024-10-27 013651](https://github.com/user-attachments/assets/75d31176-bfb5-46eb-8609-afe8515869eb)

## Soal 6
Armin berinisiasi untuk memerintahkan setiap worker PHP untuk melakukan konfigurasi virtual host untuk website berikut https://intip.in/BangsaEldia dengan menggunakan php 7.3 (6)

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

## Soal 7

Script Colossal7.sh
```
#!/bin/bash

# Konfigurasi nameserver
echo -e '
nameserver 192.234.3.3
nameserver 192.168.122.1
' > /etc/resolv.conf

# Update dan install paket yang diperlukan
apt-get update
apt-get install apache2-utils -y   # Untuk testing menggunakan ApacheBench (ab)
apt-get install nginx -y           # Install Nginx sebagai load balancer
apt-get install lynx -y            # Install lynx untuk akses web via command line

# Konfigurasi Nginx untuk load balancing menggunakan PHP load balancer
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/colossal_lb

# Tambahkan konfigurasi load balancing
echo '
    upstream php_backend {
        server 192.234.2.2;  # Armin
        server 192.234.2.3;  # Eren
        server 192.234.2.4;  # Mikasa
    }

    server {
        listen 80;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;
        server_name _;

        location / {
            proxy_pass http://php_backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
' > /etc/nginx/sites-available/colossal_lb

# Aktifkan konfigurasi load balancer
ln -sf /etc/nginx/sites-available/colossal_lb /etc/nginx/sites-enabled/

# Hapus default jika masih ada
if [ -f /etc/nginx/sites-enabled/default ]; then
    rm /etc/nginx/sites-enabled/default
fi

# Restart layanan Nginx agar konfigurasi baru berlaku
service nginx restart
```
* Test di client

`ab -n 6000 -c 200 http://192.234.3.3/`

![Screenshot 2024-10-27 031809](https://github.com/user-attachments/assets/48b9855a-3209-41a7-8319-2c43223aa9ab)
![Screenshot 2024-10-27 031710](https://github.com/user-attachments/assets/e379758d-4f3c-4d87-89d2-24d930c6d868)

## Soal 8
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

## Soal 9
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

* Jalankan di client
`ab -n 1000 -c 75 http://192.234.3.3/`










