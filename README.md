# EIGRP

### Enhanced Interior Gateway Routing Protocol

Configuración básica del Protocolo mejorado de routing de gateway interior
en un router con IOS de Cisco como solución de enrutamiento para redes basadas en el 
protocolo IPv4, IPv6 para escalar y para incluir varias topologías y proporcionar 
tiempos de convergencia extremadamente rápidos con un mínimo tráfico de red.

Introducción

Cisco desarrollo el protocolo de routing por vector distancia mejorado EIGRP donde 
la información acerca del resto de la red se obtiene a partir de routers vecinos
conectados directamente, donde además se incluye caracteristicas que se encuentran
en los protocolos de routing de estado de enlace.

Características del protocolo:

* DUAL (Diffusin Update Algorithm) es el algoritmo de actualizacion por difusión
garantizando las rutas de respaldo y sin bucles, además almacena todas las rutas
de respaldo disponibles a destinos, adaptandose a rutas alternativas.

* Con los routers conectados directamente y habilitados para EIGRP se establecen
las relaciones de adyacencias de vecinos y sirven para rastrear el estado de esos
routers.

*  RTP (Reliable Transport Protocol, protocolo de transporte confiable), encargado
de la entrega de paquetes EIGRP a los vecinos.
RTP puede enviar paquetes EIGRP como unidifusión o multidifusión.
Los paquetes de multidifusión EIGRP para IPv4 utilizan la dirección IPv4 
de multidifusión reservada 224.0.0.10.
Los paquetes de multidifusión EIGRP para IPv6 se envían a la dirección IPv6
de multidifusión reservada FF02::A.

* Para minimizar el ancho de banda que se requiere para las actualizaciones de EIGRP,
el protocolo no envia actualizaciones periodicas, como la actualizacion parcial donde
la actualizacion solo incluye informacion de cambios de ruta, como un nuevo enlace
o un enlace no disponible. La actualizacion limitada propaga las actualizaciones
parciales que se envvian solo a los routers a que los cambios afecten.

* Equilibio de carga de mismo consto u con distinto costo, permitiento a administradores
distribuir el flujo de trafico en las redes.

* Autenticación de la información de routing que se transmite asegura de que los routers 
solo acepten información de routing de otros routers que se configuraron con la misma 
contraseña o información de autenticación.
La autenticación no cifra las actualizaciones de routing EIGRP.

###	Enrutamiento dinámico con EIGRP

### Topología de red

La topología de la siguiente imagen se configura EIGRP para IPv4.

![alt text](https://github.com/brahianf/EIGRP/blob/master/topologiaRed.PNG)

### Tabla de direccionamiento

Los routers en la topología tienen una configuración inicial que incluye las direcciones 
de las interfaces y PCs. En este momento,se configura routing dinámico EIGRP.

![alt text](https://github.com/brahianf/EIGRP/blob/master/tablaDireccionamiento.PNG)

### Comandos CLI

El comando **(config)# router eigrp sistema-autónomo** del modo de configuración global 
se usa para ingresar al modo de configuración del router para EIGRP y comenzar a configurar 
el proceso EIGRP donde el argumento sistema-autónomo se puede asignar a cualquier valor de 16 
bits entre los números 1 y 65 535. Todos los routers dentro del dominio de routing EIGRP 
deben usar el mismo número de sistema autónomo,en este caso 100.

La ID de router EIGRP se utiliza para identificar de forma única a cada router en el dominio 
de routing EIGRP. La ID del router debe ser un número único de 32 bits en el dominio de 
routing EIGRP; de lo contrario, pueden ocurrir incongruencias de routing.

Para habilitar el routing EIGRP en una interfaz, utilice el comando **network ipv4-network-address** 
del modo de configuración del router. 

El comando network tiene la misma función que en todos los protocolos de routing IGP. Con el 
comando network en EIGRP:

* Se habilita cualquier interfaz en el router que coincida con la dirección de red en el comando 
  network del modo de configuración del router para enviar y recibir actualizaciones de EIGRP.
* Se incluye la red de las interfaces en las actualizaciones de routing EIGRP.


	* R0
	
	```
	R0>enable
	R0#configure terminal
	R0(config)#router eigrp 100
	R0(config-router)#eigrp router-id 1.0.0.0 
	R0(config-router)#network 10.0.0.0
	R0(config-router)#network 172.16.0.0
	R0(config-router)#network 172.17.0.0
	R0(config-router)#passive-interface fastEthernet 0/0
	```

		* R1
	
	```
	R1>enable 
	R1#configure terminal 
	R1(config)#router eigrp 100
	R1(config-router)#eigrp router-id 1.0.0.1
	R1(config-router)#network 192.168.1.0
	R1(config-router)#network 172.16.0.0 
	R1(config-router)#network 172.18.0.0
	R1(config-router)#passive-interface fastEthernet 0/0	
	```
	
		* R2
	
	```
	Router>enable 
	Router#configure terminal 
	R2(config)#router eigrp 100
	R2(config-router)#eigrp router-id 1.0.0.2
	R2(config-router)#network 192.168.2.0
	R2(config-router)#network 172.18.0.0
	R2(config-router)#network 172.19.0.0
	R2(config-router)#passive-interface fastEthernet 0/0
	```
	
		* R3
	
	```
	Router>enable 
	Router#configure terminal 
	R3(config)#router eigrp 100
	R3(config-router)#eigrp router-id 1.0.0.3
	R3(config-router)#network 20.0.0.0
	R3(config-router)#network 172.17.0.0
	R3(config-router)#network 172.19.0.0
	R3(config-router)#passive-interface fastEthernet 0/0
	```

Tan pronto como se habilita una nueva interfaz dentro de la red EIGRP, EIGRP intenta 
formar una adyacencia de vecino con cualquier router vecino para enviar y recibir 
actualizaciones de EIGRP.

Cada tanto puede ser necesario, o ventajoso, incluir una red conectada directamente
en la actualización de routing EIGRP, pero no permitir que se forme ninguna adyacencia 
de vecino fuera de esa interfaz. El comando passive-interface se puede utilizar para 
evitar que se formen adyacencias de vecino. Existen dos razones principales para habilitar 
el comando passive-interface:

* Para suprimir tráfico de actualización innecesario, por ejemplo, cuando una interfaz 
 es una interfaz LAN, sin otros routers conectados
* Para aumentar los controles de seguridad, por ejemplo, para evitar que dispositivos
 desconocidos de routing no autorizados reciban actualizaciones de EIGRP

### EIGRP-IPv4.pkt
	
https://github.com/brahianf/EIGRP/blob/master/EIGRP-IPv4.pkt?raw=true
