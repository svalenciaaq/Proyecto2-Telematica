## Analisis y Diseño

Para la VPC Se crean dos instancias en las dos zonas, cada una con una red privada, una publica, donde exiten una base de datos primaria y una base de datos secundaria que estan en la subred privada.

De esta manera se garantiza la alta disponibilidad en el despliegue, pues se van a implementar dos zonas de disponibilidad al igual que servicios de balanceo de cargas y, de auto escalamiento.


<img src="/Documentacion/Screens/Arquitectural Design.png" alt="img"/>

