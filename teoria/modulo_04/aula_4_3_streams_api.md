# Aula 4.3: Streams API - O Poder da Programação Funcional

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O que são Streams?](#1-o-que-so-streams)
  - [2. Ciclo de Vida de uma Stream](#2-ciclo-de-vida-de-uma-stream)
  - [3. Operações Intermediárias Comuns](#3-operaes-intermedirias-comuns)
  - [4. Operações Terminais Comuns](#4-operaes-terminais-comuns)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Coleções e Stream API**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o que é a Streams API e sua importância na programação funcional em Java.
- Aprender a criar Streams a partir de diferentes fontes (Collections, Arrays, etc.).
- Utilizar operações intermediárias (`filter`, `map`, `sorted`) para transformar e filtrar dados.
- Utilizar operações terminais (`forEach`, `collect`, `count`, `reduce`) para produzir um resultado final.
- Entender a diferença entre operações intermediárias e terminais.

##  Conteúdo da Aula

A **Streams API**, introduzida no Java 8, revolucionou a forma como processamos coleções de dados. Ela oferece uma abordagem funcional para realizar operações complexas em sequências de elementos, como filtragem, mapeamento e redução, de forma mais concisa, legível e, em muitos casos, mais eficiente do que os laços tradicionais.

### 1. O que são Streams?

Uma `Stream` em Java é uma sequência de elementos que suporta operações sequenciais e paralelas. Diferente das `Collections`, que são estruturas de dados que armazenam elementos, uma `Stream` não armazena dados; ela opera sobre uma fonte de dados (como uma `Collection` ou um array) e produz um resultado. Streams são:

*   **Funcionais**: As operações em Streams não modificam a fonte de dados original.
*   **Preguiçosas (Lazy)**: As operações intermediárias só são executadas quando uma operação terminal é invocada.
*   **Consumíveis**: Uma Stream só pode ser percorrida uma única vez. Após uma operação terminal, a Stream é "consumida" e não pode ser reutilizada.

### 2. Ciclo de Vida de uma Stream

O processamento de uma Stream geralmente segue três etapas:

1.  **Fonte de Dados**: A partir de onde a Stream é criada (Collection, Array, I/O, etc.).
2.  **Operações Intermediárias**: Uma ou mais operações que transformam a Stream em outra Stream. São *lazy* e encadeáveis (ex: `filter`, `map`, `sorted`).
3.  **Operação Terminal**: Uma operação que produz um resultado final ou um efeito colateral. Consome a Stream e encerra o pipeline (ex: `forEach`, `collect`, `count`, `reduce`).

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamBasicoExemplo {
    public static void main(String[] args) {
        List<String> nomes = Arrays.asList("Alice", "Bob", "Ana", "Carlos", "Amanda");

        // 1. Criando uma Stream a partir de uma List
        // 2. Operações Intermediárias: filter e map
        List<String> nomesFiltrados = nomes.stream()
                                         .filter(nome -> nome.startsWith("A")) // Filtra nomes que começam com 'A'
                                         .map(String::toUpperCase) // Converte para maiúsculas
                                         .sorted() // Ordena alfabeticamente
                                         .collect(Collectors.toList()); // 3. Operação Terminal: Coleta para uma nova List

        System.out.println("Nomes filtrados e transformados: " + nomesFiltrados); // [ALICE, AMANDA, ANA]

        // Exemplo com forEach (operação terminal)
        System.out.println("\nNomes originais:");
        nomes.stream()
             .forEach(System.out::println); // Imprime cada nome

        // Contando elementos (operação terminal)
        long count = nomes.stream()
                          .filter(nome -> nome.length() > 3)
                          .count();
        System.out.println("\nNúmero de nomes com mais de 3 letras: " + count); // 4 (Alice, Carlos, Amanda)
    }
}
```

### 3. Operações Intermediárias Comuns

*   **`filter(Predicate<T> predicate)`**: Retorna uma Stream contendo apenas os elementos que satisfazem o predicado.
*   **`map(Function<T, R> mapper)`**: Retorna uma Stream aplicando uma função a cada elemento da Stream original, transformando-o em um novo tipo ou valor.
*   **`sorted()` / `sorted(Comparator<T> comparator)`**: Retorna uma Stream com os elementos ordenados. Pode usar a ordem natural ou um `Comparator` customizado.
*   **`distinct()`**: Retorna uma Stream com elementos únicos (remove duplicatas).
*   **`limit(long maxSize)`**: Retorna uma Stream com no máximo `maxSize` elementos.
*   **`skip(long n)`**: Retorna uma Stream descartando os primeiros `n` elementos.

### 4. Operações Terminais Comuns

*   **`forEach(Consumer<T> action)`**: Executa uma ação para cada elemento da Stream. Não retorna nada.
*   **`collect(Collector<T, A, R> collector)`**: Acumula os elementos da Stream em uma `Collection` ou outro tipo de resultado. `Collectors.toList()`, `Collectors.toSet()`, `Collectors.toMap()` são exemplos comuns.
*   **`count()`**: Retorna o número de elementos na Stream como um `long`.
*   **`min(Comparator<T> comparator)` / `max(Comparator<T> comparator)`**: Retorna o menor/maior elemento da Stream, encapsulado em um `Optional`.
*   **`findFirst()` / `findAny()`**: Retorna um `Optional` contendo o primeiro/qualquer elemento da Stream.
*   **`allMatch(Predicate<T> predicate)` / `anyMatch(Predicate<T> predicate)` / `noneMatch(Predicate<T> predicate)`**: Retorna um `boolean` indicando se todos, algum ou nenhum elemento satisfaz o predicado.
*   **`reduce(BinaryOperator<T> accumulator)` / `reduce(T identity, BinaryOperator<T> accumulator)`**: Combina os elementos da Stream em um único resultado.

##  Desafio de Fixação

1.  Crie uma `List<Integer>` com os números: `1, 5, 8, 10, 15, 20, 22, 25, 30`.
2.  Usando Streams, filtre apenas os números pares.
3.  Mapeie esses números pares para o seu dobro.
4.  Ordene os números resultantes em ordem decrescente.
5.  Colete os números dobrados e ordenados em uma nova `List<Integer>`.
6.  Imprima a lista final.
7.  Calcule a soma de todos os números na lista original usando `reduce()` e imprima o resultado.


## Referências

[1] Oracle. Collections Framework. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html)
[2] Oracle. Stream Package. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_4_2_maps.md) | [Próxima aula/módulo](aula_4_4_streams_avancado.md)
