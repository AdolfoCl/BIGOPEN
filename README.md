# BIGOPEN

Este programa requiere un archivo DESCRIPTION de la base DMSII (UNISYS Clear Path MCP) que es necesario abrir después de una catástrofe.

## Escenario

Este utilitario es útil cuando por algún motivo extraordinario, las tablas de DMSII no pueden ser abiertas dado que el archivo de CONTROL de DMSII ya no las reconoce más. Tenga mucho cuidado en evaluar el escenario antes de proceder con esta solución.

## Estratégia

La idea es realizar un FIND FIRST en cada una de las estructuras (tablas) de la base de datos, con el fin de que la library DMSUPPORT coteje sus TIMESTAMP con los del archivo físico. En el caso de que los TIMESTAMP no coincidan, el programa queda en estado WAITING a la espera del comando "AX:OVERRIDE" por parte de un operador. En este caso es el propio programa quien se auto ingresa dicho comando.

IMPORTANTE: Luego de la ejecución de este programa el archivo de CONTROL de la base de datos reconocerá los archivos físico actualmente residentes en disco como los archivo válidos de la bases de datos, por lo tanto los respaldo previos ya no serán válidos.

### NOTA: UTILICE ESTE PROGRAMA CON PRECAUCIÓN

## Como Compilar:

Abra el programa fuente (BIGOPEN.dma_m) con el utilitario Programmer's Workbench (si no dispone de este utilitario copie el programa a un disco del equipo ClearPath)

Compile el programa con el compilador Algol (el utilitario Programmer's Workbench compila con Ctrl + Shift + F9). Si no posee el utilitario Programmer's Workbench, realice la compilación directamente desde CANDE con el comando:

```
    C <fuente> AS <object>
```

Donde "\<fuente>" es el nombre con el que haya creado el programa fuente en CANDE y "\<object>" el nombre con el que desee dejar el utilitario


## Como Ejecutar:

Una vez compilado el programa BIGOPEN podemos ejecutarlo para que genere un programa específico para una base de datos dada. Para esto ejecute en CANDE el siguiente comando:

```
    RUN <object>;FILE DB=<nombre de la base>
```

En donde "\<object>" es el nombre del código objeto del utilitario, por ejemplo:

```
    RUN BIGOPEN;FILE DB=COSTODB
```

En este caso se asume que el utilitario BIGOPEN fue compilado con el nombre "OBJECT/BIGOPEN" y existe una base de datos llamada COSTODB (por lo tanto existe un archivo DESCRIPTION/COSTODB)

El resultado de la ejecución será un programa DMALGOL con el nombre "SYMBOL/BIGOPEN/COSTODB". Compile el programa resultante con el comando:

```
    COMPILE SYMBOL/BIGOPEN/COSTODB AS $BIGOPEN/COSTODB
```

y luego ejecute el objeto resultante:

```
    RUN $BIGOPEN/COSTODB
```

## IMPORTANTE

Una vez ejecutado el programa, **DEBE** respaldar la base de datos, ya que todos los respaldo previos no son utilizables.