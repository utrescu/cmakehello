# Hola mundo
El programa escriu tantes vegades "Hola mundo." com el número que rebi des de l'entrada estàndard.

## Compilar i executar

En els projectes CMake s'han de generar les instruccions de construcció i després compilar. Per tant:

Generem les instruccions de compilació

```bash
mkdir build
cd build 
cmake ..
```

Compilem el programa

```bash
cma1ke build .
```

El programa apareixerà en la carpeta build i ja es pot executar.

```bash
$ echo 3 > data.txt
$ ./programa < data.txt
Hola mundo.
Hola mundo.
Hola mundo.
```
