# Aula 2.3: Herança: Reutilizando Código com Inteligência 🌳

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O Conceito de Herança](#1-o-conceito-de-herana)
  - [2. Construtores na Herança (super())](#2-construtores-na-herana-super)
  - [3. Sobrescrita de Métodos (@Override)](#3-sobrescrita-de-mtodos-override)
  - [4. A Palavra-chave final na Herança](#4-a-palavra-chave-final-na-herana)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Introdução à Orientação a Objetos**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o conceito de herança como um dos pilares da OOP.
- Aprender a criar classes filhas (subclasses) que herdam atributos e métodos de classes pai (superclasses).
- Utilizar a palavra-chave `extends` para estabelecer relacionamentos de herança.
- Entender o conceito de sobrescrita de métodos (`@Override`).
- Conhecer a importância da herança para a reutilização de código e a criação de hierarquias de classes.

##  Conteúdo da Aula

A **herança** é um mecanismo fundamental da Programação Orientada a Objetos que permite que uma classe (chamada **subclasse** ou classe filha) herde características (atributos) e comportamentos (métodos) de outra classe (chamada **superclasse** ou classe pai). Isso promove a **reutilização de código** e a criação de uma hierarquia natural entre as classes, modelando relações do tipo "é um/uma" (e.g., "Um Carro é um Veículo", "Um Cachorro é um Animal").

### 1. O Conceito de Herança

Imagine que você tem uma classe `Animal` com atributos como `nome` e `idade`, e métodos como `comer()` e `dormir()`. Em vez de reescrever esses atributos e métodos para cada tipo específico de animal (Cachorro, Gato, Pássaro), você pode fazer com que `Cachorro`, `Gato` e `Pássaro` herdem de `Animal`. Assim, eles automaticamente terão essas características e comportamentos, e você só precisará adicionar o que é específico de cada um (e.g., `latir()` para `Cachorro`).

Em Java, usamos a palavra-chave `extends` para indicar que uma classe herda de outra.

```java
// Superclasse (Classe Pai)
class Animal {
    String nome;
    int idade;

    public Animal(String nome, int idade) {
        this.nome = nome;
        this.idade = idade;
    }

    public void comer() {
        System.out.println(nome + " está comendo.");
    }

    public void dormir() {
        System.out.println(nome + " está dormindo.");
    }
}

// Subclasse (Classe Filha) que herda de Animal
class Cachorro extends Animal {
    String raca;

    public Cachorro(String nome, int idade, String raca) {
        super(nome, idade); // Chama o construtor da superclasse
        this.raca = raca;
    }

    public void latir() {
        System.out.println(nome + " está latindo: Au Au!");
    }
}

public class TesteHeranca {
    public static void main(String[] args) {
        Animal animalGenerico = new Animal("Bicho", 5);
        animalGenerico.comer();
        animalGenerico.dormir();

        Cachorro meuCachorro = new Cachorro("Rex", 3, "Labrador");
        System.out.println("Nome do cachorro: " + meuCachorro.nome);
        System.out.println("Idade do cachorro: " + meuCachorro.idade);
        System.out.println("Raça do cachorro: " + meuCachorro.raca);
        meuCachorro.comer();   // Método herdado de Animal
        meuCachorro.dormir();  // Método herdado de Animal
        meuCachorro.latir();   // Método específico de Cachorro
    }
}
```

### 2. Construtores na Herança (`super()`)

Quando uma subclasse é instanciada, o construtor da superclasse é sempre chamado primeiro. Você pode chamar explicitamente o construtor da superclasse usando a palavra-chave `super()` como a primeira instrução no construtor da subclasse. Se você não chamar `super()` explicitamente, o Java tentará chamar o construtor padrão (sem argumentos) da superclasse.

### 3. Sobrescrita de Métodos (`@Override`)

Uma subclasse pode fornecer sua própria implementação para um método que já é definido em sua superclasse. Isso é chamado de **sobrescrita de método (method overriding)**. Para indicar que você está sobrescrevendo um método, é uma boa prática usar a anotação `@Override` antes da declaração do método. Isso ajuda o compilador a verificar se o método realmente existe na superclasse e tem a mesma assinatura (nome e parâmetros).

```java
class AnimalComOverride {
    String nome;

    public AnimalComOverride(String nome) {
        this.nome = nome;
    }

    public void fazerSom() {
        System.out.println(nome + " faz algum som.");
    }
}

class Gato extends AnimalComOverride {
    public Gato(String nome) {
        super(nome);
    }

    @Override // Indica que este método sobrescreve um método da superclasse
    public void fazerSom() {
        System.out.println(nome + " está miando: Miau!");
    }
}

class Pato extends AnimalComOverride {
    public Pato(String nome) {
        super(nome);
    }

    @Override
    public void fazerSom() {
        System.out.println(nome + " está grasnando: Quack Quack!");
    }
}

public class TesteOverride {
    public static void main(String[] args) {
        AnimalComOverride animal = new AnimalComOverride("Animal Genérico");
        animal.fazerSom(); // Saída: Animal Genérico faz algum som.

        Gato gato = new Gato("Felix");
        gato.fazerSom(); // Saída: Felix está miando: Miau!

        Pato pato = new Pato("Donald");
        pato.fazerSom(); // Saída: Donald está grasnando: Quack Quack!
    }
}
```

### 4. A Palavra-chave `final` na Herança

*   **`final` em classes**: Uma classe declarada como `final` não pode ser herdada. Isso impede que outras classes estendam essa classe.
*   **`final` em métodos**: Um método declarado como `final` não pode ser sobrescrito por subclasses.

##  Desafio de Fixação

1.  Crie uma superclasse `Veiculo` com os atributos `marca` (String) e `ano` (int), e um método `ligar()` que imprima "[Marca] [Ano] está ligando...".
2.  Crie uma subclasse `Carro` que herde de `Veiculo`. Adicione um atributo `portas` (int) e um método `abrirPortas()` que imprima "Abrindo [portas] portas do [marca] [ano].".
3.  Crie uma subclasse `Moto` que herde de `Veiculo`. Adicione um atributo `cilindradas` (int) e sobrescreva o método `ligar()` para imprimir "A moto [marca] [ano] com [cilindradas] cilindradas está ligando com ronco forte!".
4.  No método `main`, crie um objeto `Carro` e um objeto `Moto`, e chame seus respectivos métodos `ligar()` e os métodos específicos de cada subclasse.


## Referências

[1] Oracle. Object-Oriented Concepts. Disponível em: [https://docs.oracle.com/javase/tutorial/java/concepts/](https://docs.oracle.com/javase/tutorial/java/concepts/)
[2] Oracle. Inheritance. Disponível em: [https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_2_2_encapsulamento.md) | [Próxima aula/módulo](aula_2_4_polimorfismo.md)
