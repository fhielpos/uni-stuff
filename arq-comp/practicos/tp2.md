# Trabajo Practico 2

## Indice

1. [Enunciado](#enunciado)
2. [Respuestas](#respuestas)
    * [Ej 2](#ej-2)
    * [Ej 3](#ej-3)
    * [Ej 4](#ej-4)
    * [Ej 5](#ej-5)

## Enunciado

```
.data
    num:
        .word 2
    otrosNum:
        .byte 7, 18
    password:
        .word 0x70657065, 0x72696E61
    datos:
        .float 2.4
        .byte 3
        .half -1
    saludo:
        .asciiz "Buen dia"
    destino:
        .word 0
```
1. Utilizando las instrucciones ` lw ` y ` sw ` copiar la contraseña guardada en ` password ` a ` saludo `. Sabiendo que la contraseña está representada en ASCII ¿Cuál es la contraseña?

2. Cargar la palabra en ` num ` al registro t1 , cargar el dato ` -1 ` en t2 utilizando la instrucción ` li `. Luego sumar valores almacenados en t1 y t2 , y almacenar el resultado en
t3 .

3. Guardar en ` destino ` el cubo del valor almacenado en ` num `.

4. Si el segmento de datos está posicionado a partir de la dirección 0x2000 ¿Cuál será el contenido de los registros t4 y t2 luego de ejecutar las instrucciones ` lw $t4, 0x2014 ` y
` lw $t4, 0x201C `

5. ¿Cuáles de las instrucciones usadas en este práctico son instrucciones reales y cuales son pseudo instrucciones?

## Respuestas

### EJ 1

```
    .text
main:
    lw $t0, password
    lw $t1, password+4
    sw $t0, saludo
    sw $t1, saludo+4
```

**Password:** `peperina`

### EJ 2
```
    .text
main:
    lw $t1, num         # Load num in t1
    li $t2, -1          # Load -1 in t2
    addu $t3, $t1, $t2  # Add t1 to t2 and store it in t3
```

### EJ 3

```
    .text
main:
    lw $t1, num         # Load num in t1
    mul $t1, $t1        # Double t1
    mflo $t2            # Load LO in t2
    mul $t2, $t1        # Multiply t1 and t2
    mflo $t2            # Load LO in t2
    sw $t2, destino     # Store t2 in destino
```

### EJ 4

**Memoria:**

|POSICION|0 - 1 - 2 - 3|DATO|
|:--:|:--:|--|
|0x2000|00 00 00 02|.word 2|
|0x0004|07 13 00 00|.byte 7, 8|
|0x2008|70 65 70 65|.word 0x70657065|
|0x200C|72 69 6E 61|.word 0x72696E61|
|0x2010|40 06 66 66|.float 2.4|
|0x2014|03 00 FF FF|.byte 3, .half -1|
|0x2018|42 75 65 6E|.ascii "Buen"|
|0x201C|20 64 69 61|.ascii " dia"|
|0x2020|00 00 00 00|.ascii 0|
|0x2024|00 00 00 00|.word 0|

`lw $t2, 0x2014` -> $t2 = 03 00 FF FF

`lw $t4, 0x201C` -> $t4 = 20 64 69 61

### EJ 5

|PSEUDO|REAL|
|--|--|
|lw $t1, `<etiqueta>`|addu $t3, $t1, $t2|
|sw $t1, `<etiqueta>`|mul $t1, $t1|
|li $t2, -1|mflo $t2|

