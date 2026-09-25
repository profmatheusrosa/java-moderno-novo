# Módulo 04: Coleções e Stream API
> Domine a manipulação de dados em Java: do básico de Listas ao poder funcional das Streams.

![Diagrama conceitual do Módulo 04: Coleções e Stream API](../imagens/modulo_04_collections_e_stream_api.svg)

## O que você vai aprender neste módulo

- A Collections Framework: List, Set e Map
- O que é a Stream API e como ela muda a forma de programar
- Operações intermediárias e terminais em Streams
- Técnicas avançadas: flatMap, reduce, agrupamentos e collectors customizados
- Como substituir loops imperativos por código funcional
- A classe Optional para evitar NullPointerException

## Sumário

- [Aula 4.1: O Universo das Collections - Listas e Conjuntos](#aula-41-o-universo-das-collections---listas-e-conjuntos)
- [Aula 4.2: Mapas e Dicionários - Chave e Valor](#aula-42-mapas-e-dicionários---chave-e-valor)
- [Aula 4.3: Streams API - O Poder da Programação Funcional](#aula-43-streams-api---o-poder-da-programação-funcional)
- [Aula 4.4: Opcionais: Diga adeus ao NullPointerException](#aula-44-opcionais-diga-adeus-ao-nullpointerexception)
- [Exercícios de fixação do módulo](#exercícios-de-fixação-do-módulo)

---

## Aula 4.1: O Universo das Collections - Listas e Conjuntos

No dia a dia da programação, você raramente vai lidar com apenas uma variável solta. É muito comum precisarmos armazenar e manipular grupos de objetos, como uma lista de clientes ou um conjunto de produtos únicos. A **Collections Framework** [1] do Java te dá estruturas de dados prontas para uso, seguras e muito rápidas.

### 1. Interface `List`: Coleções Ordenadas (e com duplicatas)

Pense na `List` como uma fila de banco ou uma lista de tarefas. A ordem em que você insere os elementos é mantida, e você pode ter elementos repetidos. Você acessa qualquer um deles pela posição (o índice).

**Principais Implementações:**
- **`ArrayList`**: É a sua lista padrão. Excelente para buscar rapidamente um elemento pela posição (`get(index)`). É um array que cresce sozinho. 
- **`LinkedList`**: Uma lista encadeada. Use quando precisar ficar inserindo e removendo elementos do começo ou do meio da lista com muita frequência.

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class ListExemplo {
    public static void main(String[] args) {
        // Criando e preenchendo um ArrayList
        List<String> nomes = new ArrayList<>();
        nomes.add("Alice");
        nomes.add("Bob");
        nomes.add("Alice"); // OK: Duplicatas são super bem-vindas!
        nomes.add(0, "Carlos"); // Empurra todo mundo e entra na posição 0

        System.out.println("Lista: " + nomes); 
        // Saída: [Carlos, Alice, Bob, Alice]
        
        System.out.println("Elemento na posição 1: " + nomes.get(1)); 
        // Saída: Alice
        
        nomes.remove("Bob");
        System.out.println("Após remover Bob: " + nomes); 
        // Saída: [Carlos, Alice, Alice]

        // LinkedList tem truques extras, como adicionar/remover nas pontas
        LinkedList<Integer> numeros = new LinkedList<>();
        numeros.add(10);
        numeros.add(20);
        numeros.addFirst(5); // Dica: método específico da LinkedList

        System.out.println("\nLinkedList: " + numeros); 
        // Saída: [5, 10, 20]
    }
}
```

### 2. Interface `Set`: Conjuntos Únicos (sem duplicatas)

O `Set` é como o álbum de figurinhas de um colecionador exigente: **nada de repetições**. A maioria dos conjuntos também não garante a ordem em que os elementos vão ficar.

**Principais Implementações:**
- **`HashSet`**: O mais rápido de todos para buscar, adicionar e remover. Mas ele bagunça a ordem.
- **`LinkedHashSet`**: Guarda a ordem de inserção, mas consome um pouquinho mais de memória.
- **`TreeSet`**: Já guarda tudo ordenado naturalmente (ordem alfabética, por exemplo) ou segundo uma regra que você criar.

```java
import java.util.HashSet;
import java.util.Set;
import java.util.TreeSet;

public class SetExemplo {
    public static void main(String[] args) {
        // HashSet ignora as repetidas
        Set<String> frutas = new HashSet<>();
        frutas.add("Maçã");
        frutas.add("Banana");
        frutas.add("Maçã"); // Atencao: Será ignorada pelo Set!
        
        System.out.println("HashSet: " + frutas); 
        // Saída: pode variar, ex: [Maçã, Banana]

        // TreeSet já organiza tudo sozinho
        Set<Integer> numeros = new TreeSet<>();
        numeros.add(50);
        numeros.add(10);
        numeros.add(30);
        
        System.out.println("TreeSet: " + numeros); 
        // Saída: [10, 30, 50] (Ordem natural!)
    }
}
```

> Dica: **Dica de ouro:** Precisa de ordem de chegada e aceita repetição? Vai de `List`. Precisa garantir que não há repetições e quer velocidade? Vai de `Set`.

---

## Aula 4.2: Mapas e Dicionários - Chave e Valor

Sabe quando você pesquisa uma palavra no dicionário para achar o significado? O `Map` funciona exatamente assim. Você tem uma **chave única** (a palavra) que aponta para um **valor** (o significado). O valor pode se repetir, mas a chave nunca.

### 1. Principais implementações de Map

- **`HashMap`**: O mapa do dia a dia. Rápido e prático, não garante ordem.
- **`LinkedHashMap`**: Mantém a ordem em que você foi colocando as chaves.
- **`TreeMap`**: Mantém as chaves ordenadas de forma natural (ou por uma regra sua).

```java
import java.util.HashMap;
import java.util.Map;

public class MapExemplo {
    public static void main(String[] args) {
        // Mapa que liga o nome da pessoa (String) à idade (Integer)
        Map<String, Integer> idades = new HashMap<>();
        
        idades.put("Alice", 30);
        idades.put("Bob", 25);
        idades.put("Carlos", 35);
        
        // Sobrescrevendo o valor da chave "Alice"
        idades.put("Alice", 31); 

        System.out.println("Idade do Bob: " + idades.get("Bob")); 
        // Saída: 25
        
        System.out.println("Temos o Carlos? " + idades.containsKey("Carlos")); 
        // Saída: true
        
        // Como iterar por todos de uma vez:
        idades.forEach((nome, idade) -> 
            System.out.println(nome + " tem " + idade + " anos")
        );
    }
}
```

---

## Aula 4.3: Streams API - O Poder da Programação Funcional

A **Streams API** [2], introduzida no Java 8, virou o jogo. Se antes precisávamos escrever laços `for` gigantes e cheios de `if` para filtrar ou transformar listas, as Streams trouxeram um estilo declarativo: você diz *o que* quer fazer, não *como* fazer.

> Uma `Stream` não armazena dados, ela é um fluxo de dados. Ela pega os itens de uma lista e passa por um funil de transformações.

### Imperativo vs Funcional

Imagine que queremos pegar uma lista de números, dobrar apenas os que são pares, e somar o resultado.

**Modo Imperativo (O antigo laço `for`):**
```java
List<Integer> numeros = List.of(1, 2, 3, 4, 5);
int soma = 0;
for (Integer n : numeros) {
    if (n % 2 == 0) {
        soma += n * 2;
    }
}
System.out.println("Soma: " + soma); // Saída: 12
```

**Modo Funcional (Com Stream API):**
```java
int somaFuncional = numeros.stream()
    .filter(n -> n % 2 == 0) // Pega só os pares
    .mapToInt(n -> n * 2)    // Multiplica por 2
    .sum();                  // Soma tudo

System.out.println("Soma Funcional: " + somaFuncional); // Saída: 12
```
Muito mais limpo, direto ao ponto e fácil de ler!

### Operações Intermediárias e Terminais

Uma Stream funciona como uma linha de montagem:
1. Você tem a **Fonte** (a lista original).
2. Você aplica **Operações Intermediárias** (que preparam o terreno, como `filter`, `map`, `sorted`, `flatMap`). Elas são *preguiçosas* e só executam quando realmente necessário.
3. Você chama a **Operação Terminal** (que finaliza a linha e produz o resultado, como `collect`, `reduce`, `sum`, `forEach`). É aqui que a mágica acontece.

### Expandindo os poderes: `flatMap`, `reduce` e Collectors

Quando as coisas ficam complexas, a Stream API brilha ainda mais.

#### O poder de "achatar" com `flatMap`
Se você tiver uma lista de frases e quiser todas as palavras individuais, o `map` criaria uma "lista de listas". O `flatMap` "achata" tudo para um nível só!

```java
List<String> frases = List.of("Java é legal", "Stream é vida");

List<String> palavras = frases.stream()
    .flatMap(frase -> Arrays.stream(frase.split(" "))) // Transforma cada frase numa stream de palavras
    .collect(Collectors.toList());

// Saída: [Java, é, legal, Stream, é, vida]
```

#### Agregações extremas com `reduce`
Quando o `sum` ou `max` não dão conta, o `reduce` te permite criar suas próprias regras para fundir vários elementos em um só (como concatenar uma frase).

```java
List<String> palavras = List.of("Java", "é", "incrível");

String frase = palavras.stream()
    .reduce("", (acumulado, palavraAtual) -> acumulado + " " + palavraAtual);
    
System.out.println(frase.trim()); // Saída: Java é incrível
```

#### Agrupamentos com `groupingBy`
Quer transformar uma lista plana em um Mapa baseado em uma regra? O `groupingBy` faz isso em uma linha.

```java
List<String> nomes = List.of("Ana", "Amanda", "Beto", "Bruno", "Carlos");

// Agrupar pela primeira letra
Map<Character, List<String>> porLetra = nomes.stream()
    .collect(Collectors.groupingBy(nome -> nome.charAt(0)));

System.out.println(porLetra); 
// Saída: {A=[Ana, Amanda], B=[Beto, Bruno], C=[Carlos]}
```

#### Collectors Avançados: `teeing` e Collectors Customizados
Desde o Java 12, podemos usar `Collectors.teeing` para fazer duas coisas ao mesmo tempo e depois juntar os resultados.

```java
List<Integer> notas = List.of(5, 7, 8, 10);

// Calcula a média E pega a maior nota ao mesmo tempo!
String resumo = notas.stream().collect(
    Collectors.teeing(
        Collectors.averagingInt(n -> n),
        Collectors.maxBy(Integer::compareTo),
        (media, maximo) -> "Média: " + media + ", Maior Nota: " + maximo.get()
    )
);
System.out.println(resumo); // Saída: Média: 7.5, Maior Nota: 10
```

Se os Collectors prontos não servirem, você pode implementar a interface `Collector` na mão, definindo o fornecedor (supplier), acumulador e combinador para construir regras super customizadas. Mas no dia a dia, os que estão na classe `Collectors` atendem 99% dos casos!

---

## Aula 4.4: Opcionais: Diga adeus ao NullPointerException

O temido `NullPointerException` (NPE) é o erro mais clássico do Java. A classe `Optional<T>` foi criada no Java 8 justamente para acabar com ele. O `Optional` é como uma caixinha: ela pode estar cheia (com seu valor) ou vazia. 

Ao invés de retornar `null` (que é perigoso), um método deve retornar um `Optional`. Assim, quem recebe é *obrigado* a lidar com a possibilidade da caixinha estar vazia!

```java
import java.util.Optional;

public class OptionalExemplo {
    // Retornamos Optional para deixar claro que o usuário pode não existir!
    public static Optional<String> buscarUsuario(int id) {
        if (id == 1) return Optional.of("Alice"); // Caixa cheia
        return Optional.empty(); // Caixa vazia!
    }

    public static void main(String[] args) {
        Optional<String> usuarioOpt = buscarUsuario(2);

        // Jeito feio e inseguro: usar .get() direto
        // usuarioOpt.get(); // Atencao: Lançaria erro se estiver vazio!

        // Jeito lindo 1: Retornar um valor padrão
        String nome = usuarioOpt.orElse("Visitante");
        System.out.println("Olá, " + nome);

        // Jeito lindo 2: Lançar uma exceção específica se vazio
        // usuarioOpt.orElseThrow(() -> new RuntimeException("Não achei!"));

        // Jeito lindo 3: Integrando com a Stream API
        buscarUsuario(1) // Aqui retorna Alice
            .map(String::toUpperCase)
            .ifPresent(nomeMaiusculo -> System.out.println("ACHOU: " + nomeMaiusculo));
    }
}
```

---

## Exercícios de fixação do módulo

1. **Brincando com Coleções:** Crie um `ArrayList` com 5 nomes de cidades (podendo repetir). Em seguida, coloque todos esses elementos num `HashSet`. Imprima ambas as coleções. O que aconteceu com as duplicatas?
2. **Transformações com Stream:** Crie uma lista com os números de 1 a 10. Usando a Stream API, filtre apenas os números ímpares, eleve eles ao quadrado (`map`), e agrupe-os em uma nova lista (`collect`).
3. **Reduzindo e agrupando:** Com uma lista de palavras diversas ("banana", "maçã", "abacate", "uva", "abacaxi"):
   - Use `groupingBy` para criar um mapa que agrupe essas frutas pela letra inicial.
   - Use `reduce` para concatenar todas as frutas numa única String separada por vírgulas.
4. **Protegendo do Nulo:** Crie um método que recebe o nome de um banco de dados e retorna um `Optional<String>` simulando uma string de conexão. Se o nome for "meu_banco", retorne `Optional.of("url_valida")`. Se for qualquer outra coisa, `Optional.empty()`. No método `main`, chame esse método e use `.orElseThrow()` para lançar um erro caso a configuração não seja encontrada.

<details>
<summary>Gabarito</summary>

```java
// Resposta 1
List<String> listaCidades = List.of("SP", "RJ", "BH", "SP", "Curitiba");
Set<String> setCidades = new HashSet<>(listaCidades);
System.out.println("Lista: " + listaCidades);
System.out.println("Set: " + setCidades); // As duplicatas ("SP") sumiram!

// Resposta 2
List<Integer> numeros = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
List<Integer> imparesAoQuadrado = numeros.stream()
    .filter(n -> n % 2 != 0)
    .map(n -> n * n)
    .collect(Collectors.toList());

// Resposta 3
List<String> frutas = List.of("banana", "maçã", "abacate", "uva", "abacaxi");
Map<Character, List<String>> porInicial = frutas.stream()
    .collect(Collectors.groupingBy(f -> f.charAt(0)));
String concatenado = frutas.stream()
    .reduce((f1, f2) -> f1 + ", " + f2).orElse("");

// Resposta 4
public static Optional<String> getConnection(String banco) {
    if ("meu_banco".equals(banco)) return Optional.of("jdbc:mysql://localhost...");
    return Optional.empty();
}
// no main: getConnection("outro").orElseThrow(() -> new RuntimeException("Banco não encontrado"));
```
</details>

## Exercícios de Fixação

**Exercício 4:** Qual é a principal diferença entre ArrayList e LinkedList em Java?
a) ArrayList não permite elementos duplicados, enquanto LinkedList permite.
b) ArrayList é mais rápido para buscas por índice, enquanto LinkedList é mais rápido para inserções/remoções frequentes nas extremidades.
c) ArrayList não mantém a ordem de inserção, enquanto LinkedList mantém.
d) ArrayList é sincronizado, enquanto LinkedList não é.

<details>
<summary>Ver Resposta</summary>

**Resposta:** b) ArrayList é mais rápido para buscas por índice, enquanto LinkedList é mais rápido para inserções/remoções frequentes nas extremidades.

**Explicação:** O ArrayList usa um array contíguo (bom para acesso por índice), enquanto a LinkedList usa uma estrutura encadeada (rápida para manipular as pontas).
</details>

**Exercício 4.1:** Ao utilizar a classe Optional, qual método deve ser preferido para evitar um NullPointerException caso o valor não esteja presente?
a) get()
b) orElse()
c) isPresent()
d) stream()

<details>
<summary>Ver Resposta</summary>

**Resposta:** b) orElse()

**Explicação:** O método orElse() fornece um valor padrão caso o Optional esteja vazio, evitando a exceção. O get() lança exceção se o valor estiver vazio e deve ser evitado ou usado junto com isPresent().
</details>

## Referências

[1] Oracle Docs. Collections Framework. Disponível em: [URL](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/package-summary.html)
[2] Oracle Docs. Stream API. Disponível em: [URL](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html)

---

[Voltar aos Links Rápidos](../README.md#links-rapidos) | [Próximo módulo: Módulo 05 — Tratamento de Erros e Tópicos Avançados](./modulo_05_tratamento_de_erros_e_topicos_avancados.md)
