---
date: "INMEGEN - 2017-06-27"
title: "Supercómputo en INMEGEN"
subtitle: "Clúster del INMEGEN @ROGUE1"
author: "Cristóbal Fresno"
---

# Historia

El **Clúster** del INMEGEN se llama `ROGUE1` y es un esfuerzo conjunto de la comunidad de INMEGEN en la cual han participado diferentes miembros de *Genómica Computacional*, *Unidad de Servicios Bioinformáticos*, *Subdirección de Bioinformática*, *Departamento de Soporte Tecnológico*, *Estudiantes* y diferentes grupos de investigación. Tiene como principal idea unificar la capacidad de cómputo instalada.

![](../imagenes/AntesHoy.png )

## Misión

La misión del Clúster es brindar el servicio de cómputo de alto rendimiento para abordar problemas de análisis de grandes volúmenes de datos de forma integrada, multidisciplinaria y novedosa, con la finalidad de satisfacer las necesidades bioinformáticas de los proyectos de investigación del *Instituto Nacional de Medicina Genómica*.

## Visión

Queremos consolidar la infraestructura para generar un equipo de bioinformáticos capaz de generar y aplicar herramientas analíticas centradas en la medicina genómica; convirtiéndonos en un referente en la investigación científica a nivel nacional e internacional.

# Qué ofrece el Clúster de INMEGEN

## Datos

- Confidencialidad en el acceso de los datos (grupos independientes).
- Control de integridad en la recepción de los datos.
- Redundancia y backups de los datos.

## Software

- Instalación acorde a las necesidades de cada proyecto.
- Actualizaciones de software.

## Colas de trabajo

- Maximizar los recursos disponibles del Clúster.
- Esquema de prioridades conforme las necesidades del proyecto y la contribución.

## Soporte

- Capacitación sobre herramientas (próximamente curso formal).
- Asistencia personalizada en correr el flujo de trabajo requerido por el proyecto.

# Infraestructura

Actualmente `ROGUE1` se encuentra instalado en el *site* de INMEGEN, en sótano 1 y se encuentra a cargo de la *Subdirección de Bioinformática* que dirige el *Ing. Joshua Ismael Haase Hernández*. Básicamente ROGUE1 se encuentra compuesto por:

![](../imagenes/Infraestructura.png)

una *unidad de cintas para backup*, un *storage de alta velocidad* y *nodos de cómputo*. A la fecha se posee de 208/300 núcleos, 1.6/2.0 TB de memoria RAM y storage de 50/200 TB, ambos dos últimos distribuidos. Esta capacidad se encuentra en constante crecimiento y esperamos poder contar con el apoyo es sus próximos proyectos de investigación.

Actualmente `ROGUE1` se encuentra basado en una arquitectura `heterogénea` de servidores en lo que respecta a cantidad de núcleos de 64 bits, memoria RAM y almacenamiento, bajo Ubuntu Server 16.04.2 LTS. En este contexto es **MUY importante** que se mantenga `homogenea` la estructura lógica de software, por lo que invitamos a los usuarios a solicitar al `administrador` que se instale la misma versión de software en todos los nodos de cómputo.

# HTCondor

![](../imagenes/htcondor.png)

El gestor de colas de trabajo actualmente implementado es [HTCondor](https://research.cs.wisc.edu/htcondor/) (conocido simplemente como *condor*). Para su correcta utilización es necesario conocer cómo se disponen los diferentes módulos de condor en el clúster:

- `Nodo cómputo`: Es aquel que realiza tareas de *procesamiento*.
- `Nodo cola`: Es la cola de trabajo propiamente dicha y se pueden enviar/eliminar trabajos de ella.
- `Nodo maestro`: Es aquel que *coordina/controla* las tareas entre la cola de trabajo y los nodos de cómputo

En `ROGUE1` la infraestructura se encuentra dispuesta de la siguiente manera:

![](../imagenes/htcondor_rogue1.png)

## Filosofía

Este gestor de colas trabaja bajo el paradigma de **anuncios de clasificados de periódico**. Así cada nodo de cómputo sabe que recursos computacionales posee y lee el periódico en busca de potenciales trabajos. Así, los trabajos que se envíen a la cola deben tener una estructura que solicite los recursos computacionales adecuados y el primer nodo disponible tomará el trabajo.

Los **anuncios** básicamente cuentan con un *encabezado* en el cual se definen recursos computacionales y un *cuerpo* donde se define el/los trabajo/s propiamente dichos. Más adelante volveremos sobre este tema en detalle.

Por último, HTCondor no es una simple cola de trabajo sino que gestiona los recursos disponibles basado en un esquema de `prioridades`. Así, la prioridad por defecto se calcula con una ventana de tiempo de un (1) día, y se pondera inversamente a la carga de trabajo enviada, es decir, cuanto más trabajos se envíen menor es la prioridad frente a un usuario que no enviá trabajos. Claro está que también pueden existir cuotas por grupo de investigación, prioritarias sobre los equipos aportadas, etc.

# Condor-mk

No es una tarea fácil generar los archivos necesarios que requiere condor. No obstante, podemos utilizar lo que ya sabemos de `mk` para hacernos el día a día más fácil. Para ello, ya se cuenta con un paquete que realiza condor basado en mk (`condor-mk`).

## Estructura de archivos de Condor-Mk

`Condor-mk` requiere de la siguiente estructura de proyecto para poder funcionar de forma correcta:

```
./
├── mkfile
├── bin/
│   └─── targets
│   └─── otros_scripts
├── data/
│   └─── archivo_con_datos_1
│   └─── archivo_con_datos_2
│   └─── archivo_con_datos_1
.   .           .
.   .           .
.   .           .
└── results/
    └─── archivo_con_resultados_1
    └─── archivo_con_resultados_2
    └─── archivo_con_resultados_3
    .           .
    .           .
    .           .
```

donde tendremos nuestro propio `mkfile` tal como lo veníamos haciendo, un directorio `bin` donde se encuentren nuestros scripts y en especial `bin/targets` que pedirá todas las ordenes al chef (`mk`), un directorio `data` con los archivos de entrada (si existieran) y una carpeta `results` con los archivos de salida que generará nuestro mkfile.

Ahora que tenemos todo preparado podemos inspeccionar cómo son los objetos de `condor-mk` para ver que realiza cada uno de ellos:

```
cat /usr/lib/condor-mk/condor.mk | grep ":"
```

donde encontramos:

- `condor.sub: condor.header`: Genera cada uno de los jobs acorde a los archivos de salida generados por `bin/targets`.
- `condor.header`: Genera el encabezado que utilizará condor para publicar en el anuncio los recursos computacionales requeridos.
- `submit:V: init condor.sub`: Envia el archivo condor.sub a la cola de trabajos y para ello también requiere de init.
- `init:V: condor.sub`: Genera la estructura de directorios que requiere condor previo al envio de trabajos.
- `time:QV:` Calcula la diferencia de tiempo, en segundos, entre el envio del trabajo y el tiempo total de ejecución.
- `check:QV:` Muestra los archivos del `bin/targets` que aún no se han procesado.
- `clean:QV:` Borrar el archivo condor.sub y los results/*.condor_* temporales.
- `mrproper:V: clean`: Borrar lo de clean y adicionalmente condor.header.

Para hacer más amigable el trabajo con condor-mk se creó el alias `condor <objetivo>` para evitar invocar mk -f /usr/lib/condor-mk/condor.mk <objetivo>.

### Condor.header

Viendo en detalle la receta de cocina:

```
condor.header:
        cat > $target".build" << EOF
        # Opciones generales de HTCondor.
        universe = vanilla
        #
        initialdir = `pwd`
        should_transfer_files = NO
        getenv = True
        #
        # Recursos necesarios para ejecutar el trabajo.
        request_cpus = 1
        request_memory = 3GB
        request_disk = 1GB
        EOF
        mv $target".build" $target

```

vemos que se define `vanilla` como universo de trabajo, el directorio inicial de trabajo desde donde se invocó `condor condor.header`, que *NO* transfiera los archivo ya que utilizaremos Network File System por defecto, utiliza el mismo `PATH` que se encuentra en sistema y por último los recursos computacionales necesarios (cantidad de cpus, memoria RAM y espacio en disco).

### Condor.sub

Viendo en detalle la receta de cocina:

```
condor.sub: condor.header
        condor check \
        | /usr/bin/condor-sub $prereq \
        > $target".build" \
        && mv $target".build" $target

```

que a su vez invoca a /usr/bin/condor-sub quien llama a /usr/lib/condor-mk/condor.body para finalmente incorporar:

```
#!/usr/bin/awk -f
{
        print ""
        print "executable = /usr/bin/time"
        print "arguments = /usr/lib/plan9/bin/mk " $0
        print "output = " $0 ".condor_out"
        print "log = " $0 ".condor_log"
        print "error = " $0 ".condor_err"
        print "queue"
}

```

Es decir, una línea que llama al `executable` que en este caso es `time` para medir el tiempo, los argumentos es la invocación de `mk` con nuestro mkfile y uno de los objetivos del `bin/targets` y los tres registros de condor_(out/log/err).

## Comandos básicos de HTCondor

Para comunicarse con los diferentes módulos de condor hay que utilizar una suite de comandos del estilo `condor_xx` donde *xx* puede ser alguno de los siguientes:
- `condor_status`: Muestra el estado de los nodos de cómputo.
- `condor_status -master`: Muestra la infraestructura disponible por HTCondor.
- `condor_q`: Muestra el estado de la cola de trabajo del usuario.
- `condor_q -all`: Muestra el estado de la cola global de trabajo.
- `condor_submit <archivo.sub>`: Envia el archivo de trabajo a la cola.
- `condor_rm <id>`: Remueve el trabajo con el <id> especificado.
- `condor_rm -all`: Remueve los trabajos enviados por el usuario.

Una lista completa puede verse en el [manual](http://research.cs.wisc.edu/htcondor/manual/index.html) de HTCondor y/o en la sugerencia de comandos útiles [aquí](http://www.iac.es/sieinvens/siepedia/pmwiki.php?n=HOWTOs.CondorUsefulCommands).

## Depurar errores

No todo es color de rosa y menos en HTCondor. Es por ello que posee de ciertas herramientas para debuguear como por ejemplo:

- `condor_q -analyze`: Permite ver mayor información del trabajo (debug).
- `condor_q -better-analyze`: Aumenta el grado de descripción de la tarea (debug).
- `condor_q -hold`: Muestra los trabajos que se encuentran en espera (*HOLD*), probablemente, falta de permisos, latencia de la red, etc.
- `condor_release <id>`: Cambia el estado del trabajo <id> de HOLD a disponible (*IDLE*) para comprobar si se resolvió el inconveniente.
- `condor_release -all`: Cambia el estado de todos los trabajos de HOLD a IDLE.

Adicionalmente se pueden ver los archivos generados por condor:

- `results/%.condor_err`: Mensajes enviados a stderr por el programa utilizado de turno.
- `results/%.condor.out`: Tareas registrada por mk y tiempo de ejecución.
- `results/%.condor.log`: Registro de las actividades realizadas por HTCondor sobre el trabajo. Por ejemplo: se envió, comenzó a ejecutar, quedó en espera y resumen de recursos ocupados.

# Ejercicios

1. ¿Cual es el nodo que mayor cantidad de memoria hay disponible? ¿Y en cantidad de núcleos?

```
condor_status -master
```

2. ¿Cómo vemos el estado de carga actual del cluster?

```
condor_status
```

3. ¿Cómo vemos si tenemos trabajos en la cola? ¿y hay alguien más corriendo trabajos?

```
condor_q
condor_q -all
```

4. Mi primer trabajo enviado a la cola.

Se requiere que le solicite a 10 trabajos de condor que emitan un saludo desde el nodo correspondiente en el cual se corrió el trabajo. Por ejemplo:

```
Hola desde ibm4
Hola desde dell2
...
```

- Para realizar esta tarea genere un directorio llamado hola que posea la estructura requerida por condor-mk:

```
cd
mkdir -p hola
mkdir -p hola/bin
mkdir -p hola/data
mkdir -p hola/results
cd hola
```

- Cree el `mkfile` correspondiente para que realice el saludo y lo guarde en un archivo `results/%.txt` utilizando el comando:

```
echo "Hola desde `hostname`"
```

como lo hicieramos en la sesión pasada utilizando *BUENAS PRÁCTICAS*.

- Pruebe su `mkfile` para ver que funciona de forma correcta:

```
mk results/hola1.txt
```

- Genere el `bin/targets` correspondiente para que solicite 3000 saludos.

```
#!/bin/sh
for n in $(seq 1 3000)
do
    echo "results/hola${n}.txt"
done
```

recuerde cambiar los permisos acorde sea necesario.

- Pruebe que el mk + bin/targets funcionan de forma local correctamente:

```
bin/targets
bin/targets | xargs mk
ls results/
rm results/*
```

- Envie el trabajo al cluster completo y monitorize su estado:

```
condor submit
condor_q
condor check
ls results/
cat results/hola1.txt.condor_out
cat results/hola1.txt.condor_err
cat results/hola1.txt.condor_log
```

¿Corrió de forma apropiada?
	- Si, pase al punto siguiente.
	- No. Verifique con `condor_q -hold` y si fuere el caso libere el trabajo con `condor_release -all` o remuévalo de la cola con `condor_rm -all`

- Verifique la salida de `results/hola1.txt`, `results/hola1.txt.condor_err`, `results/hola1.txt.condor_out` y `results/hola1.txt.condor_log`

¿Qué hay en cada archivo?
¿Cuántos recursos utilizó de los pedidos?
¿Cuánto tiempo demoró? Sugerencia: `condor time`

- Borre la salidas condor y propias:

```
condor clean
rm results/*
```

- Modifique el archivo `condor.header` para solicitiar 50 cpus y vuelva a enviar su trabajo.

¿Cómo hace debug para determinar la causa del HOLD de la totalidad de los trabajos?

```
condor_q -analyze
condor_q -better-analyze
```

¿Qué fue lo que pasó?

- Finalmente remueva los trabajos que no puede realizarse:

```
condor_rm -all
```


5. Envié el trabajo real de la clase pasada, para realizar la búsqueda de secuencias con condor.

```
git clone --branch=condormk /home/xihh/mk-debug/pato.git condor
```
