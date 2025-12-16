# Compilador-en-JAVACC-
La pase mal buscando como instalar javacc qwqU. Espero que al menos esto ayude en algo...Tampoco se como lo hice btw XD pero en la carpeta esta el javacc version 7.0.13 (no se en que version este cuando leas esto.

Ahora, como lo instale? no recuerdo bien qwq es borroso... 
1. Descargas el archivo ZIP y lo descomprimes
2. Ahora en este punto si no recuerdo mal deberias buscar javacc-7.0.13 y esa libreria la agregue en mi carpeta principal, en una sub-carpeta llamada "lib"
3. Y aqui pesh lo instalas, abres el proyecto en tu programa pa codigo (tipo visual studio y asi)
4. Agregas la carpeta en el programa y en la consola pones estos comandos:
   //ubicamos donde esta el proyecto
   cd C:\ruta de tu archivo\nombre de tu proyecto
   //Esto practicamente descarga el javacc
   curl -LO https://github.com/javacc/javacc/releases/download/javacc-7.0.13/javacc-7.0.13.jar
   //Comprobamos que esta el javacc uwu
   java -jar javacc-7.0.13.jar tu_proyecto.jj

   Ahora, no se bien si te sirva este mini tutorial raro qwqU
   Este es el codigo que me funcionó (me pelee mucho con la ia por semanas qwq espero ayude)

Esto pesh es toda la logica de lo que hara el codigo:

		   options {
		  STATIC = false;
		  UNICODE_INPUT = true;
		}
		
		PARSER_BEGIN(analizador)
		
		import java.io.*;
		
		public class analizador {
		    
		    public static void main(String[] args) {
		        analizador obj = new analizador(System.in); 
		
		        if (args.length > 0) {
		            System.out.println("Leyendo desde el archivo: " + args[0]);
		            try (InputStream inputStream = new FileInputStream(args[0])) {
		                obj.ReInit(inputStream); 
		                
		                obj.ProgramaCompleto();
		                
		                System.out.println("\tAnalisis terminado");
		                System.out.println("\tAnalisis exitoso");
		
		            } catch (ParseException e) {
		                System.out.println("\n*** ERROR DE SINTAXIS EN ARCHIVO ***");
		                System.out.println(e.getMessage());
		            } catch (Exception e) {
		                System.out.println("Error de I/O o general: " + e.getMessage());
		            }
		        } else {
		            System.out.println("Leyendo entrada estandar...");
		            try {
		                obj.Bloque(); 
		                
		                System.out.println("\tAnalisis terminado");
		                System.out.println("\tAnalisis exitoso!!! uwu");
		
		            } catch (ParseException e) {
		                // Captura el error de sintaxis detallado
		                System.out.println("\n*** ERROR DE SINTAXIS EN ENTRADA ESTÁNDAR ***");
		                System.out.println(e.getMessage());
		            } catch (Exception e) {
		                System.out.println("Error general: " + e.getMessage());
		            }
		        }
		    }
		    
		}
		
		PARSER_END(analizador) //aqui finaliza la logica para dar paso al la parte del lexico

Esta parte es el lexico (recomiendo que lo uses de referencia, ya que es como personalizado y asi jeje), si estas haciendo esto supongo que sabes mas que yo que es un Token jejeje
	/* LÉXICO */
	
	SKIP :
	{
	    " " | "\t" | "\n" | "\r"
	|   < SINGLE_LINE_COMMENT: "//" (~["\n","\r"])* >
	}
	
	
	
	/* ----------- OPERADORES & SIMBOLOS ---------- */
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
	
	/* ----------- PALABRAS RESERVADAS ----------- */
	TOKEN : {
	   < IF: "if" > | < ELSE: "else" >
	 | < ELIF: "elif" >
	 | < WHILE: "while" >
	 | < FOR: "for" >
	 | < CLASS: "class" >
	 | < PUBLIC: "public" >
	 | < PRIVATE: "private" >
	 | < STATIC_WORD: "static" >
	 | < VOID: "void" >
	 | < TYPE_INT: "int" >
	 | < TYPE_FLOAT: "float" >
	 | < TYPE_STRING: "string" >
	 | < TYPE_BOOL: "bool" >
	 | < TRUE: "true" > | < FALSE: "false" >
	 | < RETURN: "return" >
	 | < CONST: "const" >
	 | < VAR: "var" >
	 | < SWITCH: "switch" >
	 | < CASE: "case" >
	 | < BREAK: "break" >
	 | < CONTINUE: "continue" >
	 | < DEFAULT_WORD: "default" >
	 | < DO: "do" >
	 | < FUNCTION: "function" >
	 | < TRY: "try" > | < CATCH: "catch" > | < FINALLY: "finally" >
	 | < OPEN: "open" > | < FILE_WORD: "File" >
	 | < DISPLAY: "display" >
	 | < LAMBDA: "lambda" >
	 | < START: "start" >
	}
	
	
	TOKEN :
	{
	    < IDENTIFICADOR: ["a"-"z","A"-"Z","_"] (["a"-"z","A"-"Z","0"-"9","_"])* >
	|   < NUMERO: (["0"-"9"])+ ( "." (["0"-"9"])+ )? >
	|   < STRING_LITERAL: "\"" (~["\""])* "\"" >
	|   < CHAR: "'" (~["'"])* "'" >
	}
Por ultimo tenemos la parte sintactica, osea la gramatica jeje. Esto es lo que le dice al compilador las reglas de nuestro lenguaje.
De nuevo, te recomuendo usarlo mas de referencia jeje

	/* SINTÁCTICO */
	
	void ProgramaCompleto() : {}
	{
	   (Clase() | Bloque()) <EOF>
	}
	
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
	void VarSentencia(): {}
	{
	    "var" <IDENTIFICADOR> <OP_ASIG>
	    (
	        "[" ListaArgumentos() "]" ";"                    
	      | "lambda" "(" ListaParametros() ")" "=>" Expresion() ";" 
	      | Expresion() ";"                                   
	    )
	}
	void IdentSentencia(): {}
	{
	    <IDENTIFICADOR> <OP_ASIG> Expresion() ";"
	}
	
	/************* PRODUCCIONES *************/
	
	void DeclaracionConstante() : {}
	{ "const" <IDENTIFICADOR> <OP_ASIG> <NUMERO> ";" { System.out.println("Declaracion de constante"); } }
	
	void IfSentencia(): {}
	{ "if" "(" Expresion() ")" Bloque() { System.out.println("Sentencia IF"); } }
	
	void ElifSentencia(): {}
	{ "elif" "(" Expresion() ")" Bloque() { System.out.println("Sentencia ELIF"); } }
	
	void ElseSentencia(): {}
	{ "else" Bloque() { System.out.println("Sentencia ELSE"); } }
	
	void Funcion(): {}
	{ "function" <IDENTIFICADOR> "(" ListaParametros() ")" Bloque() { System.out.println("Declaracion de funcion"); } }
	
	void ReturnSentencia(): {}
	{ "return" Expresion() ";" { System.out.println("Return encontrado"); } }
	
	void WhileSentencia(): {}
	{ "while" "(" Expresion() ")" Bloque() { System.out.println("Sentencia WHILE"); } }
	
	void TrySentencia(): {}
	{ "try" Bloque() { System.out.println("TRY"); } }
	
	void CatchSentencia(): {}
	{ "catch" Bloque() { System.out.println("CATCH"); } }
	
	void OpenFile(): {}
	{ "open" "File" <LPAREN> <STRING_LITERAL> <RPAREN> ";" { System.out.println("Llamada a File.open"); } }
	void ArrayDecl(): {}
	
	{ "var" <IDENTIFICADOR> <OP_ASIG> "[" ListaArgumentos() "]" ";" { System.out.println("Declaración de arreglo"); } }
	
	void Display(): {}
	{ "display" "(" Expresion() ")" ";" { System.out.println("Display llamado"); } }
	
	void LambdaExp(): {}
	{ "var" <IDENTIFICADOR> <OP_ASIG> "lambda" "(" ListaParametros() ")" "=>" Expresion() ";" { System.out.println("Lambda 			encontrada"); } }
	
	void SwitchSentencia() : {}
	{ "switch" "(" Expresion() ")" "{" (CaseSentencia())* DefaultSentencia() "}" { System.out.println("Switch encontrado"); } }
	
	void CaseSentencia(): {}
	{ "case" Expresion() ":" Bloque() { System.out.println("Case"); } }
	
	void BreakSentencia(): {}
	{ "break" ";" { System.out.println("Break"); } }
	
	void DefaultSentencia(): {}
	{ "default" ":" Bloque() { System.out.println("Default"); } }
	
	void DoWhileSentencia(): {}
	{ "do" Bloque() "while" "(" Expresion() ")" ";" { System.out.println("DO-WHILE"); } }
	
	void Asignacion(): {}
	{ <IDENTIFICADOR> <OP_ASIG> ValorAsignable() ";" 
	    { System.out.println("Asignacion compuesta"); }
	}
	void LlamadaStart(): {}
	{ "start" "(" <IDENTIFICADOR> ")" ";" { System.out.println("Llamada a start"); } }
	
	void ListaParametros(): {}
	{ ( <IDENTIFICADOR> ( "," <IDENTIFICADOR> )* )? }
	
	void ListaArgumentos(): {}
	{ ( Expresion() ( "," Expresion() )* )? }
	
	void Expresion(): {}
	{ 
	    <IDENTIFICADOR> | <NUMERO> | <STRING_LITERAL> | <CHAR> 
	}
	
	void ExpresionAritmetica() : {}
	{
	    ( <IDENTIFICADOR> | <NUMERO> ) 
	    ( <OP_ARIT> ( <IDENTIFICADOR> | <NUMERO> ) )* }
	
	void ValorAsignable() : {}
	{
	    ValorAritmetico()
	    | <STRING_LITERAL>
	    | <CHAR>
	    | <TRUE>
	    | <FALSE>
	}
	
	    void ValorAritmetico() : {}
	{
	    // Acepta un Identificador/Número seguido de CERO o más operadores y términos.
	    ( <IDENTIFICADOR> | <NUMERO> ) 
	    ( ( <OP_ARIT> | <OP_BIT> | <OP_REL> | <OP_LOG> ) ( <IDENTIFICADOR> | <NUMERO> ) )*
	}   
	
	void Clase() : {}
	{
	    "class" <IDENTIFICADOR> "{" (Bloque() | Funcion())* "}"
	    { System.out.println("Clase detectada"); }
	}

Ahora, se deben generar los archivos .java y .class
Para eso usaremos estos comandos:
```bash
# Este para los archivos .java
java -cp .\lib\javacc-7.0.13.jar javacc tu_proyecto.jj

# Este para tus archivos .class
javac *.java

# Y con este comando ejecutas tus códigos de prueba
java tu_proyecto tu_archivo.txt
```

 
   
