# Configuración en Visual Studio

La implementación actual usa `printf` y `scanf` del CRT para simplificar la entrada/salida de números de punto flotante.

## Pasos recomendados

1. Abrir Visual Studio.
2. Crear un proyecto de tipo **C++ Empty Project** o **Console App**.
3. Seleccionar plataforma **x64**.
4. Activar MASM:
   - Click derecho sobre el proyecto.
   - `Build Dependencies` / `Build Customizations`.
   - Marcar `masm`.
5. Agregar el archivo:

```text
../src/main.asm
```

6. Verificar que el archivo `.asm` se compile con MASM.
7. Compilar y ejecutar.

## Importante sobre el Entry Point

En los ejercicios de clase se configuraba a veces `main` como punto de entrada directo. Para esta versión, no se recomienda hacerlo, porque el programa usa `printf` y `scanf` del CRT.

Dejar el punto de entrada por defecto permite que el runtime de C inicialice lo necesario y luego llame a `main`.

En resumen:

```text
Linker > Advanced > Entry Point: dejar vacío
```

## Si aparece error con msvcrt.lib

La fuente incluye:

```asm
includelib msvcrt.lib
```

Si Visual Studio no encuentra esa biblioteca, se puede probar una de estas opciones:

1. Revisar que el proyecto esté usando toolset de Visual Studio C++ instalado.
2. Agregar `msvcrt.lib` en:

```text
Linker > Input > Additional Dependencies
```

3. Si el entorno usa UCRT, se puede adaptar a `ucrt.lib` y `legacy_stdio_definitions.lib`.

Antes de cambiar bibliotecas, conviene probar primero con `msvcrt.lib`, porque suele estar disponible en el SDK de Windows.
