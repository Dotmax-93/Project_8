## Configure Apache As A Load Balancer

Create an Ubuntu Server 22.04 EC2 instance and open custom TCP port 80

Install Apache Load Balancer and configure it to point traffic coming to LB to both Web Servers(created in project 7)

# Install Apache

`sudo apt update`

`sudo apt install apache2 -y`

`sudo apt-get install libxml2-dev`

# Enable following modules

`sudo a2enmod rewrite`

`sudo a2enmod proxy`

`sudo a2enmod proxy_balancer`

`sudo a2enmod proxy_http`

`sudo a2enmod headers`

`sudo a2enmod lbmethod_bytraffic`

`sudo systemctl restart apache2`

`sudo systemctl status apache2`

# Configure load balancing

`sudo vi /etc/apache2/sites-available/000-default.conf`![alt text](./Images/Screen%20Shot%202023-03-14%20at%2016.56.45.png)

<Proxy "balancer://mycluster">
               BalancerMember http://<WebServer1-Private-IP-Address>:80
             loadfactor=5 timeout=1
               BalancerMember http://<WebServer2-Private-IP-Address>:80 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
 </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/

# Restart apache server

`sudo systemctl restart apache2`

http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php![alt text](./Images/Screen%20Shot%202023-03-14%20at%2016.55.55.png)


