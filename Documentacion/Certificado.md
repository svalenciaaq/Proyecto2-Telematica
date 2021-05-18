





```
sudo amazon-linux-extras install epel -y

sudo yum install certbot-nginx -y

sudo yum install nginx -y
```

```
sudo certbot --server https://acme-v02.api.letsencrypt.org/directory -d *.sudominio.com --manual --preferred-challenges dns-01 certonly
```


```

mkdir /home/ec2-user/wordpress
mkdir /home/ec2-user/wordpress/ssl
sudo su


```


```
cp /etc/letsencrypt/live/sudominio.com/* /home/ec2-user/wordpress/ssl/

cp /etc/letsencrypt/options-ssl-nginx.conf /home/ec2-user/wordpress/ssl/
cp /etc/letsencrypt/ssl-dhparams.pem /home/ec2-user/wordpress/ssl/
exit
```


```
git clone https://github.com/svalenciaaq/cetificadosSSL.git

cd svalenciaaq/certificadoSSL
sudo cp docker-compose.yml /home/ec2-user/wordpress
sudo cp nginx.conf /home/ec2-user/wordpress
sudo cp ssl.conf /home/ec2-user/wordpress

```


```
ps ax | grep nginx
netstat -an | grep 80

sudo reboot
```


```
cd /home/ec2-user/wordpress
docker-compose up --build -d
```