# Configuración de Nodo IOTA Hornet en Ubuntu Server
El presente manual contempla la creación de un nodo IOTA Hornet desde una instalación limpia en Ubuntu Server 20.04_x64.
Esta basado en los siguientes manuales en ingles:
- [Expose your local device to the Internet](https://docs.iota.org/docs/general/0.1/how-to-guides/expose-your-local-device)
- [Instalar los scripts de actualización DNS](https://www.duckdns.org/install.jsp)
- [Make Godaddy a Dynamic DNS Provider](https://forum.htpcguides.com/Thread-Linux-Make-godaddy-com-a-dynamic-dns-provider)
- [Hornet Playbook](https://github.com/nuriel77/hornet-playbook)

## 1. Prerequisitos
- RAM: Por lo menos 1.5 GB de RAM
- x2 CPU
- [Configurar un servidor Linux en una máquina virtual](https://docs.iota.org/docs/general/0.1/how-to-guides/set-up-virtual-machine)
- Configurar una dirección IP Statica para el servidor
- Registrar un DNS dinámico
- Configurar el reenvío de puertos desde el router local.

## 2. Configurar dirección IP Statica y actualizar servidor
Revisar la dirección gateway.
```
ip link show
```
Verificar la dirección IP
```
ip addr
```
Modificar la dirrección IP
```
sudo nano /etc/netplan/<file-that-ends-in.yaml>.yaml`
```
El archivo debe verse de la siguiente forma:

```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      addresses:
        - 10.0.10.11/16
      gateway4: 10.0.9.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```
Luego, hay que aplicar los cambios de red.
```
sudo netplan apply
ip add
```
Y probar las configuraciones
```
Ping 8.8.8.8
```
Actualizar el servidor
```
sudo apt update
```
## 3. Registrar el DNS dinámico 
Puedes utlizar DuckGo Dynamic DNS gratuito, o un servicio de DNS Dinámico desde tu dominio. Por ejemplo, Godaddy.
- Opción A: [Registrar un DuckGo Dynamic DNS](https://www.duckdns.org/) y [Configurar script](https://www.duckdns.org/install.jsp)
- Opción B: [Registrar un "A Record" en Goddady](https://ca.godaddy.com/help/add-an-a-record-19238) y [GoDaddy Dynamic DNS](https://github.com/Agro-iot/Plataforma-y-visualizacion/blob/main/IOTA/IOTA%20Hornet%20en%20Ubuntu%20Server.md)

## 4. Configurar Port Forwarding en el Router
Ingresar como administrador al router para luego agregar las siguientes entradas en la configuración Port Forwarding en el Firewall del router
- TCP 10.0.10.11:14265 
- TPC 10.0.10.11:15600
- TCP 10.0.10.11:14267
- UDP 10.0.10.11:14626

Probar si la configuración es correcta [yougetsignal.com](https://www.yougetsignal.com/tools/open-ports/)

## 5. Instalación del Nodo IOTA Hornet (Playbook)
Nota: El IOTA Hornet Playbook es una versión alternativa creada por nuriel77. La instalación oficial puede ser encontrada en [https://docs.iota.org](https://docs.iota.org/docs/hornet/1.1/tutorials/install-hornet).

Iniciar sessión como root
```
sudo su
```
Ejecutar el instalador. Tiempo aproximado 30 min.
```
sudo bash -c "bash <(curl -s https://raw.githubusercontent.com/nuriel77/hornet-playbook/master/fullnode_install.sh)"
```
### a. Opciones de instalación
Por defecto escoger y confirmar:
- INSTALL_DOCKER
- INSTALL_NGINX
- ENABLE_HAPROXY

### b. Elegir nombre de administrador
Nombre y clave a discreción.

### d. Revisar las imagenes Docker
```
docker images
```
### d. Crear script de actualizaciòn DNS

### e. Iniciar el nodo hornet
```
systemctl start hornet
```
## 6. Configurar el nodo
Ejecutar
```
sudo horc
```
Seleccionar la opcion n) Neighbors y agregar a los nodos vecinos:
- Puedes solicitar neighbors desde el canal **#nodesharing** del [discord de IOTA](discord.iota.org). 
- Para esto tienes que compartir tu dirección web; por ejemplo tcp://my-node.io:15600.
- Puedes comenzar agregando el nodo **ayni.burgeons.ca:15600**

# 7. Revisar la sincronización del Nodo
Visitar
- https://<DIRECCION_IP_LOCAL>:8081
- https://<DIRECCION_IP_LOCAL>:5555


