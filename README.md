# Projectes de C++ amb CMake

## CMake

(cross-platform make) és una eina de desenvolupament de programari gratuïta i multiplataforma per crear aplicacions mitjançant instruccions independents del compilador. També pot automatitzar proves, embalatge i instal·lació.

La clau de CMake és que no intenta ser un sistema de construcció sinó que simplement fa servir algun sistema existent. (Make, Visual Studio, Ninja, Qt, ...). També es pot fer servir per compilar, executar proves unitàries (CTest) i generar paquets (CPack)

No li diem *"Compila el fitxer main.cpp"* sinó que el que fem és dir-li *"agafa aquesta llista de codi i genera un executable"* i el que fa és generar les instruccions sobre com es construeix l'executable, no intenta construir-lo ell mateix.

El fet que generi instruccions de compilació li no ser una eina de compilació com Make, Ninja o MSBuild sinó que proporciona un llenguatge que serà transformat a les instruccions del sistema de construcció que trobi on el fem servir (Unix Make,  Ninja, CodeBlocks, Eclipse CDT,  KDevelop, Sublime Text 2,  Borland Make, MSYS Make, MinGW Make,  NMake, NMake JOM, Watcom WMake, Kate, CodeLite, Xcode, Visual Studio, ...)

## Funcionament

Es genera un fitxer amb les instruccions en l'arrel del projecte que s'ha de dir `CMakeLists.txt`. 

L'exemple més bàsic té tres línies:

- Versions de CMake que es poden fer servir
- Nom del projecte (s'hi poden definir la versió i el llenguatge)
- Generació de l'executable

```cmake
cmake_minimum_required(VERSION 3.10...4.0)
project(hello)
add_executable(hello src/main.cpp)
```

En l'exemple que hi ha en el repositori defineix que la versió de C++ a fer servir ha de ser de la versió 17.

### Generar un executable

Per generar un executable el primer es generar les instruccions de compilació (es recomana fer-ho en una carpeta exclosa de GIT per no emplenar de codi innecessari el repositori). És habitual que la carpeta es digui **build**

```bash
mkdir buid
cd build
cmake ..
```

> En el pas de generar el codi per compilar el programa es pot forçar el generador amb el paràmetre `-G` ( -G "Unix Makefiles", -G Ninja, etc..). Si no es proporciona, CMake en busca un en el sistema de forma automàtica.

Després per compilar només cal executar el programa que es fa servir `make` ... o bé fer-ho a traǘes de CMake:

```bash
cmake --build .
```

## IDES

La major part dels grans IDEs de C o C++ suporten CMake nadiuament o a través de components: Visual Studio, CLion, Eclipse, NeoVim, VSCode, ...

### VSCode

En VSCode s'instal·len els components CMake (per la sintaxi del fitxer CMakeLists.txt) i CMake-Tools (per compilar i debuggar automàticament)

![cmake-tools](images/cmake-tools.png)

Després s'ha d'executar el programa amb els botons que surten a la barra inferior del IDE.

![build](images/vscmake.png)

Per defecte l'executable sortirà en la carpeta `build`


## Exemples

### 1-Hola mundo

[Carpeta](1-Hola Mundo/)

Exemple bàsic que genera un Hello World a partir del número rebut a partir de la línia de comandes

```bash
$ echo 2 > data.txt
$ ./programa < data.txt
Hola mundo.
Hola mundo.
```

### 2-calle 

[Carpeta](2-calle/)

Determina si una llista de números de carrer estan a l'esquerra o a la dreta

```bash
$ echo 23 > data.txt
$ echo 2 >> data.txt
$ echo 0 >> data.txt
$ ./programa < data.txt
UZQUIERDA
DERECHA
```

En aquest projecte s'hi ha afegit la opció de fer tests que es poden avaluar amb CTest

```bash
$ ctest 
Test project /home/xavier/work-personal/programming/programame/02-calle/build
    Start 1: data1
1/4 Test #1: data1 ............................   Passed    0.02 sec
    Start 2: data2
2/4 Test #2: data2 ............................   Passed    0.01 sec
    Start 3: data34
3/4 Test #3: data34 ...........................   Passed    0.01 sec
    Start 4: data45
4/4 Test #4: data45 ...........................***Failed  Required regular expression not found. Regex=[^IZQUIERDA[
        ]*DERECHA[
        ]*IZQUIERDA[
        ]*DERECHA[
        ]*IZQUIERDA[
        ]*$
]  0.01 sec

75% tests passed, 1 tests failed out of 4

Total Test time (real) =   0.05 sec

The following tests FAILED:
          4 - data45 (Failed)
Errors while running CTest
Output from these tests are in: /home/xavier/work-personal/programming/programame/02-calle/build/Testing/Temporary/LastTest.log
Use "--rerun-failed --output-on-failure" to re-run the failed cases verbosely.
```

Com diu, dóna més detalls del que ha fallat si s'executa amb els paràmetres.