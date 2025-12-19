# Logica del programa


🔹 Bloque de opciones de JavaCC

```bash
options {
    STATIC = false;
    UNICODE_INPUT = true;
}
```
->STATIC = false;
* Indica que el parser NO será estático.
* Permite crear instancias del analizador (new analizador(...)).
* Es necesario cuando quieres usar ReInit() y manejar múltiples entradas.

->UNICODE_INPUT = true;
* Permite que el analizador acepte caracteres Unicode.
* Útil si el lenguaje permite acentos, ñ, símbolos especiales, etc.

🔹 Inicio del parser
```bash
PARSER_BEGIN(analizador)
```
->Marca el inicio del código Java que irá dentro del parser.
* **analizador** será:
* El nombre de la clase Java generada
* El nombre del parser

🔹 Importaciones
```bash
import java.io.*;
```
->Importa clases de entrada/salida:
* InputStream
* FileInputStream
* Necesarias para leer archivos y entrada estándar.

🔹 Declaración de la clase
```bash
public class analizador {
```
* Define la clase principal del parser.
* JavaCC genera automáticamente el constructor:
```bash
analizador(InputStream in)
```

🔹 Método main
```bash
public static void main(String[] args) {
```
* Punto de entrada del programa.
* **args** permite pasar el nombre de un archivo desde la consola.

🔹Creación del analizador
```bash
analizador obj = new analizador(System.in);
```
* Se crea una instancia del parser.
* Inicialmente usa System.in (entrada estándar).
* Luego puede cambiarse con ReInit() si se usa un archivo.

🔹Verificación de argumentos
  ```bash
  if (args.length > 0) {
  ```
  * Verifica si el usuario pasó un archivo como argumento:
  ```bash 
  java analizador archivo.txt
```
🔹 Modo archivo
```bash 
System.out.println("Leyendo desde el archivo: " + args[0]);
```
* Mensaje informativo indicando qué archivo se va a analizar.
```bash 
try (InputStream inputStream = new FileInputStream(args[0])) {
```
-> Abre el archivo usando un **InputStream.**
```bash
try-with-resources:
```
-> Cierra automáticamente el archivo al terminar.
```bash
obj.ReInit(inputStream);
```
-> Reinicializa el parser para que ahora lea desde el archivo.
Esto es posible gracias a **STATIC = false.**

```bash 
obj.ProgramaCompleto();
```
* Llama a la regla inicial de la gramática.
* Aquí comienza el análisis sintáctico completo del lenguaje.


🔹 Manejo de errores (modo archivo)
```bash
} catch (ParseException e) {
```
* Captura errores de sintaxis generados por JavaCC.
* Ocurre cuando la entrada no cumple la gramática.

```bash
System.out.println("\n*** ERROR DE SINTAXIS EN ARCHIVO ***");
System.out.println(e.getMessage());
```
-> Muestra un mensaje claro y el detalle del error:
* línea
* columna
* token inesperado

```bash
 } catch (Exception e) {
    System.out.println("Error de I/O o general: " + e.getMessage());
}
```

🔹 Modo entrada estándar
```bash
} else {
    System.out.println("Leyendo entrada estandar...");
```
* Se ejecuta si NO se pasó archivo.
* El usuario escribe directamente en consola.
-> Captura errores generales:
* archivo no encontrado
* problemas de lectura
* errores inesperados

```bash
obj.Bloque();
```
->Llama a otra regla de la gramática.
->Normalmente usada para:
* pruebas
* análisis parcial
->bloques interactivos
  * java
  * Copiar código
  * System

🔹 Manejo de errores (entrada estándar)
```bash
} catch (ParseException e) {
```
->Captura errores sintácticos al escribir directamente en consola.
```bash
System.out.println("\n*** ERROR DE SINTAXIS EN ENTRADA ESTÁNDAR ***");
System.out.println(e.getMessage());
```
->Muestra el error detallado del parser.
```bash
} catch (Exception e) {
    System.out.println("Error general: " + e.getMessage());
}
```
->Captura cualquier otro error.

🔹 Fin de la clase y del parser
```bash
}

PARSER_END(analizador)
```
* Indica a JavaCC que aquí termina el código Java manual.
* A partir de aquí JavaCC genera el resto automáticamente.
