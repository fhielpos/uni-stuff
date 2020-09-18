# Trabajo Practico 1

## Indice

1. [Enunciado](#enunciado)
2. [Respuestas](#respuestas)

    * [Ej 2](#ej-2)
    * [Ej 3](#ej-3)
    * [Ej 4](#ej-4)

## Enunciado

1. Leer el apunte de mips (http://se.fi.uncoma.edu.ar/ayodc1/apuntes/mips.pdf), con especialatención a la sección “5.1 directivas”.
    
2. Traduzca la siguiente estructura de un lenguaje de alto nivel a directivas de ensambladorde mips. Respete el orden de las variables. Tenga en cuenta que el string contiene uncarácter con valor cero para indicar el final de la cadena.

```c
struct {
    string user = "Juan”;
    byte edad = 42;
    float altura = 1.70;
    byte puntaje = 0;
    int key = 0xFA093319
} usuario_juan;
```

3. Represente un vuelco de memoria en hexadecimal (es decir, los contenidos de la memoria en hexadecimal) del segmento de datos que contiene a la estructura. Complete elespacio que se desperdicia por el alineamiento de los datos con ceros.
    
4. Reordenar la estructura para que se desperdicie la menor cantidad de espacio poralineamiento ¿Cuántos bytes de diferencia hay entre ambas versiones de la estructura?
    
5. Si la estructura original se coloca en la posición de memoria ​0x4000​ ¿Cuál es la direccióndel dato edad?

## Respuestas

### EJ 2

```
        .data
user:   .asciiz Juan
edad:   .byte 42
altura: .float 1.70
puntaje:.byte 0
key:    .word      
```

### EJ 3

|Dato|Memoria|
|--|--|
|user|0x4A75616D|
|edad|0x0000002A|
|altura|0x9a99d93F|
|puntaje|0x00000000|
|key|0xFA093319|

### EJ 4

```
        .data
user:   .asciiz Juan
edad:   .byte 42
puntaje:.byte 0
altura: .float 1.70
key:    .word      
```

### EJ 5

|Posicion|Dato|Memoria|
|:--:|--|--|
|0x00004000|user|0x4A75616D|
|0x00004005|edad|0x0000002A|
|0x00004008|altura|0x9a99d93F|
|0x0000400C|puntaje|0x00000000|
|0x00004010|key|0xFA093319|
