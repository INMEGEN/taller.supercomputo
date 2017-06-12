# OpenSSH

## Preámbulo

[**OpenSSH**](https://www.openssh.com/) (Open Secure Shell) o **SSH** es un conjunto de herramientas para la comunicación segura entre computadoras usando el protocolo ssh. **OpenSSH** fue desarrollado como alternativa libre y de código abierto a la implementación [SSH](https://www.ssh.com/ssh/protocol/) propietaria.
**OpenSSH** es en realidad un conjunto de programas que ofrecen una alternativa a los protocolos de comunicación via red no encriptados como FTP.

__¡Vamos a conectarnos @ROGUE1!__


Conéctate a esta dirección:

```
castillo.cluster.inmegen.gob.mx
```
o, lo que es lo mismo, a esta IP `192.168.105.221`


### Si estás en GNU/Linux

 
1. Abre la terminal. 
  - Si estás en GNU/Linux `Ctrl`+`Alt`+`T`. 
  - Si estás en MacOSX `cmd`+`space` y luego teclea `terminal`+`↩︎`.

2. Teclear lo que sigue sustituyendo \<usuario> por tu nombre de usuario:
	
	```
	$ ssh <usuario>@castillo.cluster.inmegen.gob.mx
	```

3. Teclea tu contraseña (parecerá que no se está escribiendo nada) y da enter de nuevo

### Si estás en Windows

1. Abre PuTTY y escribe la dirección en el campo que dice "Host Name (or IP address)"

	```
	castillo.cluster.inmegen.gob.mx
	```
	
	![PuTTY](../imagenes/putty.jpg)

2. Clickea en `Open` (puede tardar unos segundo).

3. En la terminal que se abre, teclea tu nombre de usuario, da `enter`. 

4. Teclea tu contraseña (parecerá que no se está escribiendo nada) y da `enter` de nuevo.


<p align="center"> 
<big><b>¡Ya estas usando el Clúster del INMEGEN!</b></big>
</p>



## [***Byobu***](http://byobu.co/) 

Byobu es un multiplexador de terminal. Sirve, entre otras cosas, para proteger el trabajo que hacemos en el cluster de posibles desconecciones inesperadas desde nuestra terminal. 
 

```
$ byobu
```

¿Notaron que de lindo es? ;¬)

Algunos atajos de teclado importantes de Byobu son:

`F2` Crea una nueva ventana dentro de byobu

`F3` Cambia a la ventana anterior

`F4` Cambia a la ventana siguiente

`F6` Despégate de esta sesión 

Más atajos de teclado [aquí](http://byobu.co/documentation.html).


Veamos que podemos hacer aquí. Ve a la siguiente presentación:

## [Interface de línea de comandos](linea_de_comandos.md)





Salimos de Byobu.

```
$ exit
```

Cerramos la coneción SSH

```
$ exit
```

Iniciamos la conección con las X activadas ("C" es de compresión). Los maqueros necesitan arrancar X11 o XQuartz.

```
$ ssh -XC <usuario>@192.168.41.92 -p XXX 
```

Arrancamos Rstudio

```
rstudio
```

```
> setwd("~/ejercicioSSH")
```

```
> load("modAlt1.RData")
```

```
> plot(modAlt1,type="burn_in")
```

Desde una terminal parada en nuestra computadora

```
$ cd
```

```
$ mkdir mirmecoleon
```

```
$ sshfs <usuario>@192.168.41.92 ~/mirmecoleon -o local -o volname=mirmecoleon 
```

Busca tu disco mirmecoleon.

Para desmontar:

```
$ fusermount -u mirmecoleon
```

O bien expulsa como siempre haces con un dispositivo externo.