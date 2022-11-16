
[https://www.youtube.com/watch?v=bllS9tkCkaM]

## Setup Image

- Update the system

## Install TOR

``` 
sudo apt install tor
sudo nano /etc/tor/torrc
```

Uncomment HiddenServiceDir and HiddenServicePort
Save and exit

```
sudo service tor stop
sudo service tor start

cat /var/lib/tor/hidden_service/hostname
```

hvikudrnymgd7ffgyrtojl45eawanuswoyslsnt4vq2pd7x6nqasmbad.onion

## Install nginx

```
sudo apt install nginx
sudo service nginx start

sudo nanno /etc/nginx/nginx.conf
```
uncomment server_tokens off and server_name_in_redirect off
add port_in_redirect off after the server_tokens line



nginx web site is located at /var/www/html