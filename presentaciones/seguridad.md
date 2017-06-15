---
title: Supercómputo en INMEGEN
subtitle: Seguridad de la información
author: Joshua Haase
lang: es-MX
date: 2017-06-15
---

## Seguridad Informática

Confiabilidad
: Sólo las personas autorizadas pueden acceder y modificar los datos.

Confidencialidad
: Sólo las personas autorizadas pueden acceder a los datos.

Integridad de datos
: Los datos que se muestran son los correctos.

# Confiabilidad

## ¿Cómo sé que el usuario es la persona correcta?

- Algo que sé (contraseñas).

- Algo que tengo (llaves).

- Algo que soy (huellas biométricas).

## Contraseña

Una contraseña es más fuerte mientras más grande sea el espacio de búsqueda.
Una buena contraseña es:

- Larga.
- Aleatoria.
- Única.

## Contraseñas

![Dos esquemas de contraseñas.](http://imgs.xkcd.com/comics/password_strength.png)

## Uso de gestor de contraseñas

![Un gestor de contraseñas.](http://keepass.info/screenshots/keepass_2x/main_big.png)

<http://keepass.info>

# Llaves criptográficas (clave pública y privada)

## Llaves criptográficas

Se generan un par de llaves:

![Clave pública.](../imagenes/candado.png)

![Clave privada.](https://cdn3.iconfinder.com/data/icons/wpzoom-developer-icon-set/500/104-512.png)

## Generar el par de llaves

```
# Generar el directorio para el par de llaves
mkdir -p ~/.ssh

# Sólo permitir acceso al usuario
chmod 700 ~/.ssh

# Generar el par de claves criptográficas
ssh-keygen \
	-t rsa \
	-b 4096 \
	-C "`whoami`@`hostname`" \
	-f ~/.ssh/"`whoami`@`hostname`"
```

## Configurar el cliente ssh

```
gedit ~/.ssh/config
```

```
Host *
    # Evitar que se desconecte la computadora
    ServerAliveInterval 100
    TCPKeepAlive    yes

    # Buscar los servidores en estos dominios
    CanonicalDomains        cluster.inmegen.gob.mx  inmegen.gob.mx
    CanonicalizeHostname    yes

    # Mostrar un identificador del servidor
    VisualHostKey   yes
```

## Configurar el cliente ssh

```

# Usar la clave pública de inmegen dentro del INMEGEN
Host    *.inmegen.gob.mx
    UserName	[usuario]
    IdentityFile    ~/.ssh/[usuario]@inmegen
```

## Generar el par de llaves (putty)

https://www.howtoforge.com/ssh_key_based_logins_putty

## ¿Funcionó el uso de claves?

Si la configuración quedó bien:

```
ssh castillo
```

debería entrar sin pedir contraseña.

# Confidencialidad

## Hay 3 permisos

-----------------------------------------------------------------
Permiso    |Abreviatura | Valor |Utilidad
-----------|------------|-------|--------------------------------
 Lectura   | r (read)   |   4   |¿Puedo ver los datos?
 Escritura | w (write)  |   2   |¿Puedo cambiar los datos?
 Ejecución | x (execute)|   1   |¿Puedo usar este programa?
-----------------------------------------------------------------
: Permisos en unix

## Hay 3 contextos para los permisos

---------------------------------
 Usuario  | Grupo    | Otros
----------|----------|-----------
   rwx    |   rwx    |   rwx
   421    |   421    |   421
---------------------------------
: Contextos para permisos en unix

¿Cómo ver los permisos?

```
$ ls -l
total 4
-rw-r--r-- 1 xihh    csbig 1472 Jun  9 19:37 permisos.md
permisos     usuario grupo
```

## Permisos, usuarios y grupos

Los archivos en UNIX tienen:

usuario
: ¿De quién son los archivos?

grupo
: ¿Quiénes más tienen acceso extra?

## Ejemplo de permisos

----------------------------------------
  Permisos |  Usuario | Grupo   | Otros
-----------|----------|---------|-------
 123       |   --x    |  -w-    |  -wx
 456       |   r--    |  r-x    |  rw-
 755       |   rwx    |  r-x    |  r-x
----------------------------------------

## Cambiar permisos (`chmod`)

Para cambiar los permisos se utiliza `chmod` (change mode):

```
# Dar permisos de sólo de lectura y escritura al usuario
chmod u=rw,go-rwx archivo.txt

# Dar permisos sólo de lectura y escritura al usuario
chmod 600 archivo.txt
```

**Sólo el dueño puede cambiar los permisos de los archivos.**

## Cambiar grupo

Para cambiar los permisos se utiliza `chmod` (change mode):

```
# Ver los grupos a los que se pertenece
$ id alumno1
uid=5051(alumno1) gid=9028(taller) groups=9028(taller)

# Cambiar el grupo con el que se comparte el archivo
$ chgrp [grupo] archivo.txt

```

**Sólo el dueño puede cambiar el grupo de los archivos
y sólo puede asignar un grupo al que pertenezca.**

## ¡NO! ¡NO! ¡NO!

![Permisos 777](https://img.devrant.io/devrant/rant/r_574536_X1cAX.jpg)

## Cifrado

Más allá del alcance de este taller.

```
# Para los intrépidos
man gpg
```

# Integridad ![](https://azurecomcdn.azureedge.net/cvt-82fa5a5b61233507a1f07292e1e92f1f94134e7850b2e6516294f02a7b6466a5/images/page/services/storage/data-integrity.png)

## ¿Por qué podría no estar bien mi información?

- Errores en transmisión.
- Usuario malicioso.
- Ataque informático.
- Virus.
- ¿Guardar cambios?

## Resúmen criptográfico (hash)

- Cambia mucho si cambia la entrada de datos.

```
$ echo pato | md5sum
b86f979c3e33ea6c2bc3fb8e423edd9f  -
$ echo peto | md5sum
9aacef02bf4e663962a3bff0bbf61f96  -
```

- Siempre muestra el mismo resultado ante la entrada de datos.

```
$ echo pato | md5sum
b86f979c3e33ea6c2bc3fb8e423edd9f  -
$ echo pato | md5sum
b86f979c3e33ea6c2bc3fb8e423edd9f  -
```

## Resúmen criptográfico (hash)

- No es falsificable.

```
$ md5sum data/seguridad.md
7b34ad9b6b85fc7d89483f326d3a3ffd  data/seguridad.md
```

![md5 ya es falsificable](../imagenes/2017-06-15-002117.png)

<https://blog.avira.com/md5-the-broken-algorithm>

## Revisar integridad de datos

Desde el lugar original de los datos:

```
find datos/ | xargs sha256sum > datos.sha256
```

Desde el la copia de los datos:

```
sha256sum -c datos.sha256
```
