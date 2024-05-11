[Running WordPress Behind SSL and NGINX Reverse Proxy | ldev blog](https://blog.ldev.app/running-wordpress-behind-ssl-and-nginx-reverse-proxy/)

> If you are behind a reverse proxy that does SSL/TLS for you, or in a similar situation, wordpress needs to know that this is the case (otherwise it will assume unencrypted http and will make all links to references unencrypted). If http gets redirected to https this can cause problems.
> 
> You can resolve this by configuring the webserver to add certain headers, e.g. for Nginx inside the server block, add:
> 
> ```bash
> location /blog/ {
>   proxy_pass http://backend:8081/;
>   proxy_set_header X-Forwarded-Host $host;
>   proxy_set_header X-Forwarded-Proto $scheme;
> }
> ```
> 
> and in the wp-config.php (usually created after installation, so you'll have to do the install without styling):
> 
> ```php
> /**
>  * Handle SSL reverse proxy
>  */
> if ($_SERVER['HTTP_X_FORWARDED_PROTO'] == 'https')
>     $_SERVER['HTTPS']='on';
> 
> if (isset($_SERVER['HTTP_X_FORWARDED_HOST'])) {
>     $_SERVER['HTTP_HOST'] = $_SERVER['HTTP_X_FORWARDED_HOST'];
> }
> ```
> 
> I had the same problem and found this out here: [https://www.variantweb.net/blog/wordpress-behind-an-nginx-ssl-reverse-proxy/](https://www.variantweb.net/blog/wordpress-behind-an-nginx-ssl-reverse-proxy/)

