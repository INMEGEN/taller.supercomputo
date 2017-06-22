---
date: "2017-06-22"
title: "Supercómputo en INMEGEN"
subtitle: ""
author: "Joshua Haase"
---

# Recapitulando (mk, regexp)

## mkfile

`objetivo($target):ATTRIBUTOS:	ingredientes($prereq)`
\alert{$\langle tab \rangle$}`	receta(instrucciones)`

$\,$\
$\,$\
$\,$\

`mk` es como un chef.

`mkfile` es el recetario.

`bin/targets` son las órdenes de los clientes.

## mk

![`mk` genera un árbol de dependencias.](data/imagenes/mk-tree.png)

## Estructura de directorios

```
$ tree
./
├── mkfile
├── bin/
│   ├── targets
│   └── otros-scripts...
├── data/
│   └── pato
└── results/
    └── pato-a-la-naranja

3 directories, 5 files
```

## Expresiones regulares

![](data/imagenes/regexp_1.png ){align="center" width="60%"}
![](data/imagenes/regexp_2.png ){align="center" width="59%"}
![](data/imagenes/regexp_3.png ){align="center" width="20%"}

# Algoritmo para hacer mk

## Algoritmo para hacer mk

- Nombrar el platillo (nombres de archivo).
- Hacer la receta (comando).
- Probar el recetario (mkfile).
- Corregir el recetario (mkfile).
- Tomar las órdenes (bin/targets).
- Poner a trabajar al chef (comando final).

## Nombrar el platillo (nombres de archivo).

```
$ cat mkfile
results/%.asado:	data/%
```

## Hacer la receta (comando).

```
$ asar --con-pimienta pato > pato.asado
#
# se vuelve:
#
$ cat mkfile
results/%.asado:	data/%
	mkdir -p `dirname $target`
	asar --con-pimiemta $prereq > $target'.build' \
	&& mv $target'.build' $target
```

## Probar el recetario (mkfile).

```
mk results/pato.asado
```

Para saber qué se está ejecutando y cómo

```
mk -dep results/pato.asado | less
```

## Corregir el recetario (mkfile).

```
$ cat mkfile
results/%.asado:	data/%
	mkdir -p `dirname $target`
	asar --con-pimienta $prereq > $target'.build' \
	&& mv $target'.build' $target
$ mk -dep
```

## Tomar las órdenes (bin/targets).

```
$ cat bin/targets
#!/bin/sh
find -L data \
	-type f \
| sed -r \
	-e 's#^data/#results/#' \
	-e 's#$#.asado#'
```

## Poner a trabajar al chef (comando final).

```
bin/targets | xargs mk
```

**NOTA**: Este comando cambiará por otro para enviar trabajos al clúster.
Sin embargo debe funcionar para ejecutarse correctamente.

# Problemas comunes

## Permisos

```
chmod +x [archivos]
chmod +r [archivos]
chmod +w [directorio]
```

## Archivos vacíos

En caso de que sean correctos, ignorarlos:

```
$ cat bin/targets
#!/bin/sh
find -L data \
	-type f \
	'!' -empty \
| sed -r \
	-e 's#^data/#results/#' \
	-e 's#$#.asado#'
```


## Archivos vacíos o truncos

En caso de que esto sea incorrecto, repetir el análisis.

## Nombres de archivo con caracteres especiales

```
$ cat bin/targets
#!/bin/sh
find -L data \
	-type f \
| sed -r \
	-e 's#^data/#results/#' \
	-e 's#$#.asado#'
```


## Comandos incorrectos

```
mk -dep
```

Ejecutar el comando y corregirlo.

## El programa no está en la ruta (`PATH`)

Si el programa que estás usando no tiene la ruta

```
env PATH=$PATH:/ruta/al/programa mk 
```
