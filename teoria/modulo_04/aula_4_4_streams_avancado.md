# Aula 4.4: Operações Avançadas com Streams 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. flatMap: Achatar Streams de Streams](#1-flatmap-achatar-streams-de-streams)
  - [2. peek: Observando a Stream para Depuração](#2-peek-observando-a-stream-para-depurao)
  - [3. reduce: Agregações Customizadas](#3-reduce-agregaes-customizadas)
  - [4. collect com groupingBy e partitioningBy](#4-collect-com-groupingby-e-partitioningby)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Coleções e Stream API**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Aprofundar o conhecimento em operações intermediárias e terminais da Streams API.
- Utilizar `flatMap` para achatar Streams de Streams.
- Explorar `peek` para depuração e efeitos colaterais.
- Entender e aplicar `reduce` para agregações complexas.
- Utilizar `collect` com `groupingBy` e `partitioningBy` para agrupar e particionar dados.

##  Conteúdo da Aula

Na aula anterior, vimos os fundamentos da Streams API. Agora, vamos explorar algumas operações mais avançadas que nos permitem resolver problemas complexos de manipulação de dados de forma elegante e funcional.

### 1. `flatMap`: Achatar Streams de Streams

O método `map` transforma cada elemento de uma Stream em um único novo elemento. No entanto, se a função de mapeamento retornar uma `Stream` (ou uma `Collection` que pode ser convertida em `Stream`), `map` resultaria em uma `Stream<Stream<T>>` ou `Stream<List<T>>`. O `flatMap` é usado para "achatar" essas Streams aninhadas em uma única `Stream`.

Imagine que você tem uma lista de frases e quer obter uma única lista de todas as palavras únicas.

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class FlatMapExemplo {
    public static void main(String[] args) {
        List<String> frases = Arrays.asList(
            "Java é uma linguagem poderosa",
            "Streams API facilita o processamento de dados",
            "Programação funcional em Java"
        );

        // Usando map - resultaria em Stream<Stream<String>>
        // frases.stream().map(frase -> Arrays.stream(frase.split(" ")));

        // Usando flatMap para obter uma única Stream de palavras
        List<String> palavrasUnicas = frases.stream()
                                            .flatMap(frase -> Arrays.stream(frase.split(" "))) // Achata as Streams de palavras
                                            .map(String::toLowerCase) // Converte para minúsculas
                                            .distinct() // Remove duplicatas
                                            .sorted() // Ordena alfabeticamente
                                            .collect(Collectors.toList());

        System.out.println("Palavras únicas: " + palavrasUnicas);
        // Saída: [a, api, de, dados, em, é, facilita, funcional, java, linguagem, o, poderosa, processamento, programação, streams, uma]
    }
}
```

### 2. `peek`: Observando a Stream para Depuração

O método `peek` é uma operação intermediária que permite executar uma ação em cada elemento da Stream sem modificá-lo. É útil principalmente para depuração, para ver o que está acontecendo em cada estágio do pipeline da Stream, sem alterar o fluxo de dados.

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class PeekExemplo {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(1, 2, 3, 4, 5);

        List<Integer> resultado = numeros.stream()
                                         .peek(n -> System.out.println("Antes do filter: " + n)) // Debug
                                         .filter(n -> n % 2 == 0) // Filtra pares
                                         .peek(n -> System.out.println("Após o filter: " + n)) // Debug
                                         .map(n -> n * 10) // Multiplica por 10
                                         .peek(n -> System.out.println("Após o map: " + n)) // Debug
                                         .collect(Collectors.toList());

        System.out.println("Resultado final: " + resultado);
    }
}
```

### 3. `reduce`: Agregações Customizadas

O `reduce` é uma operação terminal poderosa que combina todos os elementos de uma Stream em um único resultado, usando uma função de agregação. Vimos um exemplo simples na aula anterior para somar números. Ele pode ser usado para encontrar o maior/menor, concatenar strings, ou qualquer outra operação que reduza uma sequência a um único valor.

#### Variações de `reduce`:

*   `Optional<T> reduce(BinaryOperator<T> accumulator)`: Retorna um `Optional` porque a Stream pode estar vazia.
*   `T reduce(T identity, BinaryOperator<T> accumulator)`: `identity` é o valor inicial e também o valor retornado se a Stream estiver vazia.
*   `U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner)`: Usado para processamento paralelo, onde `combiner` combina os resultados parciais.

```java
import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class ReduceExemplo {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(1, 2, 3, 4, 5);

        // Soma de todos os números (com identity)
        int soma = numeros.stream().reduce(0, (a, b) -> a + b);
        System.out.println("Soma: " + soma); // 15

        // Multiplicação de todos os números (com identity)
        int produto = numeros.stream().reduce(1, (a, b) -> a * b);
        System.out.println("Produto: " + produto); // 120 (1*2*3*4*5)

        // Encontrar o maior número (sem identity, retorna Optional)
        Optional<Integer> maior = numeros.stream().reduce(Integer::max);
        maior.ifPresent(m -> System.out.println("Maior: " + m)); // 5

        // Concatenação de strings
        List<String> palavras = Arrays.asList("Olá", "Mundo", "Java");
        String frase = palavras.stream().reduce("", (s1, s2) -> s1 + " " + s2);
        System.out.println("Frase: " + frase.trim()); // Olá Mundo Java
    }
}
```

### 4. `collect` com `groupingBy` e `partitioningBy`

O coletor `collect` é extremamente versátil. Além de coletar para listas e conjuntos, ele pode ser usado para agrupar e particionar elementos com base em certas características.

*   **`Collectors.groupingBy(Function classifier)`**: Agrupa elementos da Stream em um `Map` onde as chaves são o resultado da função classificadora e os valores são listas dos elementos agrupados.
*   **`Collectors.partitioningBy(Predicate predicate)`**: Particiona os elementos da Stream em dois grupos (um `Map<Boolean, List<T>>`) com base em um predicado: um grupo para elementos que satisfazem o predicado (`true`) e outro para os que não satisfazem (`false`).

```java
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class GroupingPartitioningExemplo {
    public static void main(String[] args) {
        List<String> nomes = Arrays.asList("Alice", "Bob", "Ana", "Carlos", "Amanda", "Beatriz");

        // Agrupando nomes por letra inicial
        Map<Character, List<String>> nomesPorInicial = nomes.stream()
                                                            .collect(Collectors.groupingBy(nome -> nome.charAt(0)));
        System.out.println("Nomes por inicial: " + nomesPorInicial);
        // Saída: {A=[Alice, Ana, Amanda], B=[Bob, Beatriz], C=[Carlos]}

        // Particionando nomes por comprimento (maior que 4 letras)
        Map<Boolean, List<String>> nomesPorComprimento = nomes.stream()
                                                              .collect(Collectors.partitioningBy(nome -> nome.length() > 4));
        System.out.println("\nNomes particionados (comprimento > 4): " + nomesPorComprimento);
        // Saída: {false=[Bob, Ana], true=[Alice, Carlos, Amanda, Beatriz]}

        // Agrupando e contando
        Map<Character, Long> contagemPorInicial = nomes.stream()
                                                       .collect(Collectors.groupingBy(nome -> nome.charAt(0), Collectors.counting()));
        System.out.println("\nContagem de nomes por inicial: " + contagemPorInicial);
        // Saída: {A=3, B=2, C=1}
    }
}
```

##  Desafio de Fixação

1.  Crie uma `List<String>` com os seguintes nomes: "João", "Maria", "Pedro", "Ana", "Mariana", "Carlos", "Julia", "Fernando".
2.  Usando `flatMap`, transforme essa lista em uma `Stream<Character>` contendo todas as letras únicas (em minúsculas) de todos os nomes, ordenadas alfabeticamente.
3.  Utilize `peek` para imprimir cada letra antes de coletá-las.
4.  Colete as letras únicas em uma `List<Character>` e imprima-a.
5.  Usando `groupingBy`, agrupe os nomes da lista original por seu comprimento (número de letras). O resultado deve ser um `Map<Integer, List<String>>`.
6.  Usando `partitioningBy`, separe os nomes da lista original em dois grupos: aqueles que começam com 'M' e aqueles que não começam. O resultado deve ser um `Map<Boolean, List<String>>`.


## Referências

[1] Oracle. Collections Framework. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html)
[2] Oracle. Stream Package. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_4_3_streams_api.md) | [Próxima aula/módulo](aula_4_5_optional.md)
