# Aula 3.1: Records: Classes de Dados Simples e Imutáveis 📦

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O Problema do Código Boilerplate](#1-o-problema-do-cdigo-boilerplate)
  - [2. Introdução aos Records](#2-introduo-aos-records)
  - [3. Vantagens dos Records](#3-vantagens-dos-records)
  - [4. Quando Usar Records?](#4-quando-usar-records)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Java Moderno: Novas Features**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o propósito e os benefícios dos Records em Java.
- Aprender a declarar e utilizar Records para modelar dados imutáveis.
- Entender como os Records simplificam a criação de classes de dados, reduzindo código boilerplate.

##  Conteúdo da Aula

Com a evolução do Java, a linguagem tem buscado oferecer formas mais concisas e expressivas de escrever código. Uma das adições mais significativas nesse sentido são os **Records**, introduzidos como um recurso de preview no Java 14 e padronizados no Java 16. Records são classes especiais projetadas para atuar como **portadores de dados imutáveis**.

### 1. O Problema do Código Boilerplate

Tradicionalmente, para criar uma classe simples que apenas armazena dados (como um DTO - Data Transfer Object), precisávamos escrever muito código repetitivo (boilerplate):

*   Atributos privados.
*   Construtor para inicializar todos os atributos.
*   Métodos *getters* para acessar os atributos.
*   Métodos `equals()`, `hashCode()` e `toString()` para comparação e representação textual.

```java
// Exemplo de classe de dados tradicional (antes dos Records)
public class PontoTradicional {
    private final int x;
    private final int y;

    public PontoTradicional(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() {
        return x;
    }

    public int getY() {
        return y;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        PontoTradicional that = (PontoTradicional) o;
        return x == that.x && y == that.y;
    }

    @Override
    public int hashCode() {
        return java.util.Objects.hash(x, y);
    }

    @Override
    public String toString() {
        return "PontoTradicional[x=" + x + ", y=" + y + "]";
    }
}
```

### 2. Introdução aos Records

Records resolvem o problema do boilerplate, permitindo que você declare classes de dados de forma muito mais concisa. Ao declarar um `record`, o compilador Java gera automaticamente para você:

*   Um construtor canônico (que inicializa todos os componentes).
*   Métodos de acesso para cada componente (com o mesmo nome do componente, sem o prefixo `get`).
*   Implementações de `equals()`, `hashCode()` e `toString()` baseadas em todos os componentes.
*   A classe é implicitamente `final` (não pode ser estendida) e seus componentes são implicitamente `final` (imutáveis).

#### Sintaxe Básica:

`record NomeDoRecord(TipoComponente1 nomeComponente1, TipoComponente2 nomeComponente2, ...) { /* corpo opcional */ }`

```java
// Exemplo de Record (equivalente à classe PontoTradicional acima)
public record Ponto(int x, int y) {
    // Opcional: Você pode adicionar métodos adicionais, construtores compactos ou construtores canônicos customizados
    // Exemplo de método adicional
    public double distanciaDaOrigem() {
        return Math.sqrt(x * x + y * y);
    }

    // Exemplo de construtor compacto (sem lista de parâmetros, apenas validações)
    public Ponto {
        if (x < 0 || y < 0) {
            throw new IllegalArgumentException("Coordenadas não podem ser negativas");
        }
    }
}

public class TesteRecords {
    public static void main(String[] args) {
        Ponto p1 = new Ponto(10, 20);
        Ponto p2 = new Ponto(10, 20);
        Ponto p3 = new Ponto(30, 40);

        System.out.println(p1); // Saída: Ponto[x=10, y=20] (toString gerado automaticamente)
        System.out.println("X de p1: " + p1.x()); // Acesso ao componente x
        System.out.println("Distância da origem de p1: " + p1.distanciaDaOrigem());

        System.out.println("p1 equals p2? " + p1.equals(p2)); // true (equals gerado automaticamente)
        System.out.println("p1 equals p3? " + p1.equals(p3)); // false

        // Tentando criar um Ponto com coordenadas negativas (validação do construtor compacto)
        try {
            new Ponto(-1, 5);
        } catch (IllegalArgumentException e) {
            System.out.println("Erro ao criar Ponto: " + e.getMessage());
        }
    }
}
```

### 3. Vantagens dos Records

*   **Concisão**: Reduz drasticamente a quantidade de código boilerplate.
*   **Imutabilidade**: Por padrão, os componentes de um Record são `final`, o que promove a imutabilidade e facilita o raciocínio sobre o estado dos objetos, especialmente em programação concorrente.
*   **Clareza**: Deixa claro que a principal função da classe é ser um portador de dados.
*   **Segurança**: A imutabilidade ajuda a evitar efeitos colaterais indesejados.

### 4. Quando Usar Records?

Records são ideais para:

*   **DTOs (Data Transfer Objects)**: Objetos usados para transferir dados entre camadas da aplicação.
*   **Value Objects**: Objetos que representam um valor, como um ponto, uma cor, uma coordenada.
*   **Retornos de Métodos**: Quando um método precisa retornar múltiplos valores.
*   **Chaves de Mapas**: Devido à sua imutabilidade e implementações corretas de `equals()` e `hashCode()`.

Records *não* são adequados para classes que precisam de herança, que possuem estado mutável complexo ou que têm comportamentos muito ricos que não se encaixam na ideia de um simples portador de dados.

##  Desafio de Fixação

1.  Crie um `record` chamado `Produto` com os componentes `id` (long), `nome` (String), `preco` (double) e `emEstoque` (boolean).
2.  Adicione um método `public String exibirDetalhes()` ao `record Produto` que retorne uma string formatada com todos os detalhes do produto.
3.  No método `main`, crie duas instâncias do `record Produto` com dados diferentes.
4.  Imprima os detalhes de cada produto usando o método `exibirDetalhes()`.
5.  Verifique se os dois produtos são iguais usando `equals()` e imprima o resultado.


## Referências

[1] OpenJDK. Project Amber. Disponível em: [https://openjdk.org/projects/amber/](https://openjdk.org/projects/amber/)
[2] Oracle. Java Language Changes. Disponível em: [https://docs.oracle.com/en/java/javase/21/language/java-language-changes.html](https://docs.oracle.com/en/java/javase/21/language/java-language-changes.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](../modulo_03_recursos_modernos_do_java.md) | [Próxima aula/módulo](aula_3_2_sealed_classes.md)
