# Aula 4.2: Mapas e Dicionários - Chave e Valor 🔑

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. Interface Map: Chave-Valor Único](#1-interface-map-chave-valor-nico)
  - [2. Iterando sobre um Map](#2-iterando-sobre-um-map)
  - [3. Quando Usar Qual Implementação de Map?](#3-quando-usar-qual-implementao-de-map)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Coleções e Stream API**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o conceito da interface `Map` na Collections Framework do Java.
- Diferenciar e utilizar `HashMap`, `LinkedHashMap` e `TreeMap` para armazenar pares chave-valor.
- Realizar operações básicas em `Map`s: adicionar, remover, buscar e iterar.
- Escolher a implementação de `Map` mais adequada para diferentes cenários.

##  Conteúdo da Aula

Além de listas e conjuntos, outra estrutura de dados fundamental na programação é o **Mapa** (ou Dicionário). Um `Map` armazena dados como pares de **chave-valor**, onde cada chave é única e mapeia para um único valor. Pense em um dicionário real: cada palavra (chave) tem uma definição (valor).

### 1. Interface `Map`: Chave-Valor Único

A interface `Map` não estende a interface `Collection`, mas faz parte da Collections Framework. Ela representa um mapeamento de chaves para valores. As chaves são únicas, mas os valores podem ser duplicados.

#### Principais Implementações de `Map`:

*   **`HashMap`**: A implementação mais comum de `Map`. Armazena os pares chave-valor em uma tabela hash. Oferece desempenho de tempo constante (`O(1)`) para operações básicas como `put`, `get` e `remove`, assumindo uma boa função de hash. Não garante a ordem de inserção ou qualquer outra ordem.
*   **`LinkedHashMap`**: Mantém a ordem de inserção dos pares chave-valor. É útil quando a ordem em que os elementos foram adicionados é importante. Um pouco mais lento que `HashMap`.
*   **`TreeMap`**: Armazena os pares chave-valor em uma estrutura de árvore binária, o que os mantém ordenados pelas chaves (naturalmente ou por um `Comparator` fornecido). Oferece operações de tempo logarítmico (`O(log n)`) para `put`, `get` e `remove`.

```java
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.Map;
import java.util.TreeMap;

public class MapExemplo {
    public static void main(String[] args) {
        // Exemplo com HashMap (sem ordem garantida)
        Map<String, Integer> idades = new HashMap<>();
        idades.put("Alice", 30);
        idades.put("Bob", 25);
        idades.put("Carlos", 35);
        idades.put("Alice", 31); // Chave duplicada, sobrescreve o valor anterior

        System.out.println("HashMap: " + idades); // Ordem pode variar: {Bob=25, Alice=31, Carlos=35}
        System.out.println("Idade de Bob: " + idades.get("Bob")); // 25
        System.out.println("Contém Carlos? " + idades.containsKey("Carlos")); // true
        System.out.println("Tamanho do mapa: " + idades.size()); // 3

        idades.remove("Bob");
        System.out.println("HashMap após remover Bob: " + idades); // {Alice=31, Carlos=35}

        // Exemplo com LinkedHashMap (mantém ordem de inserção)
        Map<String, String> capitais = new LinkedHashMap<>();
        capitais.put("Brasil", "Brasília");
        capitais.put("Argentina", "Buenos Aires");
        capitais.put("Chile", "Santiago");

        System.out.println("\nLinkedHashMap: " + capitais); // {Brasil=Brasília, Argentina=Buenos Aires, Chile=Santiago}

        // Exemplo com TreeMap (ordenado pelas chaves)
        Map<String, Double> notas = new TreeMap<>();
        notas.put("Matemática", 9.5);
        notas.put("Português", 8.0);
        notas.put("História", 7.0);

        System.out.println("\nTreeMap: " + notas); // {História=7.0, Matemática=9.5, Português=8.0}
    }
}
```

### 2. Iterando sobre um `Map`

Existem várias maneiras de iterar sobre os elementos de um `Map`:

*   **`keySet()`**: Para iterar sobre as chaves.
*   **`values()`**: Para iterar sobre os valores.
*   **`entrySet()`**: Para iterar sobre os pares chave-valor (o mais comum e eficiente).

```java
import java.util.HashMap;
import java.util.Map;

public class IteracaoMapExemplo {
    public static void main(String[] args) {
        Map<String, String> dicionario = new HashMap<>();
        dicionario.put("casa", "habitação");
        dicionario.put("carro", "veículo");
        dicionario.put("livro", "obra literária");

        System.out.println("--- Iterando sobre chaves (keySet()) ---");
        for (String chave : dicionario.keySet()) {
            System.out.println("Chave: " + chave);
        }

        System.out.println("\n--- Iterando sobre valores (values()) ---");
        for (String valor : dicionario.values()) {
            System.out.println("Valor: " + valor);
        }

        System.out.println("\n--- Iterando sobre pares chave-valor (entrySet()) ---");
        for (Map.Entry<String, String> entry : dicionario.entrySet()) {
            System.out.println("Chave: " + entry.getKey() + ", Valor: " + entry.getValue());
        }

        System.out.println("\n--- Usando forEach (Java 8+) ---");
        dicionario.forEach((chave, valor) -> System.out.println("Chave: " + chave + ", Valor: " + valor));
    }
}
```

### 3. Quando Usar Qual Implementação de `Map`?

*   **`HashMap`**: É a escolha padrão para a maioria dos casos. Use quando você precisa de acesso rápido e a ordem dos elementos não é importante.
*   **`LinkedHashMap`**: Use quando você precisa manter a ordem de inserção dos pares chave-valor.
*   **`TreeMap`**: Use quando você precisa que os pares chave-valor sejam ordenados naturalmente pelas chaves (ou por um `Comparator` personalizado).

##  Desafio de Fixação

1.  Crie um `HashMap` onde as chaves são `String` (nomes de alunos) e os valores são `Double` (notas).
2.  Adicione 4 alunos com suas respectivas notas ao mapa.
3.  Imprima a nota de um aluno específico.
4.  Remova um aluno do mapa.
5.  Verifique se um aluno existe no mapa e imprima o resultado.
6.  Itere sobre o mapa e imprima todos os nomes dos alunos e suas notas.


## Referências

[1] Oracle. Collections Framework. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html)
[2] Oracle. Stream Package. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_4_1_list_set.md) | [Próxima aula/módulo](aula_4_3_streams_api.md)
