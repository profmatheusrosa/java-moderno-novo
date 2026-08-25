# Aula 5.2: Generics: Código Flexível e Seguro em Tipos 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O Problema Antes dos Generics](#1-o-problema-antes-dos-generics)
  - [2. Introdução aos Generics](#2-introduo-aos-generics)
  - [3. Vantagens dos Generics](#3-vantagens-dos-generics)
  - [4. Bounded Type Parameters (Parâmetros de Tipo Delimitados)](#4-bounded-type-parameters-parmetros-de-tipo-delimitados)
  - [5. Wildcards (?)](#5-wildcards-)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Tratamento de Erros e Tópicos Avançados**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o conceito de Generics em Java e sua importância.
- Aprender a utilizar classes e interfaces genéricas.
- Entender como os Generics promovem a reutilização de código e a segurança de tipos em tempo de compilação.
- Conhecer as Wildcards (`?`) e suas aplicações (`? extends T`, `? super T`).

##  Conteúdo da Aula

Antes dos **Generics**, introduzidos no Java 5, a reutilização de código para diferentes tipos de dados era frequentemente feita usando a classe `Object`. Isso levava a problemas de segurança de tipo, pois o compilador não podia verificar o tipo correto em tempo de compilação, resultando em `ClassCastException` em tempo de execução. Os Generics resolveram esse problema, permitindo que você escreva classes, interfaces e métodos que operam em tipos parametrizados, garantindo a segurança de tipo em tempo de compilação.

### 1. O Problema Antes dos Generics

Considere uma classe simples para armazenar um único item:

```java
public class CaixaObjeto {
    private Object item;

    public void setItem(Object item) {
        this.item = item;
    }

    public Object getItem() {
        return item;
    }

    public static void main(String[] args) {
        CaixaObjeto caixaString = new CaixaObjeto();
        caixaString.setItem("Hello Generics");
        String str = (String) caixaString.getItem(); // Cast necessário, propenso a ClassCastException
        System.out.println(str);

        CaixaObjeto caixaInteger = new CaixaObjeto();
        caixaInteger.setItem(123);
        // String outraStr = (String) caixaInteger.getItem(); // ERRO em tempo de execução: ClassCastException
    }
}
```

### 2. Introdução aos Generics

Generics permitem que você defina classes, interfaces e métodos com um ou mais **parâmetros de tipo**. Esses parâmetros atuam como um "placeholder" para o tipo real que será usado quando a classe/interface/método for instanciada ou chamada.

#### Sintaxe Básica:

*   **Classes Genéricas**: `class NomeClasse<T> { ... }`
*   **Interfaces Genéricas**: `interface NomeInterface<T> { ... }`
*   **Métodos Genéricos**: `public <T> T nomeMetodo(T parametro) { ... }`

O `T` (ou qualquer outra letra maiúscula, como `E` para Elemento, `K` para Chave, `V` para Valor) é o parâmetro de tipo.

```java
// Classe Genérica Caixa
public class Caixa<T> {
    private T item;

    public void setItem(T item) {
        this.item = item;
    }

    public T getItem() {
        return item;
    }

    public static void main(String[] args) {
        // Uso com String
        Caixa<String> caixaString = new Caixa<>(); // O tipo é inferido como String
        caixaString.setItem("Hello Generics");
        String str = caixaString.getItem(); // Nenhum cast necessário!
        System.out.println(str);

        // Uso com Integer
        Caixa<Integer> caixaInteger = new Caixa<>();
        caixaInteger.setItem(123);
        Integer num = caixaInteger.getItem(); // Nenhum cast necessário!
        System.out.println(num);

        // caixaString.setItem(456); // ERRO em tempo de compilação! Segurança de tipo garantida.
    }
}
```

### 3. Vantagens dos Generics

*   **Segurança de Tipo em Tempo de Compilação**: Erros de tipo são detectados pelo compilador, não em tempo de execução.
*   **Eliminação de Casts**: Não é mais necessário fazer casts explícitos, tornando o código mais limpo e legível.
*   **Reutilização de Código**: Você pode escrever algoritmos e estruturas de dados que funcionam com qualquer tipo de objeto, sem sacrificar a segurança de tipo.

### 4. Bounded Type Parameters (Parâmetros de Tipo Delimitados)

Às vezes, você quer restringir os tipos que podem ser usados como parâmetros genéricos. Isso é feito usando `extends` (para classes e interfaces) ou `super` (para interfaces).

*   **`<T extends ClasseOuInterface>`**: Indica que `T` deve ser `ClasseOuInterface` ou uma subclasse/implementação dela. Isso permite que você chame métodos de `ClasseOuInterface` em objetos do tipo `T`.

```java
// Exemplo: Método genérico que aceita apenas números
public class CalculadoraGenerica {
    public static <T extends Number> double somarDobro(T num1, T num2) {
        return num1.doubleValue() * 2 + num2.doubleValue() * 2;
    }

    public static void main(String[] args) {
        System.out.println(somarDobro(10, 20)); // Com Integer
        System.out.println(somarDobro(10.5, 20.3)); // Com Double
        // System.out.println(somarDobro("abc", "def")); // ERRO em tempo de compilação!
    }
}
```

### 5. Wildcards (`?`)

Wildcards (`?`) são usados em parâmetros de tipo para representar um tipo desconhecido. Eles são particularmente úteis ao trabalhar com coleções genéricas.

*   **`?` (Unbounded Wildcard)**: Representa qualquer tipo. `List<?>` significa uma lista de tipo desconhecido.
*   **`? extends T` (Upper Bounded Wildcard)**: Representa um tipo desconhecido que é `T` ou uma subclasse de `T`. Usado para leitura (produtor).
*   **`? super T` (Lower Bounded Wildcard)**: Representa um tipo desconhecido que é `T` ou uma superclasse de `T`. Usado para escrita (consumidor).

#### PECS: Producer Extends, Consumer Super

Uma regra mnemônica útil para wildcards:
*   Se você está apenas **lendo** (`Producer`) de uma estrutura genérica, use `extends` (`? extends T`).
*   Se você está apenas **escrevendo** (`Consumer`) para uma estrutura genérica, use `super` (`? super T`).

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class WildcardExemplo {

    // Método que pode imprimir qualquer lista de números (Producer Extends)
    public static void imprimirListaNumeros(List<? extends Number> lista) {
        for (Number n : lista) {
            System.out.println(n);
        }
        // lista.add(new Integer(1)); // ERRO: Não pode adicionar em ? extends Number
    }

    // Método que pode adicionar Integers a uma lista de Integers ou de suas superclasses (Consumer Super)
    public static void adicionarInteiro(List<? super Integer> lista) {
        lista.add(100);
        lista.add(200);
        // Integer i = lista.get(0); // ERRO: Não pode ler com segurança de ? super Integer
    }

    public static void main(String[] args) {
        List<Integer> inteiros = Arrays.asList(1, 2, 3);
        List<Double> doubles = Arrays.asList(1.1, 2.2, 3.3);
        List<Object> objetos = new ArrayList<>();

        System.out.println("Imprimindo lista de Inteiros:");
        imprimirListaNumeros(inteiros);
        System.out.println("Imprimindo lista de Doubles:");
        imprimirListaNumeros(doubles);

        System.out.println("\nAdicionando inteiros a lista de Objetos:");
        adicionarInteiro(objetos);
        System.out.println("Lista de Objetos após adição: " + objetos);

        List<Number> listaDeNumeros = new ArrayList<>();
        adicionarInteiro(listaDeNumeros);
        System.out.println("Lista de Numbers após adição: " + listaDeNumeros);
    }
}
```

##  Desafio de Fixação

1.  Crie uma classe genérica `Pilha<T>` que implemente uma pilha (LIFO - Last In, First Out) simples. Ela deve ter os métodos:
    *   `void push(T item)`: Adiciona um item ao topo da pilha.
    *   `T pop()`: Remove e retorna o item do topo da pilha. Deve lançar uma `IllegalStateException` se a pilha estiver vazia.
    *   `boolean isEmpty()`: Retorna `true` se a pilha estiver vazia, `false` caso contrário.
2.  No método `main`, crie uma `Pilha<String>` e adicione algumas strings. Em seguida, remova e imprima os itens até a pilha ficar vazia.
3.  Crie uma `Pilha<Integer>` e adicione alguns números. Tente remover um item de uma pilha vazia para ver a exceção.
4.  Crie um método genérico `public static <E> void transferir(Pilha<? extends E> origem, Pilha<? super E> destino)` que transfere todos os elementos de uma pilha `origem` para uma pilha `destino`.
5.  Teste o método `transferir` com suas pilhas criadas.


## Referências

[1] Oracle. Virtual Threads. Disponível em: [https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html](https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html)
[2] Oracle. Exceptions Tutorial. Disponível em: [https://docs.oracle.com/javase/tutorial/essential/exceptions/](https://docs.oracle.com/javase/tutorial/essential/exceptions/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_5_1_excecoes.md) | [Próxima aula/módulo](aula_5_3_lambdas.md)
