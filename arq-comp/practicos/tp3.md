# Trabajo Practico 3

## Indice

1. [Enunciado](#enunciado)
2. [Respuestas](#respuestas)
    * [Ej 1](#ej-1)
    * [Ej 2](#ej-2)
    * [Ej 3](#ej-3)
    * [Ej 4](#ej-4)

## Enunciado

Dadas las siguientes declaraciones de la sección de datos, y sabiendo que el segmento de datos se carga a partir de la dirección de memoria `0x0100 0000`, y el segmento de texto a partir de la dirección `0x0400 0000`.

```
    .data
nick:
    .asciiz "peperina"
lastLogin:
    .word 1600430986
datos:
    .byte 15
    .float 0
    .half 0x7FFF
#password (esto es un comentario, no una etiqueta)
    .word 0x68756E74, 0x65723200, 0x00000000
saludo:
    .asciiz "Buen dia <nick>!"
```

1. ¿Cuantos bytes son reservados por el segmento de datos? ¿cuántos son esperdiciados por alineamiento?
2. ¿Cuál es el desplazamiento del primer byte de la contraseña con respecto a la etiqueta ` datos ` ? ¿Cuál es la dirección del carácter `!`?
3. ¿Cuáles serán los contenidos de los registros `t1, t2, y t3` luego de ejecutar las siguientes instrucciones? ¿Cuáles son las direcciones efectivas de los datos cargados para cada instrucción de carga?

```
li $t0, 4
lh t1, lastLogin+4($t0)
addi $t0, $t0, 4
lw t2, lastLogin($t0)
la $t0, datos
lb $t3, 9($t0)
```

4. Utilizando las instrucciones ` lb `, ` sb `, instrucciones de salto y cualquier otra que considere necesaria, copiar la contraseña guardada en ` password ` a ` saludo `. Como la contraseña está representada en ASCII , copie los bytes hasta el primer byte en cero, el código debe seguir funcionando aunque el usuario cambie la longitud de la contraseña ¿Cuál es la contraseña?

## Respuestas

### EJ 1

|POSICION|0 - 1 - 2 - 3|DATO|
|:--:|:--:|--|
|0x0100 0000| 70 65 70 65 | .ascii "pepe" |
|0x0100 0004| 72 69 6E 61 | .ascii "rina" |
|0x0100 0008| 00 00 00 00 | .ascii "0" |
|0x0100 000C| 5F 64 A3 8A | .word 1600430986 |
|0x0100 0010| 0F 00 00 00 | .byte 15|
|0x0100 0014| 00 00 00 00 | .float 0|
|0x0100 0018| 7F FF 00 00 | .half 0x7FFF|
|0x0100 001C| 68 75 6E 74 | .word 0x68756E74|
|0x0100 0020| 65 72 32 00 | .word 0x65723200|
|0x0100 0024| 00 00 00 00 | .word 0x00000000|
|0x0100 0028| 42 75 65 6E | .ascii "Buen" |
|0x0100 002C| 20 64 69 61 | .ascii " dia" |
|0x0100 0030| 20 70 65 70 | .ascii " pep" |
|0x0100 0034| 65 72 69 6E | .ascii "erin" |
|0x0100 0038| 61 21 00 00 | .ascii "a!0" |
|...|...|...|
|0x0400 0000|00 00 00 00|...|

Bytes desperdiciados por alineamiento: `8 bytes`

### EJ 2

Desplazamiento desde la etiqueta `datos` al primer byte de `password`: 12 bytes
Direccion del caracter `!`: `0x0100 0039`

## EJ 3

VALORES DE LOS REGISTROS:
```
li $t0, 4                   # t0 = 4          
lh t1, lastLogin+4($t0)     # t1 = 0x00 00
addi $t0, $t0, 4            # t0 = 8
lw t2, lastLogin($t0)       # t2 = 0x00 00 00 00
la $t0, datos               # t0 = 0x01 00 00 10
lb $t3, 9($t0)              # t3 = 0xFF FF FF FF
```

DIRECCIONES EFECTIVAS:
```
li $t0, 4                   # t0 = 4
lh t1, lastLogin+4($t0)     # lastLogin+4($t0) = 0x0100 0014
addi $t0, $t0, 4            # t0 = 8
lw t2, lastLogin($t0)       # lastLogin+($t0) = 0x0100 0014
la $t0, datos               # datos = 0x0100 0010
lb $t3, 9($t0)              # 9($t0) = 0x0100 0019
```

**NOTA:**
`lb` completa con el bit más significativo. Mientras que `lbu` no.
`lb $t3, 0x0100 0019`

`0x0100 0019` = FF
`FF` = (1)111 1111
`$t3` = 0xFF FF FF FF

`lbu $t3, 0x0100 0019` = 0x00 00 00 FF