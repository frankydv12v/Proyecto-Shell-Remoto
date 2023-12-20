# Proyecto-Shell-Remoto
Proyecto de Sistemas Operativos

### Descripcion

Proyecto Final Sistemas Operativos. Objetivo programar en C, una aplicación cliente/servidor que tiene como cliente al usuario que ejecuta los comandos y el servidor es el computador remoto  donde se ejecutan los comandos.

# Ejecucion de proyecto

### Requisitos

Para hacer funcionar el proyecto debemos contar con Docker y disponer de una terminal Linux. Trabajando en el sistema operativo Windows debemos contar con Windows Subsystem Linux (WSL). 

### Primer Paso

Abre una terminal tipo Linux. 
Hacemos un copia del repositorio escribiendo en la terminal asi:

```
git clone https://github.com/michaelRS2002/Proyecto-Shell-Remoto
```

Abrimos la carpeta y revisamos que se hayan guardado todos los archivos asi

```
cd Proyecto-Shell-Remoto/
```
```
ls
```
### Segundo Paso
Creamos un network donde se van a comunicar los contenedores cliente-servidor asi

```
docker network create workspace
```

Si estamos usando la aplicacion en el mismo computador dividimos la terminal con:

`tmux`

`Ctrl+B` + `Shift+2`

Para ir de un lado a otro de pantalla  `Ctrl+B` + Flecha. Tambien se pueden abrir dos terminales al mismo tiempo si es que los comandos del teclado no te estan funcionando para dividir la terminal.



Creamos 2 contenedores uno  para el servidor y otro para cliente con el siguiente comando cada uno en una parte del terminal 

```
docker container run --name servidor -v $(pwd):/servidor -it --network workspace ubuntu
```
```
docker container run --name cliente -v $(pwd):/cliente -it --network workspace ubuntu
```

## Tercer Paso 
Acceder a la carpeta servidor y la carpeta cliente usa

```
cd servidor
```
```
cd cliente
```

Revisamos si tenemos archivos `ls`

Si es primera vez que lo ejecutamos escribimos `apt update && apt install -y build-essential` en ambas terminales

#### Compilamos
```
gcc -o servidor servidor.c tcp.c leercadena.c
```
```
gcc -o cliente cliente.c tcp.c leercadena.c  
```

[!IMPORTANTE] Ejecutamos siempre de primero el servidor

```
./Servidor 
```
```
./cliente servidor 12348 
```

### SI te haz perdido aqui un resumen

```
git clone https://github.com/michaelRS2002/Proyecto-Shell-Remoto
```
```
cd Proyecto-Shell-Remoto/
```
```
docker network create workspace
```
Ejecutamos siempre de primero el servidor

Servidor: 
```
docker container run --name servidor -v $(pwd):/servidor -it --network workspace ubuntu
```
```
cd servidor
```
```
gcc -o servidor servidor.c tcp.c leercadena.c
```
```
./Servidor 
```
Cliente:
```
docker container run --name cliente -v $(pwd):/cliente -it --network workspace ubuntu
```
```
cd cliente
```
```
gcc -o cliente cliente.c tcp.c leercadena.c  
```
```
./cliente servidor 12348 
```

### Algunos comandos que puedes utilizar:

`Salida` -> Con este comando se cerrara la coneccion entre el cliente y el servidor

`delete + el nombre del archivo` -> Con este comando podras eliminar un comando desde el cliente.

`edit + el nombre del archivo` -> Con este comando podras eliminar un comando desde el cliente.

`create + el nombre del archivo` -> Con este comando podras eliminar un comando desde el cliente.

A continuación se da una descripción de los archivos cliente y servidor.

## `cliente.c`

Este archivo ([`cliente.c`](cliente.c)) implementa un cliente que tiene una de línea de comandos diseñado para interactuar con un servidor a través de una conexión TCP/IP. El usuario proporciona la dirección IP del servidor y el puerto como argumentos de línea de comandos. El programa entra en un bucle interactivo que permite al usuario enviar comandos al servidor, incluyendo operaciones como la creación, edición y eliminación de archivos.Utiliza funciones de comunicación TCP personalizadas y emplea códigos ANSI para imprimir mensajes coloridos en la consola. 

## `servidor.c`

Este archivo ([`servidor.c`](servidor.c)) implementa un servidor que escucha en un puerto específico (en este caso, 12348) para recibir conexiones TCP de clientes. Cada vez que un cliente se conecta, el servidor espera a que el cliente envíe comandos. Los comandos pueden incluir operaciones como "create" para crear archivos, "edit" para editar archivos existentes con el editor de texto "nano", y "delete" para borrar archivos. Además, el servidor puede ejecutar comandos del sistema operativo enviados por el cliente y redirigir la salida estándar y de error al cliente. El programa también maneja la finalización de la conexión cuando se recibe el comando "salida". Se utiliza la librería "tcp.h" para funciones de comunicación TCP personalizadas y la librería "leercadena.h" para leer cadenas de la entrada estándar. El servidor muestra mensajes informativos en la consola, utiliza códigos ANSI para imprimir texto en colores y maneja errores de manera adecuada.
