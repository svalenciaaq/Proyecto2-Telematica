
- [Documentacion para la creacion de un sistema escalable basado en wordpress](#documentacion-para-la-creacion-de-un-sistema-escalable-basado-en-wordpress)
  - [Introduccion](#introduccion)
  - [Recursos](#recursos)
  - [Desarollo](#desarollo)
  - [Configuracion](#configuracion)
    - [Creacion y configurar la vpc](#creacion-y-configurar-la-vpc)
    - [Creacion y configuracion de las subredes](#creacion-y-configuracion-de-las-subredes)
    - [Creacion y configuracion de las NATs instance](#creacion-y-configuracion-de-las-nats-instance)
    - [Creacion y configuracion de las tablas de enrutamiento](#creacion-y-configuracion-de-las-tablas-de-enrutamiento)
    - [Creacion y configuracion de los bastion hosts](#creacion-y-configuracion-de-los-bastion-hosts)
    - [Creacion de los security gropu para trafico web y para la base de datos y](#creacion-de-los-security-gropu-para-trafico-web-y-para-la-base-de-datos-y)
    - [Creacion y configuracion del servidor de bases de datos en la subred privada](#creacion-y-configuracion-del-servidor-de-bases-de-datos-en-la-subred-privada)
    - [Creacion y configuracion el sistema de archivos compartidos EFS](#creacion-y-configuracion-el-sistema-de-archivos-compartidos-efs)
    - [Creacion y configurar la instancia del servidor web wordpress](#creacion-y-configurar-la-instancia-del-servidor-web-wordpress)
    - [Instalacion y configuracion de la istancia wordpress](#instalacion-y-configuracion-de-la-istancia-wordpress)
    - [Creacion de la AMi para el servicio de auto scaling](#creacion-de-la-ami-para-el-servicio-de-auto-scaling)
    - [Creacion y configuracion del servicio de auto scaling](#creacion-y-configuracion-del-servicio-de-auto-scaling)




# Documentacion para la creacion de un sistema escalable basado en wordpress

## Introduccion


## Recursos


## Desarollo

## Configuracion

### Creacion y configurar la vpc

### Creacion y configuracion de las subredes

### Creacion y configuracion de las NATs instance

### Creacion y configuracion de las tablas de enrutamiento
Las tablas de ruteo contienen conjuntos de reglas, denominadas rutas, que se usan para determinar a dónde se dirige el tráfico de red desde nuestra subred o gateway. 

Tipos de tabla de enrutamiento 

Existen varios tipos de tablas de enrutamiento, entre ellas estan: 

 

- **Tabla de enrutamiento principal:** la tabla de enrutamiento que viene de forma automática con la VPC. Controla el direccionamiento de todas las subredes que no están explícitamente asociadas a ninguna otra tabla de ruteo. 

- **Tabla de enrutamiento personalizada:** una tabla de enrutamiento que se crea para la VPC. 

- **Asociación de borde:** una tabla de enrutamiento que se utiliza para enrutar el tráfico de VPC entrante a un dispositivo. Asocie una tabla de ruteo a la gateway de Internet o a la gateway privada virtual y especifique la interfaz de red del dispositivo como objetivo para el tráfico de la VPC. 

- **Asociación de tabla de enrutamiento:** la asociación entre una tabla de enrutamiento y una subred, gateway de Internet o gateway privada virtual. 

- **Tabla de enrutamiento de subred:** una tabla de enrutamiento asociada con una subred. 

- **Tabla de enrutamiento de gateway**: una tabla de enrutamiento asociada con una gateway de Internet o gateway privada virtual. 

- **Tabla de enrutamiento de gateway local**: una tabla de enrutamiento asociada con una gateway local de Outposts. Para obtener información acerca de las gateways locales, consulte Gateways locales en la Guía del usuario de AWS Outposts. 

- **Destino**: el intervalo de direcciones IP a las que desea que vaya el tráfico (CIDR de destino). Por ejemplo, una red corporativa externa con un CIDR 172.16.0.0/12. 

- **Propagación**: la propagación de rutas permite que una gateway privada virtual propague rutas automáticamente a las tablas de enrutamiento. Esto significa que no es necesario introducir manualmente rutas de VPN en las tablas de ruteo. Para obtener más información acerca de las opciones de enrutamiento de VPN, consulte Opciones de enrutamiento de la VPN de sitio a sitio en la Guía del usuario de la VPN de sitio a sitio. 

- **Destino:** la gateway, interfaz de red o conexión a través de la cual enviar el tráfico de destino, por ejemplo, una gateway de Internet. 

- **Ruta local:** una ruta predeterminada para la comunicación dentro de la VPC. 

Para este proyecto se usará la tabla de enrutamiento personalizada. 

De forma predeterminada, cada tabla de ruta personalizada que creemos ya tendrá la ruta local dentro, lo que permitirá que la comunicación fluya entre todos los recursos y subredes dentro de la VPC. Cuando utilizamos el asistente de la VPC en la consola para crear una VPC con una gateway de Internet, el asistente crea una tabla de ruteo personalizada y agrega una ruta a la gateway de Internet. Una forma de proteger la VPC es dejar la tabla de ruteo principal en su estado predeterminado original. Después, asociar de forma explícita cada nueva subred que creemos a una de las tablas de ruteo personalizadas que hayamos creado. De este modo, se asegurará de que controlamos de manera explícita el modo en que cada subred direcciona el tráfico. 

De este modo, podremos añadir, quitar y modificar rutas en la tabla de ruteo personalizada.  

Si bien la tabla de enrutamiento principal controla el enrutamiento de nuestra VPC, se quiere ser más detallado sobre cómo enrutamos el tráfico para subredes específicas. La aplicación consta de una interfaz y una base de datos. Crearemos subredes independientes para estos recursos y se proporcionara diferentes rutas para cada uno de ellos. Al asociar una tabla de enrutamiento personalizada con una subred, la subred la usará en lugar de la tabla de enrutamiento principal.  

 

**Pasos para crear y configurar la tabla de enrutamiento**


Cada subred de una VPC debe estar asociada a una tabla de ruteo. Por ende se deben seguir los siguientes pasos

1. Abrimos la consola y nos dirijimos a la configuración de vpc 

	 

2. Nos vamos a la opción route tables en la izquierda  

	 

3. Creamos una nueva ruta  

	 

4. Ponemos un nombre y seleccionamos nuestra VPC 

	 

5. Este proceso lo repetiremos 4 veces poniendo los siguientes nombres  

 

   * Private Route Table A 

   * Private Route Table B 

    * Public Route Table A 

    * Public Route Table A 

 
**Asociar subredes a las tablas**  

Ahora asociaremos las tablas a cada subred creada en el numeral anterior.  

Ahora se debe crear las rutas por defecto. Para el caso de la subred privada, el tráfico debe ser enviado al NAT Gateway. Para esto en el panel izquierdo, seleccione la opción de Route Tables. Seleccione la subred privada “Private Route Table” y abajo, en el panel de “Routes” 

* Click en “Edit routes”. 
* Click en “Add route”. 
* Destination: 0.0.0.0/0 

* Target: Seleccione la instancia del NAT Gateway (NAT-GW-Instance) que se desplegó para cada AZ. 

* Click en “Save routes”. 

Debe observar el mensaje de que la ruta se ha editado de manera exitosa. 

 
 

Por otro lado, para el caso de la red pública, este tráfico debe ser enviado al Internet Gateway. Para esto en el panel izquierdo, seleccione la opción de Route Tables. Seleccione la subred pública “Public Route Table” y abajo, en el panel de “Routes” 

* Click en “Edit routes”. 

* Click en “Add route”. 

* Destination: 0.0.0.0/0 

* Target: Seleccione el Internet Gateway (MyWebApp-IGW). 

* Click en “Save routes”. 

Debe observar el mensaje de que la ruta se ha editado de manera exitosa. 

 
 

Ahora debemos asociar a cada tabla de enrutamiento, las subredes. Seleccione la “Private Route Table A” y en la pestaña de “Subnet Associations”: 

* Click en “Edit subnet associations”. 

* Marque la casilla de la subred privada RR.RR.1.0/24. 

* Click en Save. 

 
 

Ahora debemos asociar a cada tabla de enrutamiento, las subredes. Seleccione la “Private Route Table B” y en la pestaña de “Subnet Associations”: 

* Click en “Edit subnet associations”. 

* Marque la casilla de la subred privada RR.RR.3.0/24. 

* Click en Save. 

 
 

Para el caso de las subredes públicas, seleccione la “Public Route Table A” y en la pestaña de “Subnet Associations”: 

* Click en “Edit subnet associations”. 

* Marque la casilla de la subred privada RR.RR.2.0/24. 

* Click en Save. 

 
 

Ahora, seleccione la “Public Route Table B” y en la pestaña de “Subnet Associations”: 

* Click en “Edit subnet associations”. 

* Marque la casilla de la subred privada RR.RR.4.0/24. 

* Click en Save. 

### Creacion y configuracion de los bastion hosts
- **Tabla de enrutamiento principal:**
	- Bastion Host AZ-A:  
	- Diríjase al “home”  
	- Escoja el servicio de EC2. 


![img](https://imgur.com/tF2v95l)

En el panel izquierdo seleccione la opción de “Instances” seleccione la opción “launch instances", luego: 

![img](https://imgur.com/SN2lEop)

- Escoja la imagen de Amazon Machine Image (AMI) 
	- Seleccione Amazon Linux 2 AMI (HVM), SSD Volume Type.  

![img](https://imgur.com/X6iw0mr)

- Seleccione el tipo de instancia t2.micro (columna type) 
	- Next : Configure Instance details. 

![img](https://imgur.com/yGyi4l5)

- Ahora configure, los siguientes parámetros:  
	- Network: MyLabVPC  
	- Subnet: Public Subnet A.  
	- Auto-assign Public IP: Enable 

![img](https://imgur.com/I0Surjy)


- Clic en “Next:Add storage”.  
- Clic en “Next:Add tags”, nueva llave.  
	- Llave: Name.  
	- Valor: Bastion Host. 
- Clic en “Next:Configure security group”:
	- Seleccione la opción de un security group existente. Seleccione “SG-Bastion”. 
- Clic en “Next: Review instance and launch”.  
- Clic en “Launch”.  
- Seleccione an existing key pair or create a new key pair. 
	- Seleccione el key pair creado anteriormente

### Creacion de los security gropu para trafico web y para la base de datos y


### Creacion y configuracion del servidor de bases de datos en la subred privada

### Creacion y configuracion el sistema de archivos compartidos EFS

### Creacion y configurar la instancia del servidor web wordpress

WordPress es un sistema de gestión de contenidos (CMS) que permite crear y mantener un blog u otro tipo de web. Un sistema de gestión de contenidos o CMS es un programa informático que permite crear un entorno de trabajo para la creación y administración de contenidos, principalmente en páginas web, por parte de los administradores, editores, participantes y demás usuarios 

WordPress dispone de un sistema de plugins, que permiten extender las capacidades de WordPress, de esa forma se consigue un CMS más flexible. 

 

 

En esta sección crearemos una instancia EC2 la cual actuará como Web Server. Esta VM estará asociada a la subred privada a la VPC. Esta instancia será creada para servir como Imagen AMI más adelante en el Autoscaling Group. 

 
**Pasos para la creación y configuración de la instancia del servidor** 

 

1. Diríjase al “home” de la consola de administración de AWS. Escoja el servicio de EC2.  

2. En el panel izquierdo seleccione la opción de “Instances” seleccione la opción “launch instances”.  

3. Escoja la imagen de Amazon Machine Image (AMI) la cual contiene la imagen del sistema operativo.
4.  Seleccione Amazon Linux 2 AMI (HVM), SSD Volume Type (click select). 
5.  Seleccione el tipo de instancia t2.micro (columna type) y click “configure instance details”. Ahora configure, los siguientes parámetros: 

    Network: MyLabVPC 

    Subnet: Private Subnet A 

    Auto-assign Public IP: disable 

    File systems: Click en “Add file system” 

    Seleccionar el EFS Creado anteriormente (WP-EFS) 

    Como ruta en vez de /mnt/efs/fs1   -> coloque /mnt/efs/wordpress 

    Des-seleccionar la opción: Automatically create and attach the required security groups. 

    Termine de configurar  

    Click en “Add storage”. 

    Click en “Add tags”. 

    Key: Name. 

    Value: Web Server. 

6. Click en “Configure security group”: 

7. Seleccione la opción de un security group existente. Seleccione “Web Security Group” 

8. Click en “Review and launch”. 

9. Editar el archivo /etc/fstab  

        {file_system_id_1}.efs.us-east-1.amazonaws.com:/ /mnt/efs/wordpress nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport,_netdev 0 0 

10. Bootear la maquina 

11. Verificar con los siguientes comandos 

        $ df -k 

        le debe aparecer el /mnt/efs/wordpress 

        Este directorio debe esta vacio, si tiene archivos borrelos: 

        $ sudo rm -rf /mnt/efs/wordpress/* 

12. Verificar que este corriendo 

        $ docker-compose up –d 

### Instalacion y configuracion de la istancia wordpress
Una vez conectado a la máquina vía ssh, se procede a instalar docker, docker-compose y el archivo docker-compose.yml de wordpress, para esto se requiere que ejecute los siguientes comandos: 

 

 

1. Abra la maquina ec2-web-wordpress 
 

2. Instale docker 
 

        sudo amazon-linux-extras install docker -y 

        sudo yum install git -y 

 
 

        sudo systemctl enable docker 

        sudo systemctl start docker 

        sudo usermod -a -G docker ec2-user 

 
 

3. instalar docker-compose: 

 
        sudo curl -L https://github.com/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose 

 
 

        sudo chmod +x /usr/local/bin/docker-compose 

        
        

        utilizar este docker-compose.yml: 

        
        

        more docker-compose.yml  

        version: '3.1' 

        services: 

          wordpress: 

            image: wordpress 

            restart: always 

            ports: 

              - 80:80 

            environment: 

              WORDPRESS_DB_HOST: dns-url-rdsdns-url-rds 

              WORDPRESS_DB_USER: exampleuser 

              WORDPRESS_DB_PASSWORD: examplepass 

              WORDPRESS_DB_NAME: exampledb 

            volumes: 

              - /mnt/efs/wordpress:/var/www/html 

        volumes: 

          wordpress: 
### Creacion de la AMi para el servicio de auto scaling
A continuación, vamos a crear una AMI del servidor web que contiene el wordpress. De esta forma se guardaran el contenido del boot disk y las nuevas instancias desplegadas a partir de esta se van a instanciar con un contenido idéntico. Es así como una Amazon Machine Image se convierte en una plantilla que contiene una configuración básica la cual sirve para instanciar posteriormente máquinas.

Para crear una AMI, en el home de la consola de administración, seleccione el servicio de EC2.

- En el panel izquierdo, click en Instances. Confirme que la instancia de la cual vamos a realizar la AMI esté corriendo (web server, 2/2 checks passed). Click en refresh para actualizar si es necesario.
- Seleccione Web Server.
- En el menú de Actions, click en Image \&gt; Create Image y configure los siguientes parámetros:
  - Image name: Web Server AMI
  - Image description: Lab AMI for Web Server
  - Click en Create Image. Observará un mensaje de confirmación que muestra el AMI ID para la nueva AMI.
  - Click en Close.

Ahora podrá usar esta AMI en el servicio de Auto Scaling.

##
**Crear y configurar un balanceador de carga.**

En esta sección, vamos a implementar un balanceador de carga. A través de este distribuiremos las peticiones entrantes hacia múltiples instancias y en diferentes zonas de disponibilidad. Para esto realizaremos lo siguiente:

- En el home de la consola, seleccione servicio de EC2.
- En el panel izquierdo, click Load Balancers.
- Click en Create Load Balancer.
- Como puede observar, se definen tres tipos diferentes de balanceadores de carga. Haga click en Create en la sección de Application Load Balancer.
- En la sección de configuración básica, configure los siguientes parámetros:
  - Name: ELB-MyWebApp
  - Scheme: Seleccione internet-facing.
  - IP address type: ipv4
  - VPC: MyWebApp-VPC.
- En la sección de Listeners, configure los siguientes parámetros:
  - Load Balancer Protocol: http.
  - Load Balancer Port: 80.
- En la sección de Availability Zones:
  - VPC: MyWebApp-VPC
  - Availability Zones: Seleccione la casilla de cada zona de disponibilidad. Y seleccione la subred pública para ambas zonas. Recuerde que el balanceador de carga va desplegado en la subred pública.
  - Click en Next: Configure Security Settings
- En la sección de Security Settings, puede ignorar el mensaje y dar click en Next: Configure Security Groups.
- En la sección de Configure Security Group, seleccione la opción de escoger un security group existente. En este sentido, escoja el web security group definido anteriormente para permitir el tráfico entrante de peticiones web. Click en Next: Configure Routing.
- En la sección de Configure Routing, se procede a configurar hacia donde enviar las peticiones que llegan al balanceador de cargas. Aquí vamos a crear un Target group que será usado por el servicio de autoscaling.
  - Name: TG-MyWebApp
  - Click en Next: Register Targets.
- El servicio de autoscaling de manera automática registra las instancias como targets. Esto lo haremos posteriormente.
- Click en Next: Review. Verifique los parámetros configurados.
- Click en Create.
 


 
 
### Creacion y configuracion del servicio de auto scaling

Un grupo de Auto Scaling está asociado con una configuración de lanzamiento a la vez y no puede modificar una configuración de lanzamiento después de haberla creado. Para cambiar la configuración de lanzamiento de un grupo de Auto Scaling, utilice una configuración de lanzamiento existente como base para una nueva configuración de lanzamiento. Luego, actualice el grupo de Auto Scaling para usar la nueva configuración de lanzamiento. Cambio de la configuración de lanzamiento para un grupo de Auto Scaling - Amazon EC2 Auto Scaling

En esta sección se procede a configurar un grupo de Auto Scaling así como la configuración de lanzamiento (Launch Configuration). Esta configuración de lanzamiento es un template que utiliza el grupo de Auto Scaling para instanciar las máquinas EC2. Cuándo se crea una configuración de lanzamiento, se hace necesario especificar la información de la instancia tal como: AMI, tipo de instancia, key pair, security groups y disco.

En el home de la consola de administración, seleccione el servicio de EC2.

- En el panel izquierdo, seleccione **Launch configurations.**
- Click en Create launch configuration.
- Proceda a configurar los siguientes aspectos:
  - Launch configuration name: MyWebbApp
  - Amazon machine image: Seleccione el nombre de la AMI creada: Web Server – AMI.
  - Instance type: Click en Choose instance type.
    - Buscar y seleccionar t2.micro
    - Seleccione Choose.
  - Additional Configuration:
    - Monitoring: Seleccione la casilla para Enable EC2 instance detailed monitoring within Cloud Watch. Esto va a permitir al Auto Scaling reaccionar rápidamente a cambios en la utilización.
  - En Security Groups, se configurará utilizando el security group que realmente ha sido creado para este laboratorio.
    - Seleccione Select an existing security group.
    - Escoja Web Security group.
  - En la sección de Key Pair:
    - Key pair options: Choose an existing key pair.
    - Existing key pair: la definida por ud para el curso.
    - Marque la casilla de I acknowledge…
    - Click en Create launch configuration.

Ahora se procederá a crear el grupo de Auto Scaling. Para esto:

- Seleccione el launch configuration creado.
- Click en el menú Actions. Click en Create auto scaling group.
- Digite el nombre del auto scaling group:
  - Name: MyWebApp-Auto Scaling Group
- Click en Next.
- En la sección de Network configure los siguientes parámetros:
  - Network: MyWebApp-VPC.
  - Subnet: Seleccione las dos subredes privadas ubicadas en las dos zonas de disponibilidad.
  - Click en Next.
- En la sección de Configure advanced options:
  - Seleccione la casilla para: Attach to an existing load balancer.
  - Escoja: Choose from your load balancer target groups
  - Escoja el target group que se creó para la aplicación. TG-MyWebApp.
  - Marque la casilla de Enable group metrics collection within CloudWatch.
  - Click en Next.
- En la sección de Configure Group Size
  - Desired capacity: 2
  - Minimum capacity: 2
  - Maximum capacity: 3
  - Esta configuración va a permitir escalar entre dos y tres máquinas.
- En esta misma sección para scaling policies:
  - Seleccione target tracking scaling policy.
  - Scaling policy name: MyWebApp-ScalingPolicy
  - Metric type: Average CPU utilization.
  - Target Value: 60.

Esta configuración le va indicar al servicio de auto scaling mantener en promedio una utilización de CPU de las instancias del 60%. De esta forma, el servicio de auto scaling automáticamente adicionará o quitará capacidad tanto como sea requerido con el fin de mantener la métrica seleccionada lo más cercana posible al umbral definido. Click en Next.

- Click en Next. Add notifications.
- Click en Next. Add tag.
  - Key: Name
  - Value: Web Instance
- Click. Next.
- Revise la información. Click en Create Auto Scaling Group.
