# Diseño de memoria - Proyecto 2

## Decisión principal

Aunque las matrices son de `3x3`, internamente cada fila se guarda como si tuviera 4 elementos:

```text
[x1, x2, x3, 0.0]
```

El cuarto valor es padding. Esto permite cargar una fila completa en un registro `YMM` de 256 bits usando precisión doble.

## Por qué funciona

Un registro `YMM` tiene 256 bits.

Un `double` ocupa 64 bits.

Por tanto:

```text
256 bits / 64 bits = 4 doubles
```

Entonces, cada fila se representa así:

```text
Fila real:      [a11, a12, a13]
Fila en memoria:[a11, a12, a13, 0.0]
```

## Distribución en memoria

Cada matriz usa 12 doubles:

```text
MatrizA:
Fila 1: A[0][0], A[0][1], A[0][2], 0.0
Fila 2: A[1][0], A[1][1], A[1][2], 0.0
Fila 3: A[2][0], A[2][1], A[2][2], 0.0
```

Como cada double ocupa 8 bytes, cada fila ocupa:

```text
4 * 8 = 32 bytes
```

Offsets usados:

```text
Fila 1: +0 bytes
Fila 2: +32 bytes
Fila 3: +64 bytes
```

## Variables principales

```asm
MatrizA   REAL8 12 DUP(0.0)
MatrizB   REAL8 12 DUP(0.0)
MatrizD   REAL8 12 DUP(0.0)
MatrizAbs REAL8 12 DUP(0.0)
TempSums  REAL8 4 DUP(0.0)
Resultado REAL8 0.0
```

## Máscara de valor absoluto

Para doubles, la máscara usada para apagar el bit de signo es:

```asm
AbsMask QWORD 07FFFFFFFFFFFFFFFh, 07FFFFFFFFFFFFFFFh, 07FFFFFFFFFFFFFFFh, 07FFFFFFFFFFFFFFFh
```

Aplicada con:

```asm
vandpd ymm3, ymm2, ymmword ptr [AbsMask]
```

Esto convierte cada lane en su valor absoluto sin usar ramas ni comparaciones.
