---
title: Uso básico de git
author: Joshua Haase
date: 2017-07-04 - INMEGEN
lang: es-mx
---

## Recapitulando

`git status`:
 ~ Ver qué archivos cambiaron.

`git diff`:
 ~ Ver qué ha cambiado y *no* va a salir en la foto.

`git add`:
 ~ Agregar archivos al escenario.

## Recapitulando

`git diff --cached`:
 ~ Ver qué va a salir en la foto.

`git commit`:
 ~ Tomar la foto.

`git checkout [commit]`:
 ~ Volver a una versión anterior del trabajo (todos los archivos).

`git checkout [commit] -- [archivos]`:
 ~ Regresar los archivos a una versión anterior.

## Conectarse a castillo con interfaz gráfica

- Sistemas de verdad:

```
ssh -X castillo.cluster.inmegen.gob.mx
```

## Usar meld

Verificar que el siguiente comando funciona:

```
meld
```

## Conectarse a castillo con interfaz gráfica

- [Windows](https://superuser.com/questions/119792/how-to-use-x11-forwarding-with-putty ):

    - Instalar [xming](https://sourceforge.net/projects/xming/ )

    - Configurar de la siguiente forma:

[![](https://i.stack.imgur.com/B7r4t.png){width="5cm", align="center"}](https://superuser.com/questions/119792/how-to-use-x11-forwarding-with-putty )

## Resolver conflictos

Verificar que el siguiente comando funciona:

```
meld
```


## Resolver conflictos

```
git clone git@github.com:xihh87/receta-de-pay.git
```

- *Ejercicio 1*: guardar cambios en git.

- *Ejercicio 2*: ejecutar el comando `git push`. En la mayoría de los casos va a fallar. ¿Por qué?

- *Ejercicio 3*: ejecutar `git pull`. El comando tendrá éxito pero en la mayoría de los casos habrá problemas.
    Para solucionarlos ejecutar `git mergetool`.

## Vivir sin preocupaciones

Agregar a `~/.bashrc`:

```
alias gs="git status"
alias gd="git diff"
alias gdc="git diff --cached"
alias gc="git commit"
alias gcm="git commit -m"
alias ga="git add"
alias gp="git push"
alias gb="git branch"
```

## Vivir sin preocupaciones

Agregar a `~/.bashrc`:

```
alias gl="git log"
alias glc="git log --cc"
```

## Vivir sin preocupaciones

Para aplicar los cambios en la configuración:

1. Cerrar todas las ventanas de byobu.

0. Salir del servidor.

0. Volver a entrar.

## Esta es una historia real...

\
![](data/imagenes/codigo1.jpg)

## Esta es una historia real...

![](data/imagenes/codigo1.jpg){width="5cm"} \
\
`git bisect [funciona] [no-funciona]` nos ayuda a encontrar el problema.


## Esto también pasa

\
![](data/imagenes/codigo2.jpg)

## Esto también pasa

![](data/imagenes/codigo2.jpg){width="5cm"} \
\
`git log --cc` nos da la respuesta si documentamos bien.
# Usar git mejor

## Git flow

```
git flow init
```


## Saber más

- [Tutorial de atlassian](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud/create-the-repository ) \
    <https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud/create-the-repository>

- [Listado de tutoriales](http://sixrevisions.com/resources/git-tutorials-beginners/ ) \
    <http://sixrevisions.com/resources/git-tutorials-beginners/>

- [Una propuesta de flujo de trabajo con ramas](http://nvie.com/posts/a-successful-git-branching-model/ ) \
    <http://nvie.com/posts/a-successful-git-branching-model/>
