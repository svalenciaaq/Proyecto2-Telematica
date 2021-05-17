
## Creacion de la instacian EC2

![MON1](../img/mon1.PNG)
![MON2](../img/mon2.PNG)
![MON3](../img/mon3.PNG)
![MON4](../img/mon4.PNG)
![MON5](../img/mon5.PNG)
![MON6](../img/mon6.PNG)
![MON7](../img/mon7.PNG)









## instalacion de docker

```
sudo amazon-linux-extras install docker 

sudo yum install git 

sudo systemctl enable docker 

sudo systemctl start docker 

sudo usermod -a -G docker ec2-user 

```

## Instalación Docker-compose en la VM de AWS EC2

```

sudo curl -L https://github.com/docker/compose/releases/download/1.28.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose


sudo chmod +x /usr/local/bin/docker-compose

sudo reboot

```

## Instalacion Wordpress con Docker en AWS

```
version: '3.1'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - wordpress:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - db:/var/lib/mysql

volumes:
  wordpress:
  db:

```