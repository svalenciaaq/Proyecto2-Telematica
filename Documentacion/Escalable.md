
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
      - [instalacion y configuracion de la istancia wordpress](#instalacion-y-configuracion-de-la-istancia-wordpress)
    - [Creacion de la AMi para el servicio de auto scaling](#creacion-de-la-ami-para-el-servicio-de-auto-scaling)
    - [Creacion y configuracion del balanceador de cargar](#creacion-y-configuracion-del-balanceador-de-cargar)
    - [Creacion y configuracion del servidio de auto scaling](#creacion-y-configuracion-del-servidio-de-auto-scaling)




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

 

- Tabla de enrutamiento principal: la tabla de enrutamiento que viene de forma automática con la VPC. Controla el direccionamiento de todas las subredes que no están explícitamente asociadas a ninguna otra tabla de ruteo. 

- Tabla de enrutamiento personalizada: una tabla de enrutamiento que se crea para la VPC. 

- Asociación de borde: una tabla de enrutamiento que se utiliza para enrutar el tráfico de VPC entrante a un dispositivo. Asocie una tabla de ruteo a la gateway de Internet o a la gateway privada virtual y especifique la interfaz de red del dispositivo como objetivo para el tráfico de la VPC. 

- Asociación de tabla de enrutamiento: la asociación entre una tabla de enrutamiento y una subred, gateway de Internet o gateway privada virtual. 

- Tabla de enrutamiento de subred: una tabla de enrutamiento asociada con una subred. 

- Tabla de enrutamiento de gateway: una tabla de enrutamiento asociada con una gateway de Internet o gateway privada virtual. 

- Tabla de enrutamiento de gateway local: una tabla de enrutamiento asociada con una gateway local de Outposts. Para obtener información acerca de las gateways locales, consulte Gateways locales en la Guía del usuario de AWS Outposts. 

- Destino: el intervalo de direcciones IP a las que desea que vaya el tráfico (CIDR de destino). Por ejemplo, una red corporativa externa con un CIDR 172.16.0.0/12. 

- Propagación: la propagación de rutas permite que una gateway privada virtual propague rutas automáticamente a las tablas de enrutamiento. Esto significa que no es necesario introducir manualmente rutas de VPN en las tablas de ruteo. Para obtener más información acerca de las opciones de enrutamiento de VPN, consulte Opciones de enrutamiento de la VPN de sitio a sitio en la Guía del usuario de la VPN de sitio a sitio. 

- Destino: la gateway, interfaz de red o conexión a través de la cual enviar el tráfico de destino, por ejemplo, una gateway de Internet. 

- Ruta local: una ruta predeterminada para la comunicación dentro de la VPC. 

Para este proyecto se usará la tabla de enrutamiento personalizada. 

De forma predeterminada, cada tabla de ruta personalizada que creemos ya tendrá la ruta local dentro, lo que permitirá que la comunicación fluya entre todos los recursos y subredes dentro de la VPC. Cuando utilizamos el asistente de la VPC en la consola para crear una VPC con una gateway de Internet, el asistente crea una tabla de ruteo personalizada y agrega una ruta a la gateway de Internet. Una forma de proteger la VPC es dejar la tabla de ruteo principal en su estado predeterminado original. Después, asociar de forma explícita cada nueva subred que creemos a una de las tablas de ruteo personalizadas que hayamos creado. De este modo, se asegurará de que controlamos de manera explícita el modo en que cada subred direcciona el tráfico. 

De este modo, podremos añadir, quitar y modificar rutas en la tabla de ruteo personalizada.  

Si bien la tabla de enrutamiento principal controla el enrutamiento de nuestra VPC, se quiere ser más detallado sobre cómo enrutamos el tráfico para subredes específicas. La aplicación consta de una interfaz y una base de datos. Crearemos subredes independientes para estos recursos y se proporcionara diferentes rutas para cada uno de ellos. Al asociar una tabla de enrutamiento personalizada con una subred, la subred la usará en lugar de la tabla de enrutamiento principal.  

 

Pasos para crear y configurar la tabla de enrutamiento 

Cada subred de una VPC debe estar asociada a una tabla de ruteo. Por ende se deben seguir los siguientes pasos 

 

Abrimos la consola y nos dirijimos a la configuración de vpc 

	 

Nos vamos a la opción route tables en la izquierda  

	 

Creamos una nueva ruta  

	 

Ponemos un nombre y seleccionamos nuestra VPC 

	 

Este proceso lo repetiremos 4 veces poniendo los siguientes nombres  

 

Private Route Table A 

Private Route Table B 

Public Route Table A 

Public Route Table A 

 

Asociar subredes a las tablas  

Ahora asociaremos las tablas a cada subred creada en el numeral anterior.  

Ahora se debe crear las rutas por defecto. Para el caso de la subred privada, el tráfico debe ser enviado al NAT Gateway. Para esto en el panel izquierdo, seleccione la opción de Route Tables. Seleccione la subred privada “Private Route Table” y abajo, en el panel de “Routes” 

Click en “Edit routes”. 

Click en “Add route”. 

Destination: 0.0.0.0/0 

Target: Seleccione la instancia del NAT Gateway (NAT-GW-Instance) que se desplegó para cada AZ. 

Click en “Save routes”. 

Debe observar el mensaje de que la ruta se ha editado de manera exitosa. 

 
 

Por otro lado, para el caso de la red pública, este tráfico debe ser enviado al Internet Gateway. Para esto en el panel izquierdo, seleccione la opción de Route Tables. Seleccione la subred pública “Public Route Table” y abajo, en el panel de “Routes” 

Click en “Edit routes”. 

Click en “Add route”. 

Destination: 0.0.0.0/0 

Target: Seleccione el Internet Gateway (MyWebApp-IGW). 

Click en “Save routes”. 

Debe observar el mensaje de que la ruta se ha editado de manera exitosa. 

 
 

Ahora debemos asociar a cada tabla de enrutamiento, las subredes. Seleccione la “Private Route Table A” y en la pestaña de “Subnet Associations”: 

Click en “Edit subnet associations”. 

Marque la casilla de la subred privada RR.RR.1.0/24. 

Click en Save. 

 
 

Ahora debemos asociar a cada tabla de enrutamiento, las subredes. Seleccione la “Private Route Table B” y en la pestaña de “Subnet Associations”: 

Click en “Edit subnet associations”. 

Marque la casilla de la subred privada RR.RR.3.0/24. 

Click en Save. 

 
 

Para el caso de las subredes públicas, seleccione la “Public Route Table A” y en la pestaña de “Subnet Associations”: 

Click en “Edit subnet associations”. 

Marque la casilla de la subred privada RR.RR.2.0/24. 

Click en Save. 

 
 

Ahora, seleccione la “Public Route Table B” y en la pestaña de “Subnet Associations”: 

Click en “Edit subnet associations”. 

Marque la casilla de la subred privada RR.RR.4.0/24. 

Click en Save. 

### Creacion y configuracion de los bastion hosts


### Creacion de los security gropu para trafico web y para la base de datos y


### Creacion y configuracion del servidor de bases de datos en la subred privada

### Creacion y configuracion el sistema de archivos compartidos EFS

### Creacion y configurar la instancia del servidor web wordpress

#### instalacion y configuracion de la istancia wordpress

### Creacion de la AMi para el servicio de auto scaling


### Creacion y configuracion del balanceador de cargar

### Creacion y configuracion del servidio de auto scaling