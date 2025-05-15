# cLox Interpreter

Un intérprete del lenguaje Lox hecho en C.

## ¿Qué es Lox?

Lox es un lenguaje de programación dinámico, de propósito general, diseñado con fines educativos para ilustrar la construcción de intérpretes y máquinas virtuales. Es un lenguaje con soporte para funciones de primera clase y clausuras. Su sintaxis está inspirada en Python.

Lox fue diseñado por Robert Nystrom como parte del libro Crafting Interpreters.

### Gramática de Lox

El lenguaje Lox está regido por la siguiente gramática abstracta:

```
<program>        → <declaration>* <EOF>

<declaration>    → <funDecl>
                 | <varDecl>
                 | <statement>

<funDecl>        → "fun" <function>
<varDecl>        → "var" <IDENTIFIER> ( "=" <expression> )? ";"
<function>       → <IDENTIFIER> "(" <parameters>? ")" <block>
<parameters>     → <IDENTIFIER> ( "," <IDENTIFIER> )*

<statement>      → <exprStmt>
                 | <printStmt>
                 | <returnStmt>
                 | <ifStmt>
                 | <whileStmt>
                 | <forStmt>
                 | <block>

<exprStmt>       → <expression> ";"
<printStmt>      → "print" <expression> ";"
<returnStmt>     → "return" <expression>? ";"
<ifStmt>         → "if" "(" <expression> ")" <statement> ( "else" <statement> )?
<whileStmt>      → "while" "(" <expression> ")" <statement>
<forStmt>        → "for" "(" ( <varDecl> | <exprStmt> | ";" )
                           <expression>? ";"
                           <expression>? ")" <statement>
<block>          → "{" <declaration>* "}"

<expression>     → <assignment>

<assignment>     → ( <call> "." )? <IDENTIFIER> "=" <assignment>
                 | <logic_or>

<logic_or>       → <logic_and> ( "or" <logic_and> )*
<logic_and>      → <equality> ( "and" <equality> )*
<equality>       → <comparison> ( ( "!=" | "==" ) <comparison> )*
<comparison>     → <term> ( ( ">" | ">=" | "<" | "<=" ) <term> )*
<term>           → <factor> ( ( "-" | "+" ) <factor> )*
<factor>         → <unary> ( ( "/" | "*" ) <unary> )*
<unary>          → ( "!" | "-" ) <unary> | <call>
<call>           → <primary> ( "(" <arguments>? ")" | "." <IDENTIFIER> )*
<primary>        → "true" | "false" | "nil" | <NUMBER> | <STRING> | <IDENTIFIER>
                 | "(" <expression> ")"
```

## Implementación del Intérprete: Chunks de Bytecode

En esta implementación del lenguaje Lox, se propone un cambio fundamental respecto de lo requerido para el laboratorio de la materia: en lugar de ejecutar directamente el árbol de sintaxis abstracta (AST), se compila el código fuente a un formato intermedio denominado bytecode, que luego es ejecutado por una máquina virtual (VM) escrita en C.

El bytecode se organiza en estructuras llamadas chunks, que encapsulan tres componentes principales:

- Un arreglo dinámico de instrucciones (code), representadas como bytes (opcodes).

- Una tabla de constantes (constants) que almacena literales usados por el programa.

- Información de depuración (lines) que asocia cada instrucción con su línea de código fuente original.

Cada instrucción del bytecode consta de un opcode de un byte y, en algunos casos, uno o más operandos que permiten acceder a constantes u otras instrucciones. La ejecución del programa se realiza mediante un ciclo de despacho que lee cada instrucción y ejecuta la operación correspondiente dentro de la VM.

Este enfoque tiene varias ventajas: mejora significativamente la eficiencia en tiempo de ejecución, reduce el uso de memoria y permite mayor portabilidad al estar desarrollado en C.

## Compilación y ejecución

El intérprete de Lox se desarrolla en C y puede compilarse fácilmente utilizando el Makefile incluido en el proyecto.

### Compilación

Desde la raíz del projecto, ejecutar:

```bash
make
```

### Ejecución

El intérprete soporta dos modos de ejecución: interactivo o cargando archivos.

Para iniciar una consola interactiva donde ejecutar sentencias Lox, ejecutar:

```bash
./clox
```

Para ejecutar el contenido de un archivo `.lox` línea por línea, ejecutar:

```bash
./clox [script.lox]
```
