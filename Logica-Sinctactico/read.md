# Logica Sintactico
Acá se define cómo se combinan los tokens del léxico para formar estructuras válidas del lenguaje: **bloques, sentencias, funciones, ciclos, etc.**
Yyyyy todo gira alrededor de los bloques { ... }. Es como el modulo principal sabes? jeje algo asi lo entiendo 

### **ProgramaCompleto() – Punto de entrada**
```bash
void ProgramaCompleto() : {}
{
   (Clase() | Bloque()) <EOF>
}
```
## Qué significa
Un programa válido puede ser:
* una clase, o
* un bloque de instrucciones
y debe terminar correctamente **(<EOF>)**.

--> Es la puerta de entrada del analizador.<--

### **⭐ void Bloque() – El núcleo del lenguaje**

```bash
void Bloque(): {}
{
  "{"
  (
      DeclaracionConstante()
    | IfSentencia()
    | ElifSentencia()
    | ElseSentencia()
    | Funcion()
    | ReturnSentencia()
    | WhileSentencia()
    | TrySentencia()
    | CatchSentencia()
    | OpenFile()
    | Display()
    | VarSentencia()
    | SwitchSentencia()
    | CaseSentencia()
    | BreakSentencia()
    | DefaultSentencia()
    | DoWhileSentencia()
    | Asignacion()
    | LlamadaStart()
  )*
  "}"
}
```
**Estructura**
Un bloque:
empieza con {
dentro puede haber cero o más sentencias
termina con }
_el orden no es fijo_
📌 El ***** significa:
_“pueden aparecer muchas o ninguna de estas instrucciones”_
👉 Esto permite escribir bloques reales como:
```bash
{
  var x = 10;
  if (x > 5) { ... }
  display(x);
}
```

### **Declaraciones y asignaciones (lo básico)**
## **Variables**

```bash
void VarSentencia(): {}
{
    "var" <IDENTIFICADOR> <OP_ASIG>
    (
        "[" ListaArgumentos() "]" ";"                    
      | "lambda" "(" ListaParametros() ")" "=>" Expresion() ";" 
      | Expresion() ";"                                   
    )
}
```
**📌 Permite declarar variables:**
* normales
* arreglos
* lambdas
**Ejemplos:**
  ```bash
  var x = 5;
  var arr = [1,2,3];
  var f = lambda(a,b) => a+b;
  ```
### **Asignación**
```bash
void Asignacion(): {}
{
  <IDENTIFICADOR> <OP_ASIG> ValorAsignable() ";" 
}
```
**📌 Permite modificar valores ya existentes:**
```bash
x = 10;
y += 5;
```

### **Control de flujo (decisiones y ciclos)**
## **Condicionales**
```bash
if (condición) { ... }
elif (condición) { ... }
else { ... }
```

**Cada uno:**
* evalúa una expresión
* ejecuta un bloque

## **Ciclos**
```bash
while (condición) { ... }
do { ... } while (condición);
```
_📌 Ejecutan bloques repetidamente según una condición._

### **Funciones y clases**
## **Funciones**
```bash
function suma(a,b) { ... }
```
* tienen nombre
* parámetros
* un bloque interno

## **Clases**
```bash
class MiClase {
   function f() { ... }
}
```
_📌 Una clase puede contener:_
* bloques
* funciones

### **Otras sentencias útiles**
| Sentencia                 | Función            |
| ------------------------- | ------------------ |
| `return`                  | Regresa un valor   |
| `break`                   | Sale de un bloque  |
| `switch / case / default` | Selección múltiple |
| `try / catch`             | Manejo de errores  |
| `display`                 | Mostrar salida     |
| `open File("x")`          | Abrir archivos     |
| `start(id)`               | Llamada inicial    |

## **Expresiones**
```bash
void Expresion(): {}
{ 
    <IDENTIFICADOR> | <NUMERO> | <STRING_LITERAL> | <CHAR> 
}
```

_📌 Una expresión puede ser:_
* una variable
* un número
* un texto
* un carácter

**Luego se extiende con:**
* operadores aritméticos
* lógicos
* relacionales

