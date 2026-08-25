# Aula 1.4: Laços de Repetição: Automatizando Tarefas 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. Laço for: Para Quando Você Sabe Quantas Vezes](#1-lao-for-para-quando-voc-sabe-quantas-vezes)
  - [2. Laço while: Para Quando a Condição é o Que Importa](#2-lao-while-para-quando-a-condio--o-que-importa)
  - [3. Laço do-while: Executa Pelo Menos Uma Vez](#3-lao-do-while-executa-pelo-menos-uma-vez)
  - [4. break e continue: Controlando o Fluxo dos Laços](#4-break-e-continue-controlando-o-fluxo-dos-laos)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Fundamentos da Linguagem Java**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender a necessidade de laços de repetição para automatizar tarefas repetitivas.
- Utilizar o laço `for` para iterações com número conhecido de repetições.
- Empregar os laços `while` e `do-while` para iterações baseadas em condições.
- Aprender a usar `break` e `continue` para controlar o fluxo dos laços.

##  Conteúdo da Aula

Em programação, é muito comum precisarmos executar a mesma sequência de instruções várias vezes. Imagina ter que escrever `System.out.println()` 100 vezes para imprimir números de 1 a 100! Seria exaustivo e propenso a erros. É para isso que existem os **laços de repetição** (ou loops): eles nos permitem automatizar tarefas repetitivas de forma eficiente.

### 1. Laço `for`: Para Quando Você Sabe Quantas Vezes

O laço `for` é ideal quando você sabe de antemão quantas vezes um bloco de código precisa ser executado. Sua sintaxe é:

`for (inicialização; condição; atualização) { // código a ser repetido }`

*   **Inicialização**: Executada uma única vez no início do laço (ex: `int i = 0;`).
*   **Condição**: Avaliada antes de cada iteração. Se for `true`, o bloco de código é executado. Se for `false`, o laço termina (ex: `i < 10;`).
*   **Atualização**: Executada após cada iteração (ex: `i++`).

```java
public class ForLoopExemplo {
    public static void main(String[] args) {
        System.out.println("Contando de 0 a 4 com for:");
        for (int i = 0; i < 5; i++) {
            System.out.println("Número: " + i);
        }

        System.out.println("\nIterando sobre um array com for aprimorado (for-each):");
        String[] frutas = {"Maçã", "Banana", "Laranja"};
        for (String fruta : frutas) { // for-each
            System.out.println("Fruta: " + fruta);
        }
    }
}
```

### 2. Laço `while`: Para Quando a Condição é o Que Importa

O laço `while` é usado quando você quer repetir um bloco de código enquanto uma condição específica for verdadeira. O número de repetições não é necessariamente conhecido de antemão.

`while (condição) { // código a ser repetido }`

**Importante**: A condição é avaliada *antes* da execução do bloco. Se a condição for `false` desde o início, o bloco nunca será executado.

```java
public class WhileLoopExemplo {
    public static void main(String[] args) {
        int contador = 0;
        System.out.println("Contando de 0 a 4 com while:");
        while (contador < 5) {
            System.out.println("Contador: " + contador);
            contador++; // Não esqueça de atualizar a condição para evitar loop infinito!
        }
    }
}
```

### 3. Laço `do-while`: Executa Pelo Menos Uma Vez

O laço `do-while` é semelhante ao `while`, mas com uma diferença crucial: o bloco de código é executado **pelo menos uma vez** antes que a condição seja avaliada. A condição é verificada no final da primeira iteração.

`do { // código a ser repetido } while (condição);`

```java
import java.util.Scanner;

public class DoWhileLoopExemplo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int numero;

        do {
            System.out.print("Digite um número positivo: ");
            numero = scanner.nextInt();
        } while (numero <= 0);

        System.out.println("Você digitou: " + numero);
        scanner.close();
    }
}
```

### 4. `break` e `continue`: Controlando o Fluxo dos Laços

*   **`break`**: Interrompe completamente o laço de repetição mais interno e o programa continua a execução a partir da instrução que segue o laço.
*   **`continue`**: Pula a iteração atual do laço e passa para a próxima iteração (reavalia a condição do laço).

```java
public class BreakContinueExemplo {
    public static void main(String[] args) {
        System.out.println("\nExemplo com break (parar no 3):");
        for (int i = 0; i < 10; i++) {
            if (i == 3) {
                break; // Sai do loop quando i for 3
            }
            System.out.println(i);
        }

        System.out.println("\nExemplo com continue (pular o 3):");
        for (int i = 0; i < 5; i++) {
            if (i == 3) {
                continue; // Pula a iteração quando i for 3
            }
            System.out.println(i);
        }
    }
}
```

##  Desafio de Fixação

1.  Usando um laço `for`, imprima os números de 1 a 10.
2.  Usando um laço `while`, calcule a soma dos números de 1 a 50.
3.  Usando um laço `do-while`, peça ao usuário para digitar a senha "java123". Repita até que a senha correta seja digitada.
4.  Usando um laço `for` e a instrução `continue`, imprima os números de 1 a 10, mas pule a impressão do número 5.


## Referências

[1] Oracle. Java SE 21 API. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/](https://docs.oracle.com/en/java/javase/21/docs/api/)
[2] dev.java. Language Basics. Disponível em: [https://dev.java/learn/language-basics/](https://dev.java/learn/language-basics/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_1_3_estruturas_controle.md) | [Próxima aula/módulo](aula_1_5_metodos_funcoes.md)
