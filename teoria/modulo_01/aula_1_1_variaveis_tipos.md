# Aula 1.1: Variáveis e Tipos de Dados: Construindo Blocos 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O que são Variáveis?](#1-o-que-so-variveis)
  - [2. Tipos de Dados Primitivos](#2-tipos-de-dados-primitivos)
  - [3. Tipos de Dados Wrapper](#3-tipos-de-dados-wrapper)
  - [4. Constantes (final)](#4-constantes-final)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Fundamentos da Linguagem Java**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o conceito de variáveis e sua importância na programação.
- Declarar e inicializar variáveis em Java.
- Diferenciar os tipos de dados primitivos (int, double, boolean, char, etc.) e seus usos.
- Entender o conceito de tipos de dados Wrapper e quando utilizá-los.

##  Conteúdo da Aula

No mundo da programação, precisamos de um lugar para armazenar informações que nosso programa irá manipular. É aí que entram as **variáveis**. Pense nelas como caixas rotuladas onde você pode guardar diferentes tipos de dados. Cada caixa tem um nome (o nome da variável) e um tipo (o tipo de dado que ela pode armazenar).

### 1. O que são Variáveis?

Uma **variável** é um espaço na memória do computador reservado para armazenar um valor. Esse valor pode mudar (variar) durante a execução do programa. Para usar uma variável, você precisa:

1.  **Declarar**: Dar um nome e especificar o tipo de dado que ela irá armazenar.
2.  **Inicializar**: Atribuir um valor inicial a ela.

```java
// Declaração de uma variável do tipo inteiro chamada 'idade'
int idade;

// Inicialização da variável 'idade' com o valor 30
idade = 30;

// Declaração e inicialização na mesma linha
String nome = "Alice";
double altura = 1.75;
boolean estaAtivo = true;
```

### 2. Tipos de Dados Primitivos

Java é uma linguagem **fortemente tipada**, o que significa que toda variável deve ter um tipo de dado definido. Os tipos de dados primitivos são os blocos de construção básicos e armazenam valores simples diretamente na memória. Existem 8 tipos primitivos em Java:

| Tipo    | Descrição                                  | Tamanho (bits) | Valor Padrão | Exemplo         |
| :------ | :----------------------------------------- | :------------- | :----------- | :-------------- |
| `byte`  | Inteiro muito pequeno                      | 8              | 0            | `byte b = 10;`  |
| `short` | Inteiro pequeno                            | 16             | 0            | `short s = 100;`|
| `int`   | Inteiro (mais comum)                       | 32             | 0            | `int i = 1000;` |
| `long`  | Inteiro grande (sufixo `L` ou `l`)         | 64             | 0L           | `long l = 10000000000L;` |
| `float` | Ponto flutuante de precisão simples (sufixo `F` ou `f`) | 32             | 0.0f         | `float f = 10.5f;`|
| `double`| Ponto flutuante de precisão dupla (mais comum) | 64             | 0.0d         | `double d = 10.5;`|
| `boolean`| Valores lógicos (verdadeiro/falso)         | 1              | false        | `boolean b = true;`|
| `char`  | Caractere único (entre aspas simples)     | 16             | '\u0000'     | `char c = 'A';` |

```java
public class TiposPrimitivos {
    public static void main(String[] args) {
        int numeroInteiro = 100;
        double numeroDecimal = 25.5;
        boolean isJavaFun = true;
        char primeiraLetra = 'J';

        System.out.println("Número Inteiro: " + numeroInteiro);
        System.out.println("Número Decimal: " + numeroDecimal);
        System.out.println("Java é divertido? " + isJavaFun);
        System.out.println("Primeira Letra: " + primeiraLetra);
    }
}
```

### 3. Tipos de Dados Wrapper

Para cada tipo primitivo, existe uma classe correspondente no pacote `java.lang` chamada **Wrapper Class**. Essas classes permitem tratar os valores primitivos como objetos. Isso é útil em situações onde apenas objetos podem ser usados (ex: coleções como `ArrayList`, que armazenam objetos, não primitivos).

| Primitivo | Wrapper Class |
| :-------- | :------------ |
| `byte`    | `Byte`        |
| `short`   | `Short`       |
| `int`     | `Integer`     |
| `long`    | `Long`        |
| `float`   | `Float`       |
| `double`  | `Double`      |
| `boolean` | `Boolean`     |
| `char`    | `Character`   |

Java realiza automaticamente a conversão entre primitivos e Wrappers (autoboxing e unboxing):

```java
import java.util.ArrayList;
import java.util.List;

public class TiposWrapper {
    public static void main(String[] args) {
        Integer idadeObjeto = 30; // Autoboxing: int para Integer
        int idadePrimitiva = idadeObjeto; // Unboxing: Integer para int

        System.out.println("Idade Objeto: " + idadeObjeto);
        System.out.println("Idade Primitiva: " + idadePrimitiva);

        // Exemplo de uso em coleção
        List<Integer> listaDeNumeros = new ArrayList<>();
        listaDeNumeros.add(10); // Autoboxing
        listaDeNumeros.add(20);

        int primeiroNumero = listaDeNumeros.get(0); // Unboxing
        System.out.println("Primeiro número da lista: " + primeiroNumero);
    }
}
```

### 4. Constantes (`final`)

Se você tem um valor que não deve mudar durante a execução do programa, você pode declará-lo como uma **constante** usando a palavra-chave `final`. Por convenção, nomes de constantes são escritos em MAIÚSCULAS, com palavras separadas por `_`.

```java
public class Constantes {
    public static void main(String[] args) {
        final double PI = 3.14159;
        final String NOME_APLICACAO = "Meu App Java";

        System.out.println("Valor de PI: " + PI);
        System.out.println("Nome da Aplicação: " + NOME_APLICACAO);

        // PI = 3.0; // ERRO de compilação! Não é possível reatribuir um valor a uma constante final.
    }
}
```

##  Desafio de Fixação

1.  Declare uma variável `String` para armazenar seu nome completo.
2.  Declare uma variável `int` para armazenar sua idade.
3.  Declare uma variável `double` para armazenar sua altura.
4.  Declare uma variável `boolean` indicando se você é estudante de programação.
5.  Imprima todas essas informações no console, formatando a saída de forma legível.


## Referências

[1] Oracle. Java SE 21 API. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/](https://docs.oracle.com/en/java/javase/21/docs/api/)
[2] dev.java. Language Basics. Disponível em: [https://dev.java/learn/language-basics/](https://dev.java/learn/language-basics/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](../modulo_01_fundamentos_da_linguagem_java.md) | [Próxima aula/módulo](aula_1_2_operadores.md)
