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
