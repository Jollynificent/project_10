#update server and install nginx
`sudo apt update && sudo apt install nginx`

#enable nginx at start boot
`sudo systemctl enable nginx && sudo systemctl start nginx`

#confirm nginx status
`sudo systemctl status nginx`

`sudo nano /etc/nginx/sites-available/load_balancer.conf`
#insert and save
upstream web {
    server 13.40.81.52;
    server 18.134.17.43;
  }

server {
    listen 80;
    server_name toolingyf.link www.toolingyf.link;
    location / {
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_pass http://web;
    }
  }
#redirect
`sudo rm -f /etc/nginx/sites-enabled/default`

#check nginx configuration
`sudo nginx -t`

#change directory
`cd /etc/nginx/sites-enabled`

#link load balancer in sites available to sites enabled
`sudo ln -s ../sites-available/load_balancer.conf .`

`sudo systemctl reload nginx`
![alt text](login.jpg)

#install certbot and request security
`sudo systemctl status snapd`
`sudo snap install --classic certbot`
`sudo ln -s /snap/bin/certbot /usr/bin/certbot`
`sudo certbot --nginx`
![alt text](secured.jpg)

#setup renewal of certificate
`sudo certbot renew --dry-run`
`crontab -e`
choose 1 for nano and add line

* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1


