---
title: Supercómputo en INMEGEN
subtitle: Seguridad de la información
author: Joshua Haase
date: 2017-06-15
---

## Seguridad Informática

~ Confiabilidad:
     Sólo las personas correctas pueden acceder y modificar los datos.

~ Confidencialidad:
     Sólo las personas correctas ven los datos.

~ Integridad de datos:
    Los datos que se muestran son los correctos.

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

- usuario ¿De quién son los archivos?
- grupo ¿Quiénes más tienen acceso extra?

##

----------------------------------------
  Permisos    Usuario   Grupo     Otros
----------- ---------- --------- -------
 123           --x       -w-       -wx

 456           r--       r-x       rw-

 755           rwx       r-x       r-x
----------------------------------------

## chmod, chgrp



# Contraseñas

## Contraseñas

![](http://imgs.xkcd.com/comics/password_strength.png)

## Contraseñas


https://blog.hubspot.com/hs-fs/hubfs/password-stats-infographic.jpg?t=1497478617668&width=669&height=5027&name=password-stats-infographic.jpg

## Uso de gestor de contraseñas

# Llaves criptográficas (clave pública y privada)

## Llaves criptográficas

Se generan un par de llaves:

![Clave pública](https://image.flaticon.com/icons/svg/1/1213.svg){width=5cm}
![Clave privada](https://cdn3.iconfinder.com/data/icons/wpzoom-developer-icon-set/500/104-512.png){width=5cm}

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
	-f ~/.ssh/inmegen
```

## Generar el par de llaves (putty)

https://www.howtoforge.com/ssh_key_based_logins_putty

# Confiabilidad

## ¿La información que tengo es la misma que me entregaron?

- Errores en transmisión.
- Usuario malicioso.


