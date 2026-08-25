# Aula 4.5: Optional: Diga adeus ao NullPointerException 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O Problema do NullPointerException](#1-o-problema-do-nullpointerexception)
  - [2. Introdução ao Optional<T>](#2-introduo-ao-optionalt)
  - [3. Métodos Comuns de Optional](#3-mtodos-comuns-de-optional)
  - [4. Optional e Streams: map, filter, flatMap](#4-optional-e-streams-map-filter-flatmap)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Coleções e Stream API**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o problema do `NullPointerException` (NPE) em Java.
- Aprender a utilizar a classe `Optional` para representar a presença ou ausência de um valor.
- Aplicar métodos de `Optional` como `isPresent()`, `isEmpty()`, `get()`, `orElse()`, `orElseThrow()`, `map()`, `filter()` e `flatMap()`.
- Escrever código mais robusto, legível e livre de NPEs.

##  Conteúdo da Aula

O `NullPointerException` (NPE) é, sem dúvida, um dos erros mais comuns e frustrantes em Java. Ele ocorre quando tentamos acessar um método ou atributo de uma referência que aponta para `null`. Para combater esse problema e promover um código mais seguro e expressivo, o Java 8 introduziu a classe `Optional<T>`.

### 1. O Problema do `NullPointerException`

Considere um cenário onde um método pode ou não retornar um valor. Tradicionalmente, retornar `null` era a forma de indicar a ausência de um valor. No entanto, isso força o código cliente a sempre verificar `null` antes de usar o valor, caso contrário, um NPE pode ocorrer.

```java
public class NPETradicional {
    public static String getNomeUsuario(long id) {
        // Simula uma busca em banco de dados que pode não encontrar o usuário
        if (id == 1) {
            return "Alice";
        } else {
            return null; // Retorna null se o usuário não for encontrado
        }
    }

    public static void main(String[] args) {
        String nome1 = getNomeUsuario(1);
        System.out.println("Nome 1: " + nome1.toUpperCase()); // OK: Alice

        String nome2 = getNomeUsuario(2);
        // System.out.println("Nome 2: " + nome2.toUpperCase()); // ERRO: NullPointerException aqui!

        // Para evitar o NPE, precisaríamos de uma verificação:
        if (nome2 != null) {
            System.out.println("Nome 2: " + nome2.toUpperCase());
        } else {
            System.out.println("Usuário 2 não encontrado.");
        }
    }
}
```

### 2. Introdução ao `Optional<T>`

`Optional<T>` é uma classe contêiner que pode ou não conter um valor não nulo. Se um valor estiver presente, `Optional` o encapsula. Se não houver valor, o `Optional` estará vazio. Ele força o desenvolvedor a pensar explicitamente sobre a possibilidade de um valor estar ausente, tornando o código mais robusto.

#### Criando `Optional`s:

*   `Optional.empty()`: Cria um `Optional` vazio.
*   `Optional.of(T value)`: Cria um `Optional` com o valor fornecido. Lança `NullPointerException` se o valor for `null`.
*   `Optional.ofNullable(T value)`: Cria um `Optional` com o valor fornecido, se não for `null`. Se for `null`, retorna um `Optional` vazio. **É o método mais seguro e recomendado para criar `Optional`s a partir de valores que podem ser `null`**.

```java
import java.util.Optional;

public class OptionalCriacaoExemplo {
    public static void main(String[] args) {
        Optional<String> optionalVazio = Optional.empty();
        Optional<String> optionalComValor = Optional.of("Hello");
        Optional<String> optionalNullableComValor = Optional.ofNullable("World");
        Optional<String> optionalNullableVazio = Optional.ofNullable(null);

        System.out.println("Optional Vazio: " + optionalVazio);
        System.out.println("Optional com Valor: " + optionalComValor);
        System.out.println("Optional Nullable com Valor: " + optionalNullableComValor);
        System.out.println("Optional Nullable Vazio: " + optionalNullableVazio);

        // Optional.of(null); // Lançaria NullPointerException
    }
}
```

### 3. Métodos Comuns de `Optional`

*   **`isPresent()`**: Retorna `true` se o `Optional` contiver um valor, `false` caso contrário.
*   **`isEmpty()`**: Retorna `true` se o `Optional` estiver vazio, `false` caso contrário (introduzido no Java 11).
*   **`get()`**: Retorna o valor contido no `Optional`. Lança `NoSuchElementException` se o `Optional` estiver vazio. **Use com cautela, apenas após verificar `isPresent()`**.
*   **`orElse(T other)`**: Retorna o valor contido no `Optional` se estiver presente, caso contrário, retorna o valor `other` fornecido.
*   **`orElseGet(Supplier<? extends T> other)`**: Semelhante a `orElse()`, mas o valor padrão é produzido por um `Supplier` (útil para valores padrão que exigem computação).
*   **`orElseThrow()` / `orElseThrow(Supplier<? extends X> exceptionSupplier)`**: Retorna o valor se presente, caso contrário, lança uma exceção. `orElseThrow()` sem argumentos lança `NoSuchElementException` (Java 10+).
*   **`ifPresent(Consumer<? super T> action)`**: Se um valor estiver presente, executa a ação fornecida com o valor. Não faz nada se o `Optional` estiver vazio.
*   **`ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction)`**: Se um valor estiver presente, executa a ação. Caso contrário, executa a `emptyAction` (Java 9+).

```java
import java.util.Optional;

public class OptionalMetodosExemplo {
    public static Optional<String> buscarNome(long id) {
        if (id == 1) {
            return Optional.of("Bob");
        } else {
            return Optional.empty();
        }
    }

    public static void main(String[] args) {
        Optional<String> nomeEncontrado = buscarNome(1);
        Optional<String> nomeNaoEncontrado = buscarNome(2);

        // isPresent() e get()
        if (nomeEncontrado.isPresent()) {
            System.out.println("Nome encontrado (get): " + nomeEncontrado.get());
        }

        // orElse()
        String nomePadrao = nomeNaoEncontrado.orElse("Convidado");
        System.out.println("Nome padrão (orElse): " + nomePadrao);

        // orElseGet()
        String nomeCalculado = nomeNaoEncontrado.orElseGet(() -> {
            System.out.println("Gerando nome padrão...");
            return "Visitante";
        });
        System.out.println("Nome calculado (orElseGet): " + nomeCalculado);

        // ifPresent()
        nomeEncontrado.ifPresent(nome -> System.out.println("Saudação (ifPresent): Olá, " + nome + "!"));
        nomeNaoEncontrado.ifPresent(nome -> System.out.println("Isso não será impresso."));

        // orElseThrow()
        try {
            nomeNaoEncontrado.orElseThrow(() -> new IllegalArgumentException("Usuário não existe!"));
        } catch (IllegalArgumentException e) {
            System.out.println("Erro: " + e.getMessage());
        }
    }
}
```

### 4. `Optional` e Streams: `map`, `filter`, `flatMap`

`Optional` também pode ser integrado com a Streams API, usando operações funcionais para transformar ou filtrar o valor, se presente.

*   **`map(Function mapper)`**: Se um valor estiver presente, aplica a função de mapeamento a ele e retorna um `Optional` contendo o resultado. Se o `Optional` estiver vazio, retorna um `Optional` vazio.
*   **`filter(Predicate predicate)`**: Se um valor estiver presente e satisfizer o predicado, retorna um `Optional` contendo o valor. Caso contrário, retorna um `Optional` vazio.
*   **`flatMap(Function mapper)`**: Se um valor estiver presente, aplica a função de mapeamento (que deve retornar um `Optional`) a ele e retorna o resultado. Isso é útil para encadear operações que também retornam `Optional`s, evitando `Optional<Optional<T>>`.

```java
import java.util.Optional;

public class OptionalStreamsExemplo {
    public static void main(String[] args) {
        Optional<String> nomeOptional = Optional.of("  ana maria  ");

        // Usando map para transformar o valor
        Optional<String> nomeProcessado = nomeOptional
            .map(String::trim) // Remove espaços em branco
            .map(String::toUpperCase); // Converte para maiúsculas

        nomeProcessado.ifPresent(System.out::println); // ANA MARIA

        // Usando filter para verificar uma condição
        Optional<String> nomeLongo = nomeOptional
            .filter(nome -> nome.length() > 10);

        nomeLongo.ifPresentOrElse(
            nome -> System.out.println("Nome longo: " + nome),
            () -> System.out.println("Nome não é longo o suficiente.")
        ); // Nome não é longo o suficiente.

        // Exemplo com flatMap (simulando uma operação que retorna Optional)
        Optional<String> resultadoFlatMap = Optional.of("123")
            .flatMap(s -> {
                try {
                    return Optional.of(Integer.parseInt(s) * 2);
                } catch (NumberFormatException e) {
                    return Optional.empty();
                }
            })
            .map(Object::toString); // Converte o Integer de volta para String

        resultadoFlatMap.ifPresent(System.out::println); // 246
    }
}
```

##  Desafio de Fixação

1.  Crie um método `public static Optional<Double> dividir(double numerador, double denominador)` que retorne um `Optional<Double>`. Se o `denominador` for zero, retorne um `Optional.empty()`, caso contrário, retorne o resultado da divisão encapsulado em um `Optional.of()`.
2.  No método `main`, chame o método `dividir()` com:
    *   `10.0` e `2.0`
    *   `5.0` e `0.0`
3.  Para o primeiro resultado (divisão por 2.0):
    *   Use `ifPresent()` para imprimir o resultado da divisão.
    *   Use `map()` para multiplicar o resultado por `10` e, em seguida, imprima o novo valor.
4.  Para o segundo resultado (divisão por 0.0):
    *   Use `orElse()` para imprimir "Divisão por zero não permitida." se o `Optional` estiver vazio.
    *   Use `orElseThrow()` para lançar uma `ArithmeticException` com a mensagem "Erro: Divisão por zero!" se o `Optional` estiver vazio.


## Referências

[1] Oracle. Collections Framework. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html)
[2] Oracle. Stream Package. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_4_4_streams_avancado.md) | [Próxima aula/módulo](../modulo_05_tratamento_de_erros_e_topicos_avancados.md)
