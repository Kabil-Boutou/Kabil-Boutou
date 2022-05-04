#Nada
#there is also a way to config nginx with two separet files each one of theme #have a server_name that works as a listner on the subdomain and make #those files symbloqiue link after server name just write the redirect or root
server {
        listen 443 ssl default_server;
        listen [::]:443 ssl default_server;
        ssl on;
        ssl_certificate /etc/ssl/sahamsn.crt;
        ssl_certificate_key /etc/ssl/sahamsn.key;
        rewrite ^ĥttps://$http_host$request_uri? permanent;

        #root /var/www/html/boa_sn;
        # Add index.php to the list if you are using PHP
        index index.html;

        #BOA SN Backend
        location /api/v1 {
                return 301 https://zenit.sahamassurance.sn:4037/api/v1;
        }

        #INTOUCH SN Backend
         location /gateway {
                return 301 https://touchit.sahamassurance.sn:4000/gateway;
        }
        location /micro_users {
                return 301 https://touchit.sahamassurance.sn:4007/micro_users;
        }
        location /micro_params {
                return 301 https://touchit.sahamassurance.sn:4010/micro_params;
        }
        location /micro_simulation {
                return 301 https://touchit.sahamassurance.sn:4008/micro_simulation;
        }
        location /micro_contrats {
                return 301 https://touchit.sahamassurance.sn:4006/micro_contrats;
        }
        location / {
                #set $ptry  /index.html;
                if ( $host = zenit.sahamassurance.sn ){
                        root /var/www/html/boa_sn;
                        error_page 404 =200 /index.html;
                }
                if ( $host = touchit.sahamassurance.sn ){
                        root /var/www/html/intouch;
                        error_page 404 =200 /index.html;
                }
                #location / {
                        #try_files $uri /index.html;
                #}
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                #try_files $uri $ptry = 404;

         }

}


server {
        listen 80 default_server;
        listen [::]:80 default_server;
        return 301 https://$host;

}

https://docs.nginx.com/nginx/admin-guide/web-server/compression/
