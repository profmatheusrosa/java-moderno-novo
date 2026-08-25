# Aula 1.5: Métodos e Funções: Organizando o Código 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O que são Métodos?](#1-o-que-so-mtodos)
  - [2. Métodos void (Sem Retorno)](#2-mtodos-void-sem-retorno)
  - [3. Métodos com Retorno](#3-mtodos-com-retorno)
  - [4. Sobrecarga de Métodos (Method Overloading)](#4-sobrecarga-de-mtodos-method-overloading)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Fundamentos da Linguagem Java**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o conceito de métodos (funções) e sua importância para a organização do código.
- Aprender a declarar e chamar métodos em Java.
- Entender a diferença entre métodos com e sem retorno.
- Utilizar parâmetros para passar informações para os métodos.
- Conhecer o conceito de sobrecarga de métodos (method overloading).

##  Conteúdo da Aula

À medida que nossos programas crescem, eles se tornam mais complexos. Escrever todo o código em um único bloco (`main`) torna-o difícil de ler, manter e reutilizar. É aí que entram os **métodos** (também conhecidos como funções em outras linguagens de programação). Métodos são blocos de código que realizam uma tarefa específica e podem ser chamados (executados) sempre que essa tarefa for necessária.

### 1. O que são Métodos?

Um **método** é uma coleção de instruções que executa uma operação. Ele encapsula uma lógica específica, tornando o código mais modular, legível e reutilizável. Em Java, todos os métodos devem pertencer a uma classe.

#### Estrutura Básica de um Método:

```java
modificadorDeAcesso tipoDeRetorno nomeDoMetodo(tipoParametro1 nomeParametro1, tipoParametro2 nomeParametro2, ...) {
    // Corpo do método: instruções a serem executadas
    // return valor; (se o tipoDeRetorno não for void)
}
```

*   **`modificadorDeAcesso`**: Define quem pode acessar o método (ex: `public`, `private`, `protected`).
*   **`tipoDeRetorno`**: O tipo de dado que o método retorna após sua execução. Se o método não retorna nada, usa-se `void`.
*   **`nomeDoMetodo`**: Um identificador para o método, seguindo as convenções de nomenclatura Java (camelCase, começando com letra minúscula).
*   **`parâmetros`**: Uma lista de variáveis que o método recebe como entrada. São opcionais.

### 2. Métodos `void` (Sem Retorno)

Métodos `void` executam uma tarefa, mas não retornam nenhum valor. Eles são usados para realizar ações, como imprimir algo na tela, modificar o estado de um objeto, etc.

```java
public class MetodosVoidExemplo {

    // Método que não retorna nada e não recebe parâmetros
    public static void saudacao() {
        System.out.println("Olá! Bem-vindo ao curso de Java.");
    }

    // Método que não retorna nada e recebe um parâmetro
    public static void imprimirMensagem(String mensagem) {
        System.out.println("Mensagem: " + mensagem);
    }

    public static void main(String[] args) {
        saudacao(); // Chamando o método saudacao
        imprimirMensagem("Esta é uma mensagem personalizada."); // Chamando o método imprimirMensagem
    }
}
```

### 3. Métodos com Retorno

Métodos com retorno executam uma tarefa e, ao final, devolvem um valor de um tipo específico (definido pelo `tipoDeRetorno`). A palavra-chave `return` é usada para enviar o valor de volta ao local onde o método foi chamado.

```java
public class MetodosComRetornoExemplo {

    // Método que retorna a soma de dois inteiros
    public static int somar(int a, int b) {
        int resultado = a + b;
        return resultado;
    }

    // Método que retorna o maior de dois números
    public static int encontrarMaior(int num1, int num2) {
        if (num1 > num2) {
            return num1;
        } else {
            return num2;
        }
    }

    public static void main(String[] args) {
        int resultadoSoma = somar(5, 3); // Chamando o método somar e armazenando o retorno
        System.out.println("A soma é: " + resultadoSoma); // Saída: A soma é: 8

        int maiorNumero = encontrarMaior(10, 7);
        System.out.println("O maior número é: " + maiorNumero); // Saída: O maior número é: 10
    }
}
```

### 4. Sobrecarga de Métodos (Method Overloading)

A **sobrecarga de métodos** permite que uma classe tenha múltiplos métodos com o mesmo nome, desde que eles tenham listas de parâmetros diferentes. As diferenças podem ser no número de parâmetros, no tipo dos parâmetros ou na ordem dos tipos dos parâmetros. O tipo de retorno *não* é suficiente para diferenciar métodos sobrecarregados.

```java
public class SobrecargaMetodosExemplo {

    // Método para somar dois inteiros
    public static int somar(int a, int b) {
        return a + b;
    }

    // Método sobrecarregado para somar três inteiros
    public static int somar(int a, int b, int c) {
        return a + b + c;
    }

    // Método sobrecarregado para somar dois doubles
    public static double somar(double a, double b) {
        return a + b;
    }

    public static void main(String[] args) {
        System.out.println("Soma de 2 inteiros: " + somar(2, 3)); // Chama o primeiro somar
        System.out.println("Soma de 3 inteiros: " + somar(1, 2, 3)); // Chama o segundo somar
        System.out.println("Soma de 2 doubles: " + somar(2.5, 3.5)); // Chama o terceiro somar
    }
}
```

##  Desafio de Fixação

1.  Crie um método `public static void saudacaoPersonalizada(String nome)` que imprima "Olá, [nome]! Bem-vindo(a)!". Chame este método com seu nome.
2.  Crie um método `public static double calcularMedia(double nota1, double nota2, double nota3)` que receba três notas e retorne a média aritmética delas. Imprima o resultado.
3.  Crie um método sobrecarregado `public static String formatarNome(String primeiroNome, String sobrenome)` que retorne o nome completo no formato "Sobrenome, PrimeiroNome".
4.  Crie outro método sobrecarregado `public static String formatarNome(String primeiroNome, String sobrenome, String doMeio)` que retorne o nome completo no formato "Sobrenome, PrimeiroNome NomeDoMeio".
5.  Teste ambos os métodos `formatarNome`.


## Referências

[1] Oracle. Java SE 21 API. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/](https://docs.oracle.com/en/java/javase/21/docs/api/)
[2] dev.java. Language Basics. Disponível em: [https://dev.java/learn/language-basics/](https://dev.java/learn/language-basics/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_1_4_lacos_repeticao.md) | [Próxima aula/módulo](../modulo_02_orientacao_a_objetos.md)
