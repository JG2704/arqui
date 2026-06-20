# Casos de prueba - Proyecto 2

## Caso 1: ejemplo del enunciado

Entrada:

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

Suma de cuadrados:

```text
1^2 + 57^2 + (-3)^2 + 72^2 + 56^2 + 31^2 + 1^2 + (-67)^2 + 61^2
= 20751
```

Resultado esperado:

```text
sqrt(20751) = 144.052073934...
```

Salida esperada aproximada:

```text
Distancia de Frobenius: 144.052074
```

## Caso 2: matrices iguales

Entrada:

```text
A =
[ 1 2 3 ]
[ 4 5 6 ]
[ 7 8 9 ]

B =
[ 1 2 3 ]
[ 4 5 6 ]
[ 7 8 9 ]
```

Diferencia esperada:

```text
[ 0 0 0 ]
[ 0 0 0 ]
[ 0 0 0 ]
```

Resultado esperado:

```text
0.000000
```

## Caso 3: matriz B en ceros

Entrada:

```text
A =
[ 1 2 3 ]
[ 4 5 6 ]
[ 7 8 9 ]

B =
[ 0 0 0 ]
[ 0 0 0 ]
[ 0 0 0 ]
```

Suma de cuadrados:

```text
1 + 4 + 9 + 16 + 25 + 36 + 49 + 64 + 81 = 285
```

Resultado esperado:

```text
sqrt(285) = 16.881943
```

## Caso 4: diferencias negativas

Entrada:

```text
A =
[ 1 1 1 ]
[ 1 1 1 ]
[ 1 1 1 ]

B =
[ 2 2 2 ]
[ 2 2 2 ]
[ 2 2 2 ]
```

Diferencia esperada:

```text
[ -1 -1 -1 ]
[ -1 -1 -1 ]
[ -1 -1 -1 ]
```

Resultado esperado:

```text
sqrt(9) = 3.000000
```

Este caso es útil para confirmar que el valor absoluto con `vandpd` funciona correctamente.
