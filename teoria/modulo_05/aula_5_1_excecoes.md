# Aula 5.1: Exceções: Lidando com o Inesperado

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O que são Exceções?](#1-o-que-so-excees)
  - [2. Hierarquia de Exceções](#2-hierarquia-de-excees)
  - [3. Tratamento de Exceções: try-catch-finally](#3-tratamento-de-excees-try-catch-finally)
  - [4. try-with-resources (Java 7+)](#4-try-with-resources-java-7)
  - [5. Lançando Exceções (throw) e Declarando Exceções (throws)](#5-lanando-excees-throw-e-declarando-excees-throws)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Tratamento de Erros e Tópicos Avançados**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o conceito de exceções em Java e sua importância para a robustez do código.
- Aprender a utilizar os blocos `try-catch-finally` para tratar exceções.
- Diferenciar exceções `Checked` e `Unchecked`.
- Saber quando e como lançar exceções (`throw`) e declarar exceções (`throws`).
- Entender a hierarquia de exceções em Java.

##  Conteúdo da Aula

Na programação, nem tudo sai como o esperado. Erros podem acontecer: um arquivo que não existe, uma divisão por zero, uma conexão de rede que falha. Em Java, esses eventos inesperados são tratados através de **exceções**. Uma exceção é um evento que interrompe o fluxo normal de execução de um programa.

O tratamento de exceções é crucial para criar aplicações robustas e confiáveis, que podem se recuperar de erros ou, pelo menos, falhar de forma controlada e informar o usuário adequadamente.

### 1. O que são Exceções?

Uma **exceção** é um objeto que encapsula informações sobre um problema que ocorreu durante a execução de um programa. Quando um erro ocorre, o Java "lança" uma exceção. Se essa exceção não for "capturada" e tratada, o programa é encerrado abruptamente.

### 2. Hierarquia de Exceções

Todas as exceções em Java são subclasses da classe `java.lang.Throwable`. `Throwable` tem duas subclasses diretas:

*   **`Error`**: Representa problemas sérios que geralmente não podem ser recuperados pelo programa (ex: `OutOfMemoryError`, `StackOverflowError`). Não devemos tentar capturar `Error`s.
*   **`Exception`**: Representa condições que um programa pode (e geralmente deve) tentar capturar e tratar. `Exception` se divide em:
    *   **`Checked Exceptions`**: Exceções que o compilador *força* você a tratar (declarar com `throws` ou capturar com `try-catch`). Geralmente representam condições externas que o programa pode prever e se recuperar (ex: `IOException`, `SQLException`).
    *   **`Unchecked Exceptions`** (ou `RuntimeException`): Exceções que o compilador *não força* você a tratar. Geralmente indicam erros de programação (ex: `NullPointerException`, `ArrayIndexOutOfBoundsException`, `ArithmeticException`). Embora não seja obrigatório, você pode capturá-las se desejar.

```java
// Exemplo de Unchecked Exception (ArithmeticException)
public class ExcecaoUncheckedExemplo {
    public static void main(String[] args) {
        int a = 10;
        int b = 0;
        // int resultado = a / b; // Lança ArithmeticException (Unchecked)
        // System.out.println(resultado);

        String texto = null;
        // System.out.println(texto.length()); // Lança NullPointerException (Unchecked)
    }
}
```

### 3. Tratamento de Exceções: `try-catch-finally`

O bloco `try-catch-finally` é a estrutura fundamental para lidar com exceções.

*   **`try`**: Contém o código que *pode* lançar uma exceção.
*   **`catch`**: Contém o código que será executado se uma exceção do tipo especificado for lançada no bloco `try`. Você pode ter múltiplos blocos `catch` para diferentes tipos de exceções.
*   **`finally`**: Contém o código que *sempre* será executado, independentemente de uma exceção ter sido lançada ou não. É útil para liberar recursos (fechar arquivos, conexões de banco de dados).

```java
import java.io.FileReader;
import java.io.IOException;

public class TryCatchFinallyExemplo {
    public static void main(String[] args) {
        FileReader reader = null;
        try {
            reader = new FileReader("arquivo.txt"); // Pode lançar FileNotFoundException (Checked)
            int charCode = reader.read(); // Pode lançar IOException (Checked)
            System.out.println("Primeiro caractere: " + (char) charCode);

            int resultado = 10 / 0; // Lança ArithmeticException (Unchecked)
            System.out.println("Resultado: " + resultado);

        } catch (IOException e) { // Captura Checked Exceptions
            System.err.println("Erro de I/O: " + e.getMessage());
        } catch (ArithmeticException e) { // Captura Unchecked Exceptions
            System.err.println("Erro aritmético: " + e.getMessage());
        } catch (Exception e) { // Captura qualquer outra exceção (polimorfismo)
            System.err.println("Ocorreu um erro inesperado: " + e.getMessage());
        } finally {
            System.out.println("Bloco finally sempre executado.");
            if (reader != null) {
                try {
                    reader.close(); // Pode lançar IOException
                    System.out.println("Recurso fechado no finally.");
                } catch (IOException e) {
                    System.err.println("Erro ao fechar o recurso: " + e.getMessage());
                }
            }
        }
        System.out.println("Execução do programa continua após o bloco try-catch-finally.");
    }
}
```

### 4. `try-with-resources` (Java 7+)

Para garantir que recursos que implementam `AutoCloseable` (como `FileReader`, `Connection`, `Scanner`) sejam fechados automaticamente, mesmo que ocorra uma exceção, o Java 7 introduziu o `try-with-resources`. Isso elimina a necessidade do bloco `finally` para fechamento de recursos.

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class TryWithResourcesExemplo {
    public static void main(String[] args) {
        // O recurso (BufferedReader) é declarado dentro dos parênteses do try
        try (BufferedReader br = new BufferedReader(new FileReader("arquivo.txt"))) {
            String linha;
            while ((linha = br.readLine()) != null) {
                System.out.println(linha);
            }
        } catch (IOException e) {
            System.err.println("Erro ao ler o arquivo: " + e.getMessage());
        }
        System.out.println("Programa continua.");
    }
}
```

### 5. Lançando Exceções (`throw`) e Declarando Exceções (`throws`)

*   **`throw`**: Usado para lançar uma exceção explicitamente. Você cria uma instância da exceção e a lança.
    ```java
    if (idade < 0) {
        throw new IllegalArgumentException("Idade não pode ser negativa.");
    }
    ```
*   **`throws`**: Usado na assinatura de um método para declarar que o método *pode* lançar uma ou mais exceções `Checked`. Isso informa ao chamador que ele deve tratar essas exceções.
    ```java
    public void lerArquivo() throws IOException {
        // ... código que pode lançar IOException ...
    }
    ```

##  Desafio de Fixação

1.  Crie um método `public static double calcularDivisao(double numerador, double denominador)`.
2.  Dentro deste método, se o `denominador` for zero, lance uma `IllegalArgumentException` com a mensagem "Divisão por zero não permitida!".
3.  No método `main`, chame `calcularDivisao()` duas vezes:
    *   Uma vez com valores válidos (ex: 10.0, 2.0) e imprima o resultado.
    *   Outra vez com o denominador zero (ex: 5.0, 0.0).
4.  Utilize um bloco `try-catch` no `main` para capturar a `IllegalArgumentException` e imprimir uma mensagem de erro amigável ao usuário.
5.  Adicione um bloco `finally` que imprima "Operação de divisão finalizada.", independentemente de ter ocorrido uma exceção ou não.


## Referências

[1] Oracle. Virtual Threads. Disponível em: [https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html](https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html)
[2] Oracle. Exceptions Tutorial. Disponível em: [https://docs.oracle.com/javase/tutorial/essential/exceptions/](https://docs.oracle.com/javase/tutorial/essential/exceptions/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](../modulo_05_tratamento_de_erros_e_topicos_avancados.md) | [Próxima aula/módulo](aula_5_2_generics.md)
