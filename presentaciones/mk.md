# mk


[*Mk*](https://swtch.com/plan9port/man/man1/mk.html) es una herramienta general y eficiente para describir y mantener dependencias entre archvos y programas. Mk es compatible y al estilo de la herramienta de UNIX *make*. Las ventajas más importantes de mk sobre make es que ejecuta recetas en paralelo, usando metareglas de emparejado de patrones más que reglas de transformación de sufijos, y de resolucion de dependencias por cierre transitivo de todas las reglas. Mk corre donde sea de 2 a 30 veces más rápido de make.

Un ejemplo

Dado que mk está basado en make y make es un programa para instalar programas, vamos a ejemplificar mk con la instalación de un programa que, en este caso se llamará **prog**. Digamos que **prog** se hace a partir de **a.o** y **b.o**, los cuales a su vez se hacen al compilar **a.c** y **b.c** respectivamente. Además **b.c** incluye un archivo de cabecera **prog.h**. Representamos estas relaciones esquemáticamente como sigue: 

<img src="../imagenes/mk1.png" height = "150">

Las flechas significan "depende de". Por tanto **porg** dende de si **a.o** y **b.o** se modifcian y si **a.o** y **b.o** cambian entonces **prog** debe ser hecho de nuevo. Igualmente, **a.o** depende de **a.c** y **b.o** depende de **b.c** y de **prog.h**.

La descripción en texto de de como se hace **prog** se escribe en el *mkfile* y se vería así:

<img src="../imagenes/mk2.png" height = "130">

El mkfile es una secuencia de *reglas*. Cada regla define un objetivo (*tarjet*. Por ejemplo **prog**.) estos dependen de algunos prerequisitos (**a.o** y **b.o**) y los comandos (un script de shell llamado *receta*) para mantener el target actualizado. *Mk* toma esta descripción de un archivo llamado **mkfile** y contruye los targets indicados. Si en la linea de comandos no se indican tarjets se hace el primer tarjet del mkfile. Por ejemplo, si ejecutamos solo con las fuentes de nuestros archivos en nuestro directorio, *mk* creará **prog** compilando **a.c** y **b.c**.

<img src="../imagenes/mk3.png" height = "120">

Ejecutando *mk* de nuevo se hará nada, ya que **prog** estará actualizado.

<img src="../imagenes/mk4.png" height = "70">

Si cambiamos un archivo fuente, *mk* solo reconstruirá aquellos archivos que no están actualizados.

<img src="../imagenes/mk5.png" height = "120">






http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeUwRepliSeq/