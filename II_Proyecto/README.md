# Proyecto 2 - Distancia de Frobenius con AVX

Este proyecto calcula la distancia de Frobenius entre dos matrices `3x3` usando ensamblador x64 con instrucciones AVX.

## Objetivo

Dadas dos matrices `A` y `B` de `3x3`, el programa calcula:

```text
d(A, B) = ||A - B||_F = sqrt( sum_i sum_j |a_ij - b_ij|^2 )
```

## Requisitos clave del enunciado

- Pedir al usuario los valores de punto flotante de las matrices `A` y `B`.
- Calcular la matriz diferencia `A - B`.
- Usar aritmética empacada AVX para la diferencia matricial.
- Usar instrucción `AND` empacada para calcular el valor absoluto.
- Mostrar el resultado en consola.
- Para este proyecto no es obligatorio usar módulos.

## Implementación actual

Archivo principal:

```text
src/main.asm
```

La implementación usa precisión doble (`REAL8`) y guarda cada fila de 3 elementos como un vector de 4 doubles:

```text
[x1, x2, x3, 0.0]
```

Ese cuarto elemento es padding y permite procesar cada fila con un registro `YMM` de 256 bits.

## Instrucciones AVX principales

- `vmovupd`: carga/guarda vectores de doubles.
- `vsubpd`: calcula la diferencia empacada `A - B`.
- `vandpd`: calcula el valor absoluto apagando el bit de signo con una máscara.
- `vmulpd`: eleva al cuadrado cada diferencia absoluta.
- `vaddpd`: acumula sumas parciales por lane.
- `vsqrtsd`: calcula la raíz cuadrada final.

## Configuración recomendada en Visual Studio

1. Crear un proyecto C++ vacío de consola.
2. Usar plataforma `x64`.
3. Activar MASM en `Build Customizations`.
4. Agregar `src/main.asm` como archivo fuente.
5. Como esta versión usa `printf` y `scanf` del CRT, dejar el `Entry Point` por defecto del proyecto. No configurar `Linker > Advanced > Entry Point = main` para esta versión.
6. Compilar y ejecutar.

## Caso de prueba del enunciado

```text
A =
[ 4   71   12  ]
[ 81  82   84  ]
[ 6   22   140 ]

B =
[ 3   14   15 ]
[ 9   26   53 ]
[ 5   89   79 ]
```

Diferencia esperada:

```text
A - B =
[ 1   57   -3 ]
[ 72  56   31 ]
[ 1  -67   61 ]
```

Distancia esperada aproximada:

```text
144.052073
```

El enunciado redondea este valor como `144,052`.
