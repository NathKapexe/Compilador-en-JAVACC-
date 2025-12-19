# Lógica del análisis léxico
En esta parte el compilador se encarfa de leer carácter por carácter la entrada y agruparlo en tokens, que luego seran usados por el analizador sintactico.
Osea, esta parte del codigo es la gramatica de tu lenguaje.

Ahora, mi lexico esta dividido en 4 partes:

## 1. Ignorar espacios y comentarios

  ```bash
    SKIP :
  {
      " " | "\t" | "\n" | "\r"
  |   < SINGLE_LINE_COMMENT: "//" (~["\n","\r"])* >
  }
  ```
### ¿Qué hace?

Este bloque indica qué elementos no deben generar tokens y por lo tanto se ignoran durante el análisis.

### Elementos ignorados:

* Espacio " "
* Tabulación \t
* Salto de línea \n
* Retorno de carro \r

Comentarios de una sola línea:
```bash
// comentario
```

### Por qué es importante:

* Evita que el parser tenga que manejar espacios manualmente.
* Permite escribir código legible sin afectar la gramática.
* Los comentarios no influyen en la estructura del programa.

## 2. Operadores y símbolos
```bash
TOKEN : {
  < OP_ASIG: "=" | "+=" | "-=" | "*=" | "/=" | "%=" | "<<=" | ">>=" | "//=" >
| < OP_REL: "==" | "!=" | ">=" | "<=" | ">" | "<" >
| < OP_LOG: "&&" | "||" | "!" >
| < OP_ARIT: "+" | "-" | "*" | "/" | "%" | "**" | "*" >
| < OP_BIT: "&" | "|" | "~" | "<<" | ">>" >
| < LPAREN: "(" > | < RPAREN: ")" >
| < LBRACK: "[" > | < RBRACK: "]" >
| < LBRACE: "{" > | < RBRACE: "}" >
| < COMA: "," > | < PYC: ";" >
| < DOSP: ":" > | < DOT: "." >
| < INTERROG: "?" > | < DOLAR: "$" > | < TILDE: "`" >
}
```

### ¿Qué hace?

Define todos los símbolos especiales y operadores válidos en tu lenguaje.

### Agrupación lógica:

* **Asignación (OP_ASIG):** modificar valores
* **Relacionales (OP_REL):** comparaciones
* **Lógicos (OP_LOG):** condiciones booleanas
* **Aritméticos (OP_ARIT):** operaciones matemáticas
* **Bit a bit (OP_BIT):** operaciones a nivel binario
* **Delimitadores:** paréntesis, llaves, corchetes
* **Separadores:** coma, punto y coma
* **Símbolos especiales:** `? $ ``

### Por qué es importante:

El parser no ve símbolos sueltos, ve tokens con significado.

Facilita reglas como:
```bash
Expresion ::= Expresion OP_ARIT Expresion
```

# 3. Palabras reservadas
```bash
TOKEN : {
   < IF: "if" > | < ELSE: "else" >
 | < ELIF: "elif" >
 | < WHILE: "while" >
 | < FOR: "for" >
 | < CLASS: "class" >
 ...
 | < START: "start" >
}
```

### ¿Qué hace?

Define las palabras clave del lenguaje, que:
* Tienen significado fijo
* No pueden usarse como identificadores

### Tipos de palabras reservadas:

* **Control de flujo:** if, else, while, for, switch
* **Estructura:** class, function
* **Modificadores:** public, private, static
* **Tipos de datos:** int, float, string, bool
* **Manejo de errores:** try, catch, finally
* **Otros:** return, break, continue, lambda, start

### Por qué es importante:
**Evita ambigüedad:**
 ```bash
if = 3   ❌
```
**Permite reglas sintácticas claras:**
```bash
IfStmt ::= IF "(" Expresion ")" Bloque
```

# 4. Identificadores y literales
```bash
TOKEN :
{
    < IDENTIFICADOR: ["a"-"z","A"-"Z","_"] (["a"-"z","A"-"Z","0"-"9","_"])* >
|   < NUMERO: (["0"-"9"])+ ( "." (["0"-"9"])+ )? >
|   < STRING_LITERAL: "\"" (~["\""])* "\"" >
|   < CHAR: "'" (~["'"])* "'" >
}
```

### **Identificador**
```bash
< IDENTIFICADOR: ["a"-"z","A"-"Z","_"] (["a"-"z","A"-"Z","0"-"9","_"])* >
```

**Representa:**
* nombres de variables
* funciones
* clases

**Reglas:**
* No puede empezar con número
* Puede contener letras, números y **_**
**Ejemplo:**
```bash
contador
_total
funcion123
```
### **Número**
```bash
< NUMERO: (["0"-"9"])+ ( "." (["0"-"9"])+ )? >
```
**Acepta:**

* Enteros: 10
* Decimales: 3.14

_El punto decimal es opcional._

### **String literal**
```bash
< STRING_LITERAL: "\"" (~["\""])* "\"" >
```
**Texto entre comillas dobles:**
```bash
"Hola mundo"
```
_**~["\""]** evita cerrar el string antes de tiempo._

### **Char**
```bash
< CHAR: "'" (~["'"])* "'" >
```
**Un carácter entre comillas simples:**
```bash
'a'
```

**Esepero te ayude a entender qwqU**
