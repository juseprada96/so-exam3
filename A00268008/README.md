### Examen 3
**Universidad ICESI**  
**Curso:** Sistemas Operativos  
**Estudiante:** Juan Sebastián Prada.  
**Tema:** Descubrimiento de servicios, Microservicios  
**Correo:** juan.prada1 at correo.icesi.edu.co

### Objetivos
* Implementar servicios web que puedan ser consumidos por usuarios o aplicaciones
* Conocer y emplear tecnologías de descubrimiento de servicio

### Prerrequisitos
* Virtualbox o WMWare
* Máquina virtual con sistema operativo CentOS7
* Framework consul, zookeper o etcd

### Descripción
El tercer parcial del curso sistemas operativos trata sobre la creación de servicios web y el uso de tecnologías para el descubrimiento de servicio

### Actividades

1. Despliegue un esquema como el mostrado en la **figura 1**. Empleen un servicio web de su preferencia (puede usar alguno de los ejemplos de clase). No es necesario incluir los componentes para monitoreo (Elasticsearch, Kibana, Logstash) (30%)

Para desplegar el esquema se utilizarón 4 maquinas virtuales más la maquina host que hizo el requerimiento del navegador que accede a los servicios.

Para el despliegue de las maquinas virtuales de discovery service y las de micorservicios se siguio la guia propuesta en https://github.com/ICESI/so-discovery-service

Muestras de las maquinas corriendo y funcionando.

![][1]

![][2]

![][3]

![][4]

![][5]

![][6]

![][8]

Muestra de la lista de miembros de consul con la replica del servicio.

![][15]


Para el despliegue del balanceador se utilizo nginx y consul template. Para la configuración del consul template se uso la configuración que se hace en la guia del link aterior.
El template para nginx fue tomado de https://github.com/hashicorp/consul-template/blob/master/examples/nginx.md.

Durante el desarrollo del parcial se presento un error de permiso con nginx y se encontro la siguiente solución:

´´
yum install -y policycoreutils-devel
grep nginx /var/log/audit/audit.log | audit2allow -M nginx
semodule -i nginx.pp
´´

Muestra de funcionamiento de nginx, archivo de template, archivo de configuración generado.

![][9]

![][10]

![][11]


Muestra de funcionamiento del discovery service, y balanceador de carga enviando las peticiones a los dos servidores.

![][12]

![][13]

![][14]




3. Describa los cambios o adiciones necesarias en el diagrama de la **figura 1** para adicionar un microservicio diferente al ya desplegado en el ambiente, tenga en cuenta los siguientes conceptos en su descripción: API Gateway, paradigma reactivo, load balancer, protocolo publicador/suscriptor (interconexión de microservicios) (20%)

Los cambios necesarios para agregar un nuevo microservicio diferente al ya despeglado son:

Primero se tiene que adicionar el servidor contenedor del microservicio nuevo.
El contenedor del microservicio nuevo sería identico al del microservicio que ya esta desplegado.
Como el dicovery service hace uso de un protocolo de publicador/suscriptor es necesario suscribirse al servidor
de discovery service, lo cual involucra un enlace entre el servidor de discoverty service y el nuevo contenedor con el microservicio.
El balanceador de carga, que se encarga de mantener la carga balanceada entre los diferentes servidores que ofrecen los diferentes microservicios, sería ofrecido por el componente ya desplegado gracias al paradigma reactivo, al modificar los servicios ofrecidos por el discovery service el balanceador de carga ahora podrá ofrecer el servicio nuevo.

El api gateway es el api que me permite hacer el mapeo de los microservicios desplegados y como serán usados, es decir es una capa de abstracción para que el uso de una funcionalidad comlpeta ofrecida por varios microservicios sea mucho más fácil.

[1]: images/Screenshot_1.png
[2]: images/Screenshot_2.png
[3]: images/Screenshot_3.png
[4]: images/Screenshot_4.png
[5]: images/Screenshot_5.png
[6]: images/Screenshot_6.png
[7]: images/Screenshot_7.png
[8]: images/Screenshot_8.png
[9]: images/Screenshot_9.png
[10]: images/Screenshot_10.png
[11]: images/Screenshot_11.png
[12]: images/Screenshot_12.png
[13]: images/Screenshot_13.png
[14]: images/Screenshot_16.png
[15]: images/Screenshot_17.png



### Referencias
https://github.com/ICESI/so-microservices-python  
http://microservices.io/patterns/microservices.html
https://github.com/hashicorp/consul-template/blob/master/examples/nginx.md.
https://github.com/ICESI/so-discovery-service
https://stackoverflow.com/questions/25774999/nginx-stat-failed-13-permission-denied
