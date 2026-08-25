# Aula 2.4: Polimorfismo: Múltiplas Formas, Uma Interface 🎭

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O Conceito de Polimorfismo](#1-o-conceito-de-polimorfismo)
  - [2. Polimorfismo em Tempo de Compilação (Sobrecarga)](#2-polimorfismo-em-tempo-de-compilao-sobrecarga)
  - [3. Polimorfismo em Tempo de Execução (Sobrescrita)](#3-polimorfismo-em-tempo-de-execuo-sobrescrita)
  - [4. Vantagens do Polimorfismo](#4-vantagens-do-polimorfismo)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Introdução à Orientação a Objetos**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o conceito de polimorfismo como um dos pilares da OOP.
- Entender como o polimorfismo permite que objetos de diferentes classes sejam tratados como objetos de uma classe comum.
- Aplicar polimorfismo em métodos e parâmetros.
- Conhecer a diferença entre polimorfismo em tempo de compilação (sobrecarga) e em tempo de execução (sobrescrita).

##  Conteúdo da Aula

O **polimorfismo** (do grego, "muitas formas") é um dos conceitos mais poderosos da Programação Orientada a Objetos. Ele permite que objetos de diferentes classes, que compartilham uma superclasse ou implementam uma interface comum, sejam tratados de forma genérica como objetos desse tipo comum. Isso significa que um único nome de método pode ter diferentes implementações dependendo do objeto que o invoca.

### 1. O Conceito de Polimorfismo

Imagine que você tem uma lista de `Animal`s. Cada animal pode fazer um som diferente (`latir`, `miar`, `grasnar`). Com polimorfismo, você pode simplesmente chamar o método `fazerSom()` em cada `Animal` da lista, e o Java se encarregará de chamar a implementação correta para cada tipo específico de animal (Cachorro, Gato, Pato), sem que você precise saber o tipo exato de cada um em tempo de compilação.

O polimorfismo é fortemente baseado na **herança** e na **sobrescrita de métodos**.

```java
// Reutilizando as classes Animal, Cachorro, Gato e Pato da aula anterior
class Animal {
    String nome;

    public Animal(String nome) {
        this.nome = nome;
    }

    public void fazerSom() {
        System.out.println(nome + " faz algum som.");
    }
}

class Cachorro extends Animal {
    public Cachorro(String nome) {
        super(nome);
    }

    @Override
    public void fazerSom() {
        System.out.println(nome + " está latindo: Au Au!");
    }
}

class Gato extends Animal {
    public Gato(String nome) {
        super(nome);
    }

    @Override
    public void fazerSom() {
        System.out.println(nome + " está miando: Miau!");
    }
}

class Pato extends Animal {
    public Pato(String nome) {
        super(nome);
    }

    @Override
    public void fazerSom() {
        System.out.println(nome + " está grasnando: Quack Quack!");
    }
}

public class TestePolimorfismo {
    public static void main(String[] args) {
        // Criando objetos de diferentes tipos, mas referenciando-os como Animal
        Animal animal1 = new Cachorro("Buddy");
        Animal animal2 = new Gato("Whiskers");
        Animal animal3 = new Pato("Daffy");
        Animal animal4 = new Animal("Bicho Generico");

        // Chamando o método fazerSom() em cada objeto
        // O método correto é invocado em tempo de execução (Dynamic Method Dispatch)
        animal1.fazerSom(); // Saída: Buddy está latindo: Au Au!
        animal2.fazerSom(); // Saída: Whiskers está miando: Miau!
        animal3.fazerSom(); // Saída: Daffy está grasnando: Quack Quack!
        animal4.fazerSom(); // Saída: Bicho Generico faz algum som.

        System.out.println("\n--- Polimorfismo em uma lista ---");
        // Uma lista de Animais pode conter Cachorros, Gatos, Patos, etc.
        Animal[] animais = new Animal[4];
        animais[0] = new Cachorro("Max");
        animais[1] = new Gato("Mittens");
        animais[2] = new Pato("Pato Donald");
        animais[3] = new Animal("Outro Bicho");

        for (Animal a : animais) {
            a.fazerSom(); // Cada objeto invoca sua própria implementação de fazerSom()
        }
    }
}
```

### 2. Polimorfismo em Tempo de Compilação (Sobrecarga)

Embora a sobrecarga de métodos (abordada na Aula 1.5) seja tecnicamente uma forma de polimorfismo (o mesmo nome de método com diferentes comportamentos), ela é resolvida em tempo de compilação. O compilador decide qual método chamar com base na assinatura (número e tipo de parâmetros).

### 3. Polimorfismo em Tempo de Execução (Sobrescrita)

Este é o tipo de polimorfismo mais comumente referido e é o que vimos no exemplo acima. A decisão de qual método sobrescrito será executado é feita em tempo de execução, com base no tipo real do objeto, e não no tipo da referência. Isso é conhecido como **Dynamic Method Dispatch**.

### 4. Vantagens do Polimorfismo

*   **Flexibilidade e Extensibilidade**: Facilita a adição de novos tipos de objetos sem a necessidade de modificar o código existente que os utiliza de forma genérica.
*   **Reutilização de Código**: Permite escrever código mais genérico e reutilizável, que pode operar em uma variedade de tipos de objetos.
*   **Manutenibilidade**: Reduz a complexidade e torna o código mais fácil de manter, pois as implementações específicas são encapsuladas dentro de suas respectivas classes.

##  Desafio de Fixação

1.  Reutilize a superclasse `Veiculo` e as subclasses `Carro` e `Moto` da Aula 2.3.
2.  Crie uma nova classe `Caminhao` que também herde de `Veiculo`. Adicione um atributo `capacidadeCarga` (double) e sobrescreva o método `ligar()` para imprimir "O caminhão [marca] [ano] com [capacidadeCarga] toneladas de carga está ligando com barulho pesado!".
3.  No método `main`, crie um `ArrayList` de `Veiculo`s.
4.  Adicione instâncias de `Carro`, `Moto` e `Caminhao` a este `ArrayList`.
5.  Percorra o `ArrayList` e chame o método `ligar()` para cada `Veiculo`. Observe como o polimorfismo garante que o método `ligar()` correto seja invocado para cada tipo de veículo.


## Referências

[1] Oracle. Object-Oriented Concepts. Disponível em: [https://docs.oracle.com/javase/tutorial/java/concepts/](https://docs.oracle.com/javase/tutorial/java/concepts/)
[2] Oracle. Inheritance. Disponível em: [https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_2_3_heranca.md) | [Próxima aula/módulo](aula_2_5_interfaces_abstratas.md)
