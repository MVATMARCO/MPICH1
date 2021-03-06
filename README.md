# MPICH-Ejemplos
Usar MPICH

- La URL de base para obtener información de mpich es: https://www.mpich.org/
- Para instalar mpich remítase a: https://www.mpich.org/static/downloads/3.4/mpich-3.4-installguide.pdf
- Para usar mpich remítase a: https://www.mpich.org/static/downloads/3.4/mpich-3.4-userguide.pdf

# Rápidamente...

>El camino más rápido posible para ejecutar en un solo nodo es mediante Docker: https://hub.docker.com/r/nlknguyen/alpine-mpich/

La siguiente guía asume que tiene al menos un par de equipos, físicos o virtuales, Ubuntu 20+ server interconectados (pueden al menos hacer ping entre ellos) y sin restricciones de firewalls (ej: `sudo ufw status` --> Status: inactive). Adicionalmente en todos los equipos debe tener un usuario con el mismo nombre, con el que va a estar conectado y trabajar (puede llamarse 'mpi', 'pedro', ..., por ejemplo).

## Instalación:

>*En todos los nodos !!!*

`sudo apt install mpich`

podría ser necesario verificar que esté instalado gcc (`sudo apt install gcc`)

Debe tener instalado y activo el servidor ssh

`sudo apt install openssh-server`

`sudo service ssh start`

Es necesario que todos los equipos del clúster conozcan las IPs de los otros. Regístrelas en el /etc/hosts de todos los equipos.

Es necesario que todos los equipos puedan conectarse entre ellos por ssh, sin poner clave de usuario. Revise: https://github.com/daoc/Instrucciones/blob/master/Ubuntu.md#conectarse-por-ssh-sin-password, y luego verifique que puede conectarse de una máquina a la otra, poniendo solo el nombre de la máquina (no el usuario ni la IP): >`ssh maquina1` ó >`ssh maquina2`.

Puede ser útil tener un directorio compartido por todos los equipos (samba, nfs, ...), al cual se acceda en el mismo path.

En dicho directorio incluirá los programas y datos que usará, así como un archivo donde incluirá los nombres de todos los equipos en su clúster; este archivo suele llamarse "machinefile", y tendrá un nombre por línea, opcionalmente seguido por dos puntos y el número de procesos a ejecutarse ahí. Ej:
```
equipo1
equipo2:3
equipo3:2
```
## Ejecución...

Los ejemplos que verá han sido programados en C.

Para compilar: `mpicc mpi_hello.c -o mpi_hello`

Para ejecutar hay varias opciones:

- Un solo proceso en la máquina local:
  - `mpiexec ./mpi_hello`
- Varios procesos en la máquina local:
  - `mpiexec -n 4 ./mpi_hello`
- En clúster, de acuerdo a las definiciones  de machinefile:
  - `mpiexec -f machinefile ./mpi_hello`
- En clúster, de acuerdo a las definiciones de machinefile, indicando el número total de procesos a distribuirse:
  - `mpiexec -n 16 -f machinefile ./mpi_hello`



