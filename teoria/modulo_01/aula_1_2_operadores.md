# Aula 1.2: Operadores: A Matemática e a Lógica do Java

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. Operadores Aritméticos](#1-operadores-aritmticos)
  - [2. Operadores de Atribuição](#2-operadores-de-atribuio)
  - [3. Operadores Unários](#3-operadores-unrios)
  - [4. Operadores Relacionais](#4-operadores-relacionais)
  - [5. Operadores Lógicos](#5-operadores-lgicos)
  - [6. Operador Ternário (Condicional)](#6-operador-ternrio-condicional)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Fundamentos da Linguagem Java**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender os diferentes tipos de operadores em Java: aritméticos, relacionais, lógicos, de atribuição e unários.
- Utilizar operadores para realizar cálculos e comparações em programas Java.
- Entender a precedência e associatividade dos operadores.

##  Conteúdo da Aula

Operadores são símbolos especiais que realizam operações em uma ou mais variáveis (operandos) e retornam um resultado. Eles são a base para qualquer cálculo, comparação ou manipulação de dados em Java.

### 1. Operadores Aritméticos

Usados para realizar operações matemáticas básicas:

| Operador | Descrição      | Exemplo      |
| :------- | :------------- | :----------- |
| `+`      | Adição         | `a + b`      |
| `-`      | Subtração      | `a - b`      |
| `*`      | Multiplicação  | `a * b`      |
| `/`      | Divisão        | `a / b`      |
| `%`      | Módulo (resto) | `a % b`      |

```java
public class OperadoresAritmeticos {
    public static void main(String[] args) {
        int a = 10;
        int b = 3;

        System.out.println("a + b = " + (a + b)); // 13
        System.out.println("a - b = " + (a - b)); // 7
        System.out.println("a * b = " + (a * b)); // 30
        System.out.println("a / b = " + (a / b)); // 3 (divisão inteira)
        System.out.println("a % b = " + (a % b)); // 1 (resto da divisão)

        double x = 10.0;
        double y = 3.0;
        System.out.println("x / y = " + (x / y)); // 3.333...
    }
}
```

### 2. Operadores de Atribuição

Usados para atribuir valores a variáveis. O operador de atribuição básico é `=`. Existem também operadores de atribuição compostos:

| Operador | Exemplo   | Equivalente a |
| :------- | :-------- | :------------ |
| `=`      | `a = 5`   | `a = 5`       |
| `+=`     | `a += 5`  | `a = a + 5`   |
| `-=`     | `a -= 5`  | `a = a - 5`   |
| `*=`     | `a *= 5`  | `a = a * 5`   |
| `/=`     | `a /= 5`  | `a = a / 5`   |
| `%=`     | `a %= 5`  | `a = a % 5`   |

```java
public class OperadoresAtribuicao {
    public static void main(String[] args) {
        int valor = 10;
        System.out.println("Valor inicial: " + valor); // 10

        valor += 5; // valor = valor + 5; -> 15
        System.out.println("Valor depois de += 5: " + valor); // 15

        valor -= 3; // valor = valor - 3; -> 12
        System.out.println("Valor depois de -= 3: " + valor); // 12
    }
}
```

### 3. Operadores Unários

Operam em um único operando:

| Operador | Descrição                               | Exemplo      |
| :------- | :-------------------------------------- | :----------- |
| `+`      | Sinal positivo (redundante)             | `+a`         |
| `-`      | Negação (inverte o sinal)               | `-a`         |
| `++`     | Incremento (adiciona 1)                 | `a++` ou `++a`|
| `--`     | Decremento (subtrai 1)                  | `a--` ou `--a`|
| `!`      | Negação lógica (inverte boolean)        | `!true`      |

**Incremento/Decremento (Prefixado vs. Pós-fixado)**:

*   **Pós-fixado (`a++`, `a--`)**: O valor original da variável é usado na expressão e *depois* a variável é incrementada/decrementada.
*   **Prefixado (`++a`, `--a`)**: A variável é incrementada/decrementada *primeiro* e *depois* o novo valor é usado na expressão.

```java
public class OperadoresUnarios {
    public static void main(String[] args) {
        int i = 5;
        int j = i++; // j recebe 5, depois i vira 6
        System.out.println("i: " + i + ", j: " + j); // i: 6, j: 5

        int x = 5;
        int y = ++x; // x vira 6, depois y recebe 6
        System.out.println("x: " + x + ", y: " + y); // x: 6, y: 6

        boolean verdade = true;
        System.out.println("!verdade: " + !verdade); // false
    }
}
```

### 4. Operadores Relacionais

Usados para comparar dois valores e sempre retornam um valor booleano (`true` ou `false`):

| Operador | Descrição                | Exemplo      |
| :------- | :----------------------- | :----------- |
| `==`     | Igual a                  | `a == b`     |
| `!=`     | Diferente de             | `a != b`     |
| `>`      | Maior que                | `a > b`      |
| `<`      | Menor que                | `a < b`      |
| `>=`     | Maior ou igual a         | `a >= b`     |
| `<=`     | Menor ou igual a         | `a <= b`     |

```java
public class OperadoresRelacionais {
    public static void main(String[] args) {
        int num1 = 10;
        int num2 = 20;

        System.out.println("num1 == num2: " + (num1 == num2)); // false
        System.out.println("num1 != num2: " + (num1 != num2)); // true
        System.out.println("num1 > num2: " + (num1 > num2));   // false
        System.out.println("num1 < num2: " + (num1 < num2));   // true
        System.out.println("num1 >= 10: " + (num1 >= 10));   // true
        System.out.println("num2 <= 20: " + (num2 <= 20));   // true
    }
}
```

### 5. Operadores Lógicos

Usados para combinar expressões booleanas e sempre retornam um valor booleano:

| Operador | Descrição | Exemplo         |
| :------- | :-------- | :-------------- |
| `&&`     | AND lógico| `(a > 0) && (b < 10)` |
| `||`     | OR lógico | `(a > 0) || (b < 10)` |
| `!`      | NOT lógico| `!(a > 0)`      |

```java
public class OperadoresLogicos {
    public static void main(String[] args) {
        boolean condicao1 = true;
        boolean condicao2 = false;

        System.out.println("condicao1 && condicao2: " + (condicao1 && condicao2)); // false
        System.out.println("condicao1 || condicao2: " + (condicao1 || condicao2)); // true
        System.out.println("!condicao1: " + (!condicao1)); // false
    }
}
```

### 6. Operador Ternário (Condicional)

É um atalho para uma instrução `if-else` simples. Tem a forma `condicao ? valorSeVerdadeiro : valorSeFalso;`

```java
public class OperadorTernario {
    public static void main(String[] args) {
        int idade = 18;
        String status = (idade >= 18) ? "Adulto" : "Menor de idade";
        System.out.println("Status: " + status); // Adulto
    }
}
```

##  Desafio de Fixação

1.  Declare duas variáveis inteiras, `numA` e `numB`, e atribua valores a elas.
2.  Calcule e imprima a soma, subtração, multiplicação, divisão e o resto da divisão entre `numA` e `numB`.
3.  Verifique se `numA` é maior que `numB` e imprima o resultado booleano.
4.  Verifique se `numA` é par E `numB` é ímpar, imprimindo o resultado.
5.  Use o operador ternário para determinar se a soma de `numA` e `numB` é maior que 50, imprimindo "Maior que 50" ou "Menor ou igual a 50".


## Referências

[1] Oracle. Java SE 21 API. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/](https://docs.oracle.com/en/java/javase/21/docs/api/)
[2] dev.java. Language Basics. Disponível em: [https://dev.java/learn/language-basics/](https://dev.java/learn/language-basics/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_1_1_variaveis_tipos.md) | [Próxima aula/módulo](aula_1_3_estruturas_controle.md)
