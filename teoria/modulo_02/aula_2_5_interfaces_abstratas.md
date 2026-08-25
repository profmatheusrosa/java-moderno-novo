# Aula 2.5: Interfaces e Classes Abstratas: Contratos e Modelos 🤝

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. Classes Abstratas: Modelos Incompletos](#1-classes-abstratas-modelos-incompletos)
  - [2. Interfaces: Contratos Puros](#2-interfaces-contratos-puros)
  - [3. Quando Usar Classes Abstratas vs. Interfaces?](#3-quando-usar-classes-abstratas-vs-interfaces)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Introdução à Orientação a Objetos**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o conceito de abstração e sua aplicação em Java.
- Diferenciar classes abstratas de interfaces e saber quando usar cada uma.
- Aprender a declarar e implementar classes abstratas e seus métodos.
- Aprender a declarar e implementar interfaces e seus métodos padrão (default) e estáticos.
- Entender como interfaces e classes abstratas contribuem para o design flexível e extensível de sistemas.

##  Conteúdo da Aula

A **abstração** é um dos pilares da Programação Orientada a Objetos, focando na representação das características essenciais de um objeto, ignorando os detalhes irrelevantes. Em Java, a abstração é alcançada principalmente através de **classes abstratas** e **interfaces**.

### 1. Classes Abstratas: Modelos Incompletos

Uma **classe abstrata** é uma classe que não pode ser instanciada diretamente (você não pode criar objetos dela). Ela serve como um modelo (blueprint) para outras classes, fornecendo uma base comum de atributos e métodos, mas deixando alguns métodos sem implementação (métodos abstratos). As subclasses são então responsáveis por fornecer a implementação concreta desses métodos abstratos.

#### Características de Classes Abstratas:
*   Declarada com a palavra-chave `abstract`.
*   Pode ter métodos abstratos (sem corpo, apenas assinatura) e métodos concretos (com implementação).
*   Pode ter construtores, mas eles só podem ser chamados por subclasses (usando `super()`).
*   Uma classe que contém pelo menos um método abstrato *deve* ser declarada como abstrata.
*   Uma subclasse de uma classe abstrata *deve* implementar todos os métodos abstratos da superclasse, a menos que ela própria seja abstrata.

```java
// Classe Abstrata Veiculo (reutilizando e adaptando da aula anterior)
abstract class Veiculo {
    String marca;
    int ano;

    public Veiculo(String marca, int ano) {
        this.marca = marca;
        this.ano = ano;
    }

    // Método concreto
    public void exibirDetalhes() {
        System.out.println("Marca: " + marca + ", Ano: " + ano);
    }

    // Método abstrato (sem implementação) - subclasses devem implementá-lo
    public abstract void ligar();
}

class Carro extends Veiculo {
    public Carro(String marca, int ano) {
        super(marca, ano);
    }

    @Override
    public void ligar() {
        System.out.println("O carro " + marca + " " + ano + " está ligando com a chave.");
    }
}

class Moto extends Veiculo {
    public Moto(String marca, int ano) {
        super(marca, ano);
    }

    @Override
    public void ligar() {
        System.out.println("A moto " + marca + " " + ano + " está ligando com o botão de partida.");
    }
}

public class TesteClassesAbstratas {
    public static void main(String[] args) {
        // Veiculo v = new Veiculo("Generico", 2000); // ERRO: Não pode instanciar classe abstrata

        Carro meuCarro = new Carro("Honda", 2023);
        meuCarro.exibirDetalhes();
        meuCarro.ligar();

        Moto minhaMoto = new Moto("Yamaha", 2022);
        minhaMoto.exibirDetalhes();
        minhaMoto.ligar();
    }
}
```

### 2. Interfaces: Contratos Puros

Uma **interface** em Java é um "contrato" que define um conjunto de métodos que uma classe deve implementar. Ela especifica "o que" uma classe deve fazer, mas não "como" ela deve fazer. Antes do Java 8, interfaces só podiam ter métodos abstratos (implicitamente `public abstract`) e constantes (implicitamente `public static final`).

#### Características de Interfaces (Java 8+):
*   Declarada com a palavra-chave `interface`.
*   Pode ter métodos abstratos (sem corpo).
*   Pode ter métodos `default` (com implementação padrão), que podem ser sobrescritos pelas classes que implementam a interface.
*   Pode ter métodos `static` (com implementação), que pertencem à interface e não podem ser sobrescritos.
*   Pode ter constantes (`public static final`).
*   Uma classe pode implementar múltiplas interfaces (resolvendo o problema da herança múltipla).
*   Uma classe que implementa uma interface *deve* implementar todos os seus métodos abstratos.

```java
// Interface para definir comportamento de um objeto que pode ser ligado/desligado
interface Ligavel {
    void ligar(); // Método abstrato (implicitamente public abstract)
    void desligar(); // Método abstrato

    // Método default (com implementação padrão)
    default void verificarStatus() {
        System.out.println("Verificando status do dispositivo...");
    }

    // Método estático
    static void exibirInstrucoes() {
        System.out.println("Para operar, ligue e desligue o dispositivo.");
    }
}

class Lampada implements Ligavel {
    private boolean ligada;

    @Override
    public void ligar() {
        ligada = true;
        System.out.println("Lâmpada ligada.");
    }

    @Override
    public void desligar() {
        ligada = false;
        System.out.println("Lâmpada desligada.");
    }

    // Opcional: sobrescrever o método default
    @Override
    public void verificarStatus() {
        System.out.println("Status da Lâmpada: " + (ligada ? "Ligada" : "Desligada"));
    }
}

class Computador implements Ligavel {
    @Override
    public void ligar() {
        System.out.println("Computador inicializando...");
    }

    @Override
    public void desligar() {
        System.out.println("Computador sendo desligado.");
    }
}

public class TesteInterfaces {
    public static void main(String[] args) {
        Lampada lampada = new Lampada();
        lampada.ligar();
        lampada.verificarStatus();
        lampada.desligar();

        System.out.println("\n---\n");

        Computador computador = new Computador();
        computador.ligar();
        computador.verificarStatus(); // Usa a implementação default da interface
        computador.desligar();

        System.out.println("\n---\n");
        Ligavel.exibirInstrucoes(); // Chamando método estático da interface
    }
}
```

### 3. Quando Usar Classes Abstratas vs. Interfaces?

| Característica        | Classe Abstrata                                   | Interface                                         |
| :-------------------- | :------------------------------------------------ | :------------------------------------------------ |
| **Tipo de Relacionamento** | "É um/uma" (Herança)                              | "Pode fazer" (Contrato de Comportamento)          |
| **Implementação**     | Pode ter métodos abstratos e concretos            | Antes do Java 8: Somente métodos abstratos. Java 8+: Métodos abstratos, default e static. |
| **Variáveis**         | Pode ter variáveis de instância (não-finais)      | Somente constantes (`public static final`)         |
| **Construtores**      | Pode ter construtores                             | Não pode ter construtores                         |
| **Herança Múltipla**  | Java não suporta herança múltipla de classes      | Uma classe pode implementar múltiplas interfaces  |
| **Modificador**       | `abstract`                                        | `interface`                                       |

*   Use **classes abstratas** quando você tem uma relação "é um/uma" forte e quer fornecer uma implementação base comum para subclasses, mas ainda deixar alguns detalhes para elas implementarem. Ideal para quando você quer compartilhar código entre classes relacionadas.
*   Use **interfaces** quando você quer definir um contrato de comportamento que classes *podem* implementar, independentemente de sua hierarquia de herança. Ideal para quando você quer definir capacidades que classes diversas podem ter.

##  Desafio de Fixação

1.  Crie uma interface `FormaGeometrica` com os métodos abstratos `calcularArea()` e `calcularPerimetro()`, ambos retornando `double`.
2.  Crie uma classe `Circulo` que implemente `FormaGeometrica`. Adicione um atributo `raio` (double) e implemente os métodos `calcularArea()` e `calcularPerimetro()`.
3.  Crie uma classe `Retangulo` que implemente `FormaGeometrica`. Adicione atributos `largura` (double) e `altura` (double) e implemente os métodos `calcularArea()` e `calcularPerimetro()`.
4.  No método `main`, crie objetos `Circulo` e `Retangulo`, e chame seus métodos para calcular e imprimir a área e o perímetro de cada um.


## Referências

[1] Oracle. Object-Oriented Concepts. Disponível em: [https://docs.oracle.com/javase/tutorial/java/concepts/](https://docs.oracle.com/javase/tutorial/java/concepts/)
[2] Oracle. Inheritance. Disponível em: [https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_2_4_polimorfismo.md) | [Próxima aula/módulo](../modulo_03_recursos_modernos_do_java.md)
