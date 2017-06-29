---
date: "2017-06-28"
title: "Supercómputo en INMEGEN"
subtitle: "Buenas prácticas de desarrollo en Investigación"
author: "Joshua Haase"
---

# Gestión de proyectos de investigación

## Respaldos

> No quieres la información que no tienes en 3 lugares *diferentes*.

## Respaldos

- Datos originales.
- Flujo de procesamiento de datos.
- Resultados.

**¡Borrar los datos intermedios!**

## Respaldos

NO es un respaldo:

- Dos veces la información en el mismo disco.
- Esquemas de protección de datos (RAID1, snapshots).
- «Mi disco está nuevo».

## Documentación

¿Para qué hacemos una bitácora de laboratorio?

## Documentación

- Recordar qué hicimos y por qué lo hicimos.
- Integrar nuevas personas al proyecto.

## Estructura de archivos y permisos.

```
/labs/
└── grupo-de-investigación/
    ├─── README.md # Explicando qué proyectos hay.
    └─── proyecto/
         ├─── README.md # Explicando de qué trata el proyecto.
         ├─── data/
         └─── analysis/
              ├─── README.md # Explicando qué hace cada paso del análisis y por qué.
              ├─── 001/
              │    ├─── README.md # Explicando qué hace este paso del análisis y cómo se ejecuta/configura.
              │    ├─── config.mk
              │    ├─── mkfile
              │    ├─── bin/
              │    │    └─── targets*
              │    ├─── data/ -> ../../data/
              │    └─── results/
              └─── 002/
                   ├─── README.md # Explicando qué hace este paso del análisis y cómo se ejecuta/configura.
                   ├─── config.mk
                   ├─── mkfile
                   ├─── bin/
                   │    └─── targets*
                   ├─── data/ -> ../001/results/
                   └─── results/
```

## Control de versiones

- No necesariamente es un respaldo, pero podría serlo.

---

[Presentación de git](presentaciones/git.md )

## Control de versiones

- [Git Flow](http://nvie.com/posts/a-successful-git-branching-model/ "Por qué usar git flow.")

- [Referencia de `git flow`](https://danielkummer.github.io/git-flow-cheatsheet/#getting-started "Cómo usar git flow.")

## Consistencia en herramientas.



## Aislamiento de aplicaciones.


--

Control de calidad en cada etapa.  Toba (en general sesgos y exploracion de datos)
