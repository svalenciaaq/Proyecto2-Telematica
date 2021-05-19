# **Proyecto2-Telematica**


## **Tabla de contenidos**

- [**Proyecto2-Telematica**](#proyecto2-telematica)
  - [**Tabla de contenidos**](#tabla-de-contenidos)
  - [**Introduccion**](#introduccion)
  - [**Descripcion**](#descripcion)
    - [**Descripcion de los servicios a utilizar**](#descripcion-de-los-servicios-a-utilizar)
      - [**AWS EC2**](#aws-ec2)
      - [**Elestic load balancers**](#elestic-load-balancers)
      - [**Auto Scaling**](#auto-scaling)
      - [**EFS**](#efs)
      - [**RDS**](#rds)
      - [**Cloudflare**](#cloudflare)
      - [**LetsEncrypt**](#letsencrypt)
      - [**Wordpress**](#wordpress)
  - [**Analisis y Diseño**](#analisis-y-diseño)
  - [**Proceso de instalacion**](#proceso-de-instalacion)
    - [**Documentacion del DNS**](#documentacion-del-dns)
    - [**Documentacion de certificados de seguridad**](#documentacion-de-certificados-de-seguridad)
    - [**Documentacion de los servicios utilizados AWS/otros y el proceso de instalacion monolitica individual en AWS**](#documentacion-de-los-servicios-utilizados-awsotros-y-el-proceso-de-instalacion-monolitica-individual-en-aws)
    - [**Documentación de los servicios utilizados AWS/otros y el proceso de instalación en AWS**](#documentación-de-los-servicios-utilizados-awsotros-y-el-proceso-de-instalación-en-aws)
    - [**Analisis de costos de la solucion. Inversion inicial y mensual**](#analisis-de-costos-de-la-solucion-inversion-inicial-y-mensual)


## **Introduccion**

Proyecto para el desarollo de un sistema de software robusto, escalable, con alta disponibilidad, buen rendimiento y con los requerimientos de seguridad

## **Descripcion**

En este documento se especificara todas las pautas necesarias para el dasarollo de un sistema robusta,escalable, con alta disponibilidad, buen rendimiento y con los requerimientos de seguridad basado en los diferentes servicios que son proporcionados por aws educate.


### **Descripcion de los servicios a utilizar**

#### **AWS EC2**

Amazon Elastic Compute Cloud (Amazon EC2) proporciona capacidad de computación escalable en la nube de Amazon Web Services (AWS). El uso de Amazon EC2 elimina la necesidad de invertir inicialmente en hardware, de manera que puede desarrollar e implementar aplicaciones en menos tiempo. Puede usar Amazon EC2 para lanzar tantos servidores virtuales como necesite, configurar la seguridad y las redes y administrar el almacenamiento. Amazon EC2 le permite escalar hacia arriba o hacia abajo para controlar cambios en los requisitos o picos de popularidad, con lo que se reduce la necesidad de prever el tráfico.


#### **Elestic load balancers**

Elastic Load Balancing distribuye automáticamente el tráfico de aplicaciones entrante a través de varios destinos, tales como las instancias de Amazon EC2, los contenedores, las direcciones IP, las funciones Lambda y los dispositivos virtuales. Puede controlar la carga variable del tráfico de su aplicación en una única zona o en varias zonas de disponibilidad. Elastic Load Balancing ofrece cuatro tipos de balanceadores de carga que cuentan con el nivel necesario de alta disponibilidad, escalabilidad automática y seguridad para que sus aplicaciones sean tolerantes a errores.


#### **Auto Scaling**

Auto Scaling es un servicio de AWS (Amazon Web Services) que te permite lanzar o terminar instancias con el objetivo de tener las necesarias para poder controlar la carga de tu aplicación


#### **EFS**

Amazon Elastic File System (Amazon EFS) ofrece un sistema de archivos elástico, sencillo, sin servidor, con posibilidad de establecer y olvidar y que permite compartir datos de archivos sin necesidad de aprovisionar o administrar el almacenamiento. Se puede utilizar con los servicios en la nube de AWS y con los recursos en las instalaciones y está diseñado para escalar bajo demanda a petabytes sin interrumpir las aplicaciones. Con Amazon EFS puede incrementar o reducir los sistemas de archivos automáticamente a medida que agrega y quita archivos, lo que elimina la necesidad de aprovisionar y administrar la capacidad para dar lugar al crecimiento.



#### **RDS**

Con Amazon Relational Database Service (Amazon RDS), es sencillo configurar, utilizar y escalar una base de datos relacional en la nube. El servicio suministra capacidad rentable y escalable al mismo tiempo que automatiza las arduas tareas administrativas, como el aprovisionamiento de hardware, la configuración de bases de datos, la implementación de parches y la creación de copias de seguridad. Lo libera de estas tareas para que pueda concentrarse en sus aplicaciones y darles el rendimiento rápido, la alta disponibilidad, la seguridad y la compatibilidad que necesitan.



#### **Cloudflare** 

Cloudflare está diseñado para una configuración fácil. Cualquier persona con un sitio web y su propio dominio puede usar Cloudflare independientemente de la plataforma que elija. Cloudflare no requiere hardware, software o cambios adicionales en tu código.

#### **LetsEncrypt**

Let’s Encrypt es una autoridad de certificación (AC, o CA por sus siglas en inglés) gratuita, automatizada, y abierta, que existe para el beneficio del público. Es un servicio provisto por el Internet Security Research Group (ISRG).

Le damos gratuitamente a las personas los certificados digitales que necesitan para poder habilitar HTTPS (SSL/TLS) en sitios web, de la forma más amigable al usuario posible. Hacemos esto porque queremos crear una web más segura y respetuosa de la privacidad.


#### **Wordpress**

WordPress es un sistema de gestión de contenidos (CMS) que permite crear y mantener un blog u otro tipo de web.

## **Analisis y Diseño**

[Documentacion del analisis y diseño](https://github.com/svalenciaaq/Proyecto2-Telematica/blob/main/Documentacion/AnalisisDiseño.md)


## **Proceso de instalacion**

### **Documentacion del DNS**


[Documentacion del DNS](https://github.com/svalenciaaq/Proyecto2-Telematica/blob/main/Documentacion/DNS.md)


### **Documentacion de certificados de seguridad**

[Documentacion del certificado de seguridad](https://github.com/svalenciaaq/Proyecto2-Telematica/blob/main/Documentacion/Certificado.md)

### **Documentacion de los servicios utilizados AWS/otros y el proceso de instalacion monolitica individual en AWS**

[Documentacion de la monolitica](https://github.com/svalenciaaq/Proyecto2-Telematica/blob/main/Documentacion/Monolitica.md)


###  **Documentación de los servicios utilizados AWS/otros y el proceso de instalación en AWS**

[Documentacion de la escalable](https://github.com/svalenciaaq/Proyecto2-Telematica/blob/main/Documentacion/Escalable.md)

### **Analisis de costos de la solucion. Inversion inicial y mensual**

[Documentacion de los costos](https://github.com/svalenciaaq/Proyecto2-Telematica/blob/main/Documentacion/Costos.md)