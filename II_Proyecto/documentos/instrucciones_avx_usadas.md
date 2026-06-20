# Instrucciones AVX usadas

Este documento resume las instrucciones relevantes usadas en `src/main.asm`.

## `vmovupd`

Carga o guarda doubles empaquetados sin requerir alineamiento estricto.

```asm
vmovupd ymm0, ymmword ptr [MatrizA]
vmovupd ymmword ptr [MatrizD], ymm2
```

Se usa para cargar filas completas de la matriz y para guardar resultados.

## `vsubpd`

Resta doubles empaquetados por lane.

```asm
vsubpd ymm2, ymm0, ymm1
```

Si:

```text
ymm0 = [a1, a2, a3, 0]
ymm1 = [b1, b2, b3, 0]
```

Entonces:

```text
ymm2 = [a1-b1, a2-b2, a3-b3, 0]
```

Esta instrucción cumple el requisito de usar aritmética empacada AVX para la diferencia matricial.

## `vandpd`

Realiza AND bit a bit sobre doubles empaquetados.

```asm
vandpd ymm3, ymm2, ymmword ptr [AbsMask]
```

Se usa para calcular el valor absoluto de cada diferencia. La máscara `0x7FFFFFFFFFFFFFFF` apaga el bit de signo de cada double.

Esta instrucción cumple el requisito de usar `AND` empacado para el valor absoluto.

## `vmulpd`

Multiplica doubles empaquetados por lane.

```asm
vmulpd ymm3, ymm3, ymm3
```

Después del valor absoluto, esta instrucción calcula el cuadrado de cada elemento.

## `vaddpd`

Suma doubles empaquetados por lane.

```asm
vaddpd ymm6, ymm6, ymm3
```

Se usa para acumular los cuadrados de las tres filas.

## `vsqrtsd`

Calcula la raíz cuadrada de un double escalar.

```asm
vsqrtsd xmm0, xmm0, xmm0
```

Se usa al final, después de reducir la suma total de cuadrados.

## `vzeroupper`

Limpia la parte alta de los registros YMM antes de volver a llamadas externas como `printf`.

```asm
vzeroupper
```

No cambia el resultado matemático, pero evita penalizaciones al mezclar AVX con código externo que puede usar SSE.
