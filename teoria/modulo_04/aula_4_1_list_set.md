# Aula 4.1: O Universo das Collections - Listas e Conjuntos 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. A Collections Framework: Visão Geral](#1-a-collections-framework-viso-geral)
  - [2. Interface List: Coleções Ordenadas e com Duplicatas](#2-interface-list-colees-ordenadas-e-com-duplicatas)
  - [3. Interface Set: Coleções Não Ordenadas e Sem Duplicatas](#3-interface-set-colees-no-ordenadas-e-sem-duplicatas)
  - [4. Quando Usar Qual?](#4-quando-usar-qual)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Coleções e Stream API**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender a Collections Framework do Java e sua importância.
- Diferenciar e utilizar `List` (ArrayList, LinkedList) para coleções ordenadas e com elementos duplicados.
- Diferenciar e utilizar `Set` (HashSet, TreeSet, LinkedHashSet) para coleções não ordenadas e sem elementos duplicados.
- Escolher a implementação de coleção mais adequada para diferentes cenários.

##  Conteúdo da Aula

No dia a dia da programação, é muito comum precisarmos armazenar e manipular grupos de objetos. A **Collections Framework** do Java é um conjunto de interfaces e classes que fornecem estruturas de dados prontas para uso, como listas, conjuntos e mapas. Ela oferece uma maneira padronizada e eficiente de trabalhar com coleções de objetos.

### 1. A Collections Framework: Visão Geral

A Collections Framework é composta por interfaces principais (`Collection`, `List`, `Set`, `Map`, `Queue`) e suas diversas implementações. Ela nos permite:

*   Armazenar, recuperar e manipular dados.
*   Realizar operações comuns como adicionar, remover, buscar e iterar sobre elementos.
*   Garantir performance e eficiência com implementações otimizadas.

### 2. Interface `List`: Coleções Ordenadas e com Duplicatas

A interface `List` representa uma coleção ordenada de elementos, onde a ordem de inserção é mantida e elementos duplicados são permitidos. Você pode acessar elementos por seu índice (posição).

#### Principais Implementações de `List`:

*   **`ArrayList`**: Implementação baseada em array redimensionável. É excelente para acesso rápido a elementos por índice (`get(index)`) e para adicionar elementos no final. Inserções e remoções no meio da lista podem ser lentas, pois exigem o deslocamento de outros elementos.
*   **`LinkedList`**: Implementação baseada em lista duplamente encadeada. É eficiente para inserções e remoções no início ou no meio da lista, mas o acesso a elementos por índice é mais lento, pois exige percorrer a lista.

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class ListExemplo {
    public static void main(String[] args) {
        // Exemplo com ArrayList
        List<String> nomes = new ArrayList<>();
        nomes.add("Alice");
        nomes.add("Bob");
        nomes.add("Alice"); // Duplicata permitida
        nomes.add(0, "Carlos"); // Adiciona na posição 0

        System.out.println("ArrayList: " + nomes); // [Carlos, Alice, Bob, Alice]
        System.out.println("Elemento na posição 1: " + nomes.get(1)); // Alice
        nomes.remove("Bob");
        System.out.println("ArrayList após remover Bob: " + nomes); // [Carlos, Alice, Alice]

        // Exemplo com LinkedList
        List<Integer> numeros = new LinkedList<>();
        numeros.add(10);
        numeros.add(20);
        numeros.add(30);
        numeros.addFirst(5); // Método específico de LinkedList

        System.out.println("\nLinkedList: " + numeros); // [5, 10, 20, 30]
        numeros.removeLast(); // Método específico de LinkedList
        System.out.println("LinkedList após remover o último: " + numeros); // [5, 10, 20]
    }
}
```

### 3. Interface `Set`: Coleções Não Ordenadas e Sem Duplicatas

A interface `Set` representa uma coleção de elementos únicos, ou seja, não permite elementos duplicados. A ordem dos elementos não é garantida (exceto em algumas implementações específicas).

#### Principais Implementações de `Set`:

*   **`HashSet`**: A implementação mais comum de `Set`. Armazena elementos em uma tabela hash, o que oferece desempenho muito rápido para adicionar, remover e verificar a existência de elementos. Não garante a ordem de inserção.
*   **`LinkedHashSet`**: Mantém a ordem de inserção dos elementos, mas ainda garante a unicidade. É um pouco mais lento que `HashSet`.
*   **`TreeSet`**: Armazena elementos em uma estrutura de árvore binária, o que os mantém ordenados naturalmente (se forem comparáveis) ou por um `Comparator` fornecido. Oferece operações mais lentas que `HashSet` devido à manutenção da ordem.

```java
import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.Set;
import java.util.TreeSet;

public class SetExemplo {
    public static void main(String[] args) {
        // Exemplo com HashSet (sem ordem, sem duplicatas)
        Set<String> frutasHashSet = new HashSet<>();
        frutasHashSet.add("Maçã");
        frutasHashSet.add("Banana");
        frutasHashSet.add("Maçã"); // Duplicata ignorada
        frutasHashSet.add("Laranja");

        System.out.println("HashSet: " + frutasHashSet); // Ordem pode variar: [Maçã, Laranja, Banana]
        System.out.println("Contém Maçã? " + frutasHashSet.contains("Maçã")); // true

        // Exemplo com LinkedHashSet (mantém ordem de inserção, sem duplicatas)
        Set<String> frutasLinkedHashSet = new LinkedHashSet<>();
        frutasLinkedHashSet.add("Maçã");
        frutasLinkedHashSet.add("Banana");
        frutasLinkedHashSet.add("Maçã");
        frutasLinkedHashSet.add("Laranja");

        System.out.println("\nLinkedHashSet: " + frutasLinkedHashSet); // [Maçã, Banana, Laranja]

        // Exemplo com TreeSet (ordenado naturalmente, sem duplicatas)
        Set<String> frutasTreeSet = new TreeSet<>();
        frutasTreeSet.add("Maçã");
        frutasTreeSet.add("Banana");
        frutasTreeSet.add("Maçã");
        frutasTreeSet.add("Laranja");

        System.out.println("\nTreeSet: " + frutasTreeSet); // [Banana, Laranja, Maçã]
    }
}
```

### 4. Quando Usar Qual?

*   **`List`**: Quando a ordem dos elementos é importante e/ou você precisa permitir duplicatas. Use `ArrayList` para acesso rápido por índice e `LinkedList` para inserções/remoções frequentes no meio da lista.
*   **`Set`**: Quando você precisa garantir que todos os elementos são únicos e a ordem não é uma preocupação primária. Use `HashSet` para desempenho geral rápido, `LinkedHashSet` se a ordem de inserção for importante, e `TreeSet` se você precisar que os elementos estejam sempre ordenados.

##  Desafio de Fixação

1.  Crie um `ArrayList` de `String`s e adicione 5 nomes de cidades. Imprima a lista.
2.  Remova a cidade na posição 2 e adicione uma nova cidade na posição 0. Imprima a lista novamente.
3.  Crie um `HashSet` de `Integer`s e adicione 5 números, incluindo um número duplicado. Imprima o conjunto e observe que o duplicado foi ignorado.
4.  Verifique se o `HashSet` contém o número 10 e imprima o resultado.
5.  Crie um `TreeSet` de `String`s e adicione 3 nomes de frutas. Imprima o conjunto e observe a ordem natural.


## Referências

[1] Oracle. Collections Framework. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html)
[2] Oracle. Stream Package. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](../modulo_04_collections_e_stream_api.md) | [Próxima aula/módulo](aula_4_2_maps.md)
