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

![](http://keepass.info/screenshots/keepass_2x/main_big.png)

<http://keepass.info>

# Llaves criptográficas (clave pública y privada)

## Llaves criptográficas

Se generan un par de llaves:

![Clave pública](/home/joshpar/src/github.com/INMEGEN/taller.supercomputo/imagenes/candado.png){width=5cm}
![Clave privada](https://cdn3.iconfinder.com/data/icons/wpzoom-developer-icon-set/500/104-512.png){width=5cm}\
```
      Clave pública                    Clave privada
```

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

# Confidencialidad

## Hay 3 permisos

-----------------------------------------------------------------
Permiso     Abreviatura   Valor  Utilidad
----------- ------------ ------- --------------------------------
 Lectura     r (read)       4    ¿Puedo ver los datos?

 Escritura   w (write)      2    ¿Puedo cambiar los datos?

 Ejecución   x (execute)    1    ¿Puedo usar este programa?
-----------------------------------------------------------------
: Permisos en unix

## Hay 3 contextos para los permisos

---------------------------------
 Usuario    Grupo      Otros
---------- ---------- -----------
   rwx        rwx        rwx

   421        421        421
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
  Permisos    Usuario   Grupo     Otros
----------- ---------- --------- -------
 123           --x       -w-       -wx

 456           r--       r-x       rw-

 755           rwx       r-x       r-x
----------------------------------------

## chmod, chgrp


## Cifrado

Más allá del alcance de este taller.

# Integridad

## ¿La información que tengo es la misma que me enviaron?

- Errores en transmisión.
- Usuario malicioso.

---

https://blog.hubspot.com/hs-fs/hubfs/password-stats-infographic.jpg?t=1497478617668&width=669&height=5027&name=password-stats-infographic.jpg
