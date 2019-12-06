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

La topología de la siguiente imagen se configura EIGRP para IPv6.

![alt text](https://github.com/brahianf/EIGRP/blob/IPv6/topologiaRed.PNG)

### Tabla de direccionamiento

Los routers en la topología tienen una configuración inicial que incluye las direcciones 
de las interfaces y PCs, además del Router ISP y el server con enrutamiento estatico en Router2. 
En este momento,se configura routing dinámico EIGRP para IPv6 en R0,R1,R2,R3.

![alt text](https://github.com/brahianf/EIGRP/blob/IPv6/tablaDireccionamiento.PNG)

### Comandos CLI

	*** R0
	
	```
	R0>enable
	R0#configure terminal
	
	R0(config)#ipv6 router eigrp 100
	R0(config-rtr)#eigrp router-id 1.0.0.0
	R0(config-rtr)#passive-interface fastEthernet 0/0
	R0(config-rtr)#redistribute static 
	R0(config-rtr)#no shutdown 
	R0(config-rtr)#exit
	
	R0(config)#interface fastEthernet 0/0
	R0(config-if)#ipv6 address fe80::00 link-local 
	R0(config-if)#ipv6 eigrp 100
	R0(config-if)#exit
	
	R0(config)#interface fastEthernet 0/1
	R0(config-if)#ipv6 address fe80::00 link-local 
	R0(config-if)#ipv6 eigrp 100
	R0(config-if)#exit
	
	R0(config)#interface serial 0/0/0
	R0(config-if)#ipv6 address fe80::00 link-local 
	R0(config-if)#ipv6 eigrp 100
	R0(config-if)#exit

	R0(config)#interface serial 0/0/1
	R0(config-if)#ipv6 address fe80::00 link-local 
	R0(config-if)#ipv6 eigrp 100
	R0(config-if)#exit
	
	
	
	```
	
	  ** R1 **
	 
	```
	R1>enable 
	R1(config)#ipv6 router eigrp 100
	R1(config-rtr)#eigrp router-id 1.0.0.1
	R1(config-rtr)#passive-interface fastEthernet 0/0
	R1(config-rtr)#exit
	
	R0(config)#interface fastEthernet 0/0
	R0(config-if)#ipv6 address fe80::01 link-local 
	R0(config-if)#ipv6 eigrp 100
	R0(config-if)#exit
	
	R0(config)#interface fastEthernet 0/1
	R0(config-if)#ipv6 address fe80::01 link-local 
	R0(config-if)#ipv6 eigrp 100
	R0(config-if)#exit
	
	R0(config)#interface serial 0/0/0
	R0(config-if)#ipv6 address fe80::01 link-local 
	R0(config-if)#ipv6 eigrp 100
	R0(config-if)#exit

	
	```
		
	  ** R2 **
	
	```
	R2>enable 
	R2(config)#ipv6 router eigrp 100
	R2(config-rtr)#eigrp router-id 1.0.0.2
	R2(config-rtr)#passive-interface fastEthernet 0/0
	R2(config-rtr)#exit
	
	R2(config)#interface fastEthernet 0/0
	R2(config-if)#ipv6 address fe80::02 link-local 
	R2(config-if)#ipv6 eigrp 100
	R2(config-if)#exit
	
	R2(config)#interface fastEthernet 0/1
	R2(config-if)#ipv6 address fe80::02 link-local 
	R2(config-if)#ipv6 eigrp 100
	R2(config-if)#exit
	
	R2(config)#interface serial 0/0/0
	R2(config-if)#ipv6 address fe80::02 link-local 
	R2(config-if)#ipv6 eigrp 100
	R2(config-if)#exit

	
	```
		
	  ** R3 **
	 
	```
	R3>enable 
	R3(config)#ipv6 router eigrp 100
	R3(config-rtr)#eigrp router-id 1.0.0.3
	R3(config-rtr)#passive-interface fastEthernet 0/0
	R3(config-rtr)#exit
	
	R3(config)#interface fastEthernet 0/0
	R3(config-if)#ipv6 address fe80::03 link-local 
	R3(config-if)#ipv6 eigrp 100
	R3(config-if)#exit
	
	R3(config)#interface fastEthernet 0/1
	R3(config-if)#ipv6 address fe80::03link-local 
	R3(config-if)#ipv6 eigrp 100
	R3(config-if)#exit
	
	R3(config)#interface serial 0/0/0
	R3(config-if)#ipv6 address fe80::03 link-local 
	R3(config-if)#ipv6 eigrp 100
	R3(config-if)#exit

	
	```
	
	
### EIGRP-IPv4.pkt

https://github.com/brahianf/EIGRP/blob/IPv6/EIGRP-IPv6.pkt?raw=true
