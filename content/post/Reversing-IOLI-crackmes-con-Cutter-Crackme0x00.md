---
title: "Reversing IOLI Crackmes Con Cutter - Crackme0x00"
date: 2018-10-20T00:07:58-05:00
tags: ["crackme","ioli","cutter","radare","reversing"]
categories: ["reversing","ELF32"]
description: "En la entrada anterior, descargamos y revisamos muy por encima lo que Cutter nos presentaba con sus paneles y ventanas. Hoy daremos inicio al analisis del primer crackme de IOLI"
draft: true
---


En la entrada anterior, descargamos y revisamos muy por encima lo que Cutter nos presentaba con sus paneles y ventanas. Hoy daremos inicio al analisis del primer crackme de IOLI. 

Comenzamos por iniciar Cutter y presionar "Select", navegamos hasta la carpeta del crackme0x00 y lo seleccionamos. En mi caso queda de la siguiente manera:

![Open](img/cutter01/open.png)

Continuamos por dar a open y nos aparecerá la ventana de "Load Options", de momento continuamos con los valores por defecto y damos a "Load"

![Load](img/cutter01/load.png)

Tras acabar la identificación y analisis de nuestro binario, nos debera aparecer una ventana como la siguiente:

![initial-load](img/cutter01/initial-load.png)

## Footprint

Iniciamos realizando una identificación y exploración del binario, por lo que cambiamos al panel de dashboard para ver que tenemos con este binario. La primera columna nos dice la siguiente información:

![info1](img/cutter01/info1.png)

Tenemos un formato ELF32, escrito en C. Continuamos con la siguiente columna:

![info2](img/cutter01/info2.png)

Primero tenemos que tiene 3 File Descriptors, la base address es 0x08048000 (con el ajuste a 0), es user-space, sin canarios ni cryto. No es estatico, mas abajo veremos que tiene una libreria de la cual depende.

![info3](img/cutter01/info3.png)

Finalmente, en la tercera columna podemos observar que pertenece a una arquitectura i386 para Linux. Y como era de esperarse con intel, es little endian.

![info4](img/cutter01/info4.png)

La parte inferior nos indica que la entropia es moderadamente alta y que el binario depende de libc, lo cual lo convierte en LDD como se menciono atras.

## Entrypoints
Ok, antes de analizar la ventana de funciones, analizaremos cuantos puntos de entrada tiene este binario, para ello en la barra de menus seleccionaremos "Windows-\>Entry Points" lo cual nos abrira una pestaña más en la parte inferior:

![entrypoints1](img/cutter01/entrypoints1.png)

Al seleccionar "Entry Points" en la parte inferior, observaremos que nos vamos a otro panel en donde solo tenemos una entrada, la cual apunta a la dirección 0x08048360, la cual tambien es etiquetada como entry0 y es la posición donde actualmente nos encontramos. Un punto importante que aun no he mencionado a detalle es que, en la parte superior se  encuentra la barra de address/section, que nos marca en rojo la posición y seccion en la que nos encontramos actualmente, esto es bastante comodo cuando queremos movernos rapidamente sobre .text o otras secciones. 

    *Hit:* Podemos dar click derecho sobre la barra de desplazamiento y nos abrira un menu para seleccionar algun otra ventana que no tengamos en este momento, inclusive nos da un vistazo rapido a todas las ventanas posibles.

## Strings
Continuamos aun sin ir a la ventana de funciones, esta vez pasaremos rapido por strings. Para ello damos a la pestaña de strings que se encuentra en la parte inferior:

![strings1](img/cutter01/strings1.png)

Como dato a analizar, las direcciones de los primeros strings son diferentes a los de los ultimos. Tambien podemos observar que aparecen algunos mensajes probablemente asociados a la ejecución.

![strings2](img/cutter01/strings2.png)

## Funciones

