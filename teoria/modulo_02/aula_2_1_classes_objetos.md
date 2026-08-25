# Aula 2.1: Classes e Objetos: O DNA do seu Programa 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O que é uma Classe?](#1-o-que--uma-classe)
  - [2. O que é um Objeto?](#2-o-que--um-objeto)
  - [3. Construtores](#3-construtores)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Introdução à Orientação a Objetos**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender os conceitos de classe e objeto na programação orientada a objetos.
- Aprender a definir classes em Java, incluindo atributos (variáveis de instância) e métodos (comportamentos).
- Criar objetos (instâncias) de classes e acessar seus membros.
- Entender o papel dos construtores na inicialização de objetos.

##  Conteúdo da Aula

Na programação orientada a objetos (OOP), tudo gira em torno de **classes** e **objetos**. Eles são os blocos fundamentais para construir programas que modelam o mundo real de forma mais intuitiva e organizada.

### 1. O que é uma Classe?

Uma **classe** é como um projeto, um molde ou uma planta para criar objetos. Ela define as características (atributos) e os comportamentos (métodos) que os objetos desse tipo terão. Uma classe não é um objeto em si, mas sim a descrição de como um objeto deve ser.

Por exemplo, pense em um carro. A "Classe Carro" definiria que todo carro tem:
*   **Atributos**: cor, marca, modelo, ano, velocidade atual.
*   **Métodos**: ligar, desligar, acelerar, frear.

```java
// Definição da Classe Carro
public class Carro {
    // Atributos (variáveis de instância)
    String cor;
    String marca;
    String modelo;
    int ano;
    double velocidadeAtual;

    // Métodos (comportamentos)
    public void ligar() {
        System.out.println("O carro " + modelo + " está ligado.");
    }

    public void acelerar(double incremento) {
        velocidadeAtual += incremento;
        System.out.println("Acelerando. Velocidade atual: " + velocidadeAtual + " km/h.");
    }

    public void frear(double decremento) {
        velocidadeAtual -= decremento;
        if (velocidadeAtual < 0) {
            velocidadeAtual = 0;
        }
        System.out.println("Freando. Velocidade atual: " + velocidadeAtual + " km/h.");
    }

    public void desligar() {
        velocidadeAtual = 0;
        System.out.println("O carro " + modelo + " está desligado.");
    }
}
```

### 2. O que é um Objeto?

Um **objeto** é uma instância concreta de uma classe. É a materialização do projeto. Você pode criar vários objetos a partir da mesma classe, e cada objeto terá seus próprios valores para os atributos, mas compartilharão os mesmos métodos.

Continuando com o exemplo do carro:
*   Um objeto seria "Meu Carro Azul" (uma instância específica da Classe Carro).
*   Outro objeto seria "Carro Vermelho do Vizinho" (outra instância).

```java
public class TesteCarro {
    public static void main(String[] args) {
        // Criando um objeto (instância) da Classe Carro
        Carro meuCarro = new Carro();

        // Atribuindo valores aos atributos do objeto meuCarro
        meuCarro.cor = "Azul";
        meuCarro.marca = "Toyota";
        meuCarro.modelo = "Corolla";
        meuCarro.ano = 2022;
        meuCarro.velocidadeAtual = 0;

        // Acessando atributos e chamando métodos do objeto meuCarro
        System.out.println("Meu carro é um " + meuCarro.marca + " " + meuCarro.modelo + " de cor " + meuCarro.cor);
        meuCarro.ligar();
        meuCarro.acelerar(50);
        meuCarro.frear(10);
        meuCarro.desligar();

        // Criando outro objeto da mesma Classe Carro
        Carro carroDoVizinho = new Carro();
        carroDoVizinho.cor = "Vermelho";
        carroDoVizinho.marca = "Honda";
        carroDoVizinho.modelo = "Civic";
        carroDoVizinho.ano = 2023;

        System.out.println("\nO carro do vizinho é um " + carroDoVizinho.marca + " " + carroDoVizinho.modelo + " de cor " + carroDoVizinho.cor);
        carroDoVizinho.ligar();
    }
}
```

### 3. Construtores

Um **construtor** é um método especial usado para inicializar um objeto recém-criado. Ele tem o mesmo nome da classe e não possui tipo de retorno (nem mesmo `void`). Se você não definir nenhum construtor, Java fornece um construtor padrão (sem argumentos) implicitamente.

Podemos definir nossos próprios construtores para garantir que os objetos sejam criados em um estado válido.

```java
public class CarroComConstrutor {
    String cor;
    String marca;
    String modelo;
    int ano;
    double velocidadeAtual;

    // Construtor padrão (sem argumentos) - pode ser omitido se houver outros construtores
    public CarroComConstrutor() {
        this.velocidadeAtual = 0; // Inicializa a velocidade em 0 por padrão
    }

    // Construtor com argumentos para inicializar os atributos
    public CarroComConstrutor(String cor, String marca, String modelo, int ano) {
        this.cor = cor; // 'this' refere-se ao atributo da instância
        this.marca = marca;
        this.modelo = modelo;
        this.ano = ano;
        this.velocidadeAtual = 0; // Atributo não passado, inicializado por padrão
    }

    // Métodos (acelerar, frear, etc. seriam os mesmos da classe Carro anterior)
    public void ligar() {
        System.out.println("O carro " + modelo + " está ligado.");
    }

    public static void main(String[] args) {
        // Usando o construtor padrão
        CarroComConstrutor carroPadrao = new CarroComConstrutor();
        carroPadrao.modelo = "Fusca";
        carroPadrao.ligar();

        // Usando o construtor com argumentos
        CarroComConstrutor novoCarro = new CarroComConstrutor("Preto", "Ford", "Ka", 2020);
        System.out.println("Novo carro: " + novoCarro.marca + " " + novoCarro.modelo + " de cor " + novoCarro.cor);
        novoCarro.ligar();
    }
}
```

##  Desafio de Fixação

1.  Crie uma classe `Pessoa` com os atributos `nome` (String), `idade` (int) e `altura` (double).
2.  Adicione um construtor à classe `Pessoa` que receba `nome`, `idade` e `altura` como parâmetros.
3.  Adicione um método `apresentar()` à classe `Pessoa` que imprima uma mensagem como: "Olá, meu nome é [nome], tenho [idade] anos e [altura]m de altura."
4.  No método `main` de uma classe `TestePessoa`, crie dois objetos da classe `Pessoa` com dados diferentes e chame o método `apresentar()` para cada um.


## Referências

[1] Oracle. Object-Oriented Concepts. Disponível em: [https://docs.oracle.com/javase/tutorial/java/concepts/](https://docs.oracle.com/javase/tutorial/java/concepts/)
[2] Oracle. Inheritance. Disponível em: [https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](../modulo_02_orientacao_a_objetos.md) | [Próxima aula/módulo](aula_2_2_encapsulamento.md)
