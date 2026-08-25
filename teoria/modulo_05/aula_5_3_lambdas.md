# Aula 5.3: Expressões Lambda: Programação Funcional no Java

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O que são Expressões Lambda?](#1-o-que-so-expresses-lambda)
  - [2. Interfaces Funcionais](#2-interfaces-funcionais)
  - [3. Lambdas com Streams API](#3-lambdas-com-streams-api)
  - [4. Method References (Referências de Método)](#4-method-references-referncias-de-mtodo)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Tratamento de Erros e Tópicos Avançados**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o conceito de Expressões Lambda e sua sintaxe.
- Entender o papel das Interfaces Funcionais e como elas se relacionam com Lambdas.
- Aplicar Lambdas para simplificar o código e torná-lo mais conciso e legível.
- Utilizar Lambdas com a Streams API e outras APIs funcionais do Java.
- Conhecer os Method References e como eles podem substituir Lambdas em certos casos.

##  Conteúdo da Aula

As **Expressões Lambda**, introduzidas no Java 8, são um dos recursos mais significativos para a programação funcional na linguagem. Elas permitem que você trate a funcionalidade como um argumento de método, ou código como dados. Em essência, uma expressão lambda é um bloco de código anônimo que pode ser passado como um objeto.

### 1. O que são Expressões Lambda?

Uma expressão lambda é uma função anônima, ou seja, uma função sem nome. Ela fornece uma maneira concisa de representar uma instância de uma interface funcional (uma interface com um único método abstrato).

#### Sintaxe Básica:

` (parâmetros) -> { corpo da expressão } `

*   **`parâmetros`**: Pode ser vazio, um único parâmetro (sem parênteses se o tipo for inferido), ou múltiplos parâmetros (com parênteses e tipos opcionais).
*   **`->`**: O operador lambda, que separa os parâmetros do corpo.
*   **`corpo da expressão`**: Pode ser uma única expressão (o resultado é retornado implicitamente) ou um bloco de código (com `return` explícito se houver um valor de retorno).

```java
// Exemplo básico de Lambda

// Antes do Java 8: Classe anônima para implementar uma interface
Runnable runnableAntigo = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello from old Runnable!");
    }
};

// Com Lambda: Implementação concisa de uma interface funcional
Runnable runnableLambda = () -> System.out.println("Hello from Lambda Runnable!");

public class LambdaBasicoExemplo {
    public static void main(String[] args) {
        new Thread(runnableAntigo).start();
        new Thread(runnableLambda).start();

        // Lambda diretamente no argumento do método
        new Thread(() -> System.out.println("Hello from inline Lambda!")).start();
    }
}
```

### 2. Interfaces Funcionais

Uma **Interface Funcional** é uma interface que possui exatamente um método abstrato. Elas são o ponto de conexão entre as expressões lambda e o sistema de tipos do Java. A anotação `@FunctionalInterface` é opcional, mas é uma boa prática para indicar que a interface é destinada a ser funcional e para que o compilador verifique essa regra.

O pacote `java.util.function` contém várias interfaces funcionais pré-definidas para uso comum:

*   **`Predicate<T>`**: `boolean test(T t)` - Recebe um `T` e retorna um `boolean`.
*   **`Consumer<T>`**: `void accept(T t)` - Recebe um `T` e não retorna nada.
*   **`Function<T, R>`**: `R apply(T t)` - Recebe um `T` e retorna um `R`.
*   **`Supplier<T>`**: `T get()` - Não recebe nada e retorna um `T`.
*   **`UnaryOperator<T>`**: `T apply(T t)` - Recebe um `T` e retorna um `T` (especialização de `Function` onde entrada e saída são do mesmo tipo).
*   **`BinaryOperator<T>`**: `T apply(T t1, T t2)` - Recebe dois `T`s e retorna um `T` (especialização de `BiFunction` onde todas as entradas e saídas são do mesmo tipo).

```java
import java.util.function.Predicate;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Supplier;

public class InterfacesFuncionaisExemplo {
    public static void main(String[] args) {
        // Predicate: Verifica se um número é par
        Predicate<Integer> isPar = (numero) -> numero % 2 == 0;
        System.out.println("10 é par? " + isPar.test(10)); // true

        // Consumer: Imprime uma mensagem
        Consumer<String> saudacao = (nome) -> System.out.println("Olá, " + nome + "!");
        saudacao.accept("Alice"); // Olá, Alice!

        // Function: Converte String para Integer
        Function<String, Integer> stringToInt = (s) -> Integer.parseInt(s);
        System.out.println("String '123' para int: " + stringToInt.apply("123")); // 123

        // Supplier: Fornece uma String
        Supplier<String> mensagem = () -> "Mensagem do Supplier";
        System.out.println(mensagem.get()); // Mensagem do Supplier
    }
}
```

### 3. Lambdas com Streams API

As expressões lambda são a espinha dorsal da Streams API, permitindo que você escreva operações de processamento de dados de forma fluida e declarativa. Métodos como `filter`, `map`, `forEach`, `sorted` e `reduce` aceitam interfaces funcionais como argumentos, tornando-os ideais para lambdas.

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class LambdaComStreamsExemplo {
    public static void main(String[] args) {
        List<String> nomes = Arrays.asList("Alice", "bob", "Carlos", "ana", "Amanda");

        // Filtrar nomes que começam com 'A' (ignorando caixa) e converter para maiúsculas
        List<String> nomesProcessados = nomes.stream()
                                         .filter(nome -> nome.toLowerCase().startsWith("a")) // Lambda para filter
                                         .map(String::toUpperCase) // Method Reference para map
                                         .sorted((s1, s2) -> s1.compareTo(s2)) // Lambda para sorted
                                         .collect(Collectors.toList());

        System.out.println("Nomes processados: " + nomesProcessados); // [ALICE, AMANDA, ANA]

        // Contar nomes com mais de 3 letras
        long count = nomes.stream()
                          .filter(nome -> nome.length() > 3) // Lambda para filter
                          .count();
        System.out.println("Número de nomes com mais de 3 letras: " + count); // 4
    }
}
```

### 4. Method References (Referências de Método)

Referências de método são uma forma ainda mais concisa de expressar expressões lambda que simplesmente chamam um método existente. Elas são úteis quando a expressão lambda apenas encaminha argumentos para um método.

#### Tipos de Method References:

*   **Referência a um método estático**: `Classe::metodoEstatico` (ex: `Math::max`)
*   **Referência a um método de instância de um objeto particular**: `objeto::metodoInstancia` (ex: `System.out::println`)
*   **Referência a um método de instância de um objeto arbitrário de um tipo particular**: `Classe::metodoInstancia` (ex: `String::length`)
*   **Referência a um construtor**: `Classe::new` (ex: `ArrayList::new`)

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Supplier;

public class MethodReferenceExemplo {
    public static void main(String[] args) {
        List<String> palavras = Arrays.asList("Java", "Lambda", "Streams");

        // Lambda: palavra -> System.out.println(palavra)
        Consumer<String> impressor = System.out::println; // Referência a método de instância de um objeto particular
        palavras.forEach(impressor);

        // Lambda: s -> s.length()
        Function<String, Integer> getLength = String::length; // Referência a método de instância de um objeto arbitrário
        palavras.stream()
                .map(getLength)
                .forEach(System.out::println); // 4, 6, 7

        // Lambda: () -> new String()
        Supplier<String> stringSupplier = String::new; // Referência a um construtor
        String novaString = stringSupplier.get();
        System.out.println("Nova String: '" + novaString + "'");

        // Lambda: (a, b) -> Math.max(a, b)
        BinaryOperator<Integer> maximo = Math::max; // Referência a método estático
        System.out.println("Máximo entre 10 e 20: " + maximo.apply(10, 20)); // 20
    }
}
```

##  Desafio de Fixação

1.  Crie uma `List<Produto>` (com atributos `nome`, `preco`, `categoria`). Adicione alguns produtos de diferentes categorias.
2.  Usando Streams e Lambdas:
    *   Filtre os produtos da categoria "Eletrônicos".
    *   Mapeie esses produtos para seus nomes em maiúsculas.
    *   Ordene os nomes resultantes em ordem alfabética.
    *   Colete os nomes em uma `List<String>` e imprima-a.
3.  Usando `forEach` com Method Reference, imprima todos os produtos da lista original.
4.  Calcule o preço total de todos os produtos da categoria "Livros" usando `filter` e `mapToDouble` (ou `reduce`).


## Referências

[1] Oracle. Virtual Threads. Disponível em: [https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html](https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html)
[2] Oracle. Exceptions Tutorial. Disponível em: [https://docs.oracle.com/javase/tutorial/essential/exceptions/](https://docs.oracle.com/javase/tutorial/essential/exceptions/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_5_2_generics.md) | [Próxima aula/módulo](aula_5_4_concorrencia_virtual_threads.md)
