# Calle

Determina si una llista de números de carrer estan a l'esquerra o a la dreta.

L'entrada són una llista de números acabats en un zero.

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

## Testos de programari

En aquest projecte s'ha afegit la carpeta tests en la que es poden definir
tests per comprovar que el programa està funcionant correctament

La carpeta tests té el seu propi fitxer `CMakeLists.txt` que se n'encarrega
de generar-los.

Els tests són arxius de text on hi ha una llista de números de carrer que
s'ha d'avaluar. Segons l'enunciat ha d'acabar en zero

```bash
$ cat data2.txt
2
4
0
```

Es poden exeutar els tests amb `ctest`

```bash
 ctest
Test project /home/xavier/work-personal/programming/programame/01-helloworld/2-calle/build
    Start 1: data1
1/4 Test #1: data1 ............................   Passed    0.01 sec
    Start 2: data2
2/4 Test #2: data2 ............................   Passed    0.01 sec
    Start 3: data34
3/4 Test #3: data34 ...........................   Passed    0.01 sec
    Start 4: data45
4/4 Test #4: data45 ...........................   Passed    0.01 sec

100% tests passed, 0 tests failed out of 4

Total Test time (real) =   0.03 sec
```

## Afegir nous tests

Per fer un nou test només cal afegir un arxiu de dades nou,i anar al
fitxer `tests/CMakeLists.txt` i afegir-hi el test:

Bàsicament és afegir una línia **execute_test()** amb els paràmetres (és una
funció que he definit per no haver de repetir codi):

- nom de l'arxiu
- resultat esperat (amb **\n** pels salts de línia)

```cmake
function (execute_test data result)
    get_filename_component(test_name ${data} NAME_WE)
    add_test(NAME ${test_name} COMMAND sh -c "cat ${CMAKE_CURRENT_SOURCE_DIR}/${data} | ${CMAKE_BINARY_DIR}/${CMAKE_PROJECT_NAME}")

    set(input "^${result}$")
    string(REPLACE "\n" "[\n\r\t]*" expected "${input}")

    set_tests_properties(${test_name} PROPERTIES
        PASS_REGULAR_EXPRESSION "${expected}")
endfunction()

#
# Definir els tests
# ----------------------------------------------

execute_test("data1.txt" "IZQUIERDA\n")
execute_test("data2.txt" "DERECHA\nDERECHA\n")
execute_test("data34.txt" "DERECHA\nDERECHA\nDERECHA\nIZQUIERDA\n")
execute_test("data45.txt" "IZQUIERDA\nDERECHA\nIZQUIERDA\nDERECHA\nIZQUIERDA\n")
```

Abans de que sigui reconegut s'ha de reconstruir el projecte perquè generi el codi
del test:

```bash
cmake ..
```
